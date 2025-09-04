# Runnable→Callable変換

# Q,

Runnableの具象クラスがFutureTaskにラップされた後にgetメソッドで取り出すとnullが出力されるという部分の定義はどこにありますか？
FutureTaskのgetメソッド内でしょうか？

非常に良い質問です。

`Runnable` を `FutureTask` にラップして実行し、`get()` を呼び出したときに `null` が返る理由と、**その定義箇所がどこか**をはっきりさせましょう。

---

## ✅ 結論：**`FutureTask#get()` ではなく、`Runnable` をラップする段階で `null` が戻り値として定義されている**

### 具体的には：

> Runnable を FutureTask に渡すとき、戻り値は null として明示的に設定されます。
> 
> 
> その定義は **`AbstractExecutorService.newTaskFor(Runnable, result)` メソッド内** です。
> 

---

## 🔍 どこで `null` が設定されているのか？

```java
protected <T> RunnableFuture<T> newTaskFor(Runnable runnable, T value) {
    return new FutureTask<T>(runnable, value);
}
```

- `ExecutorService#submit(Runnable)` を呼ぶと、内部的に `newTaskFor(runnable, null)` が呼ばれる。
- `null` は **Runnable 実行後の戻り値** として `FutureTask` に渡される。

---

## 🔧 `FutureTask` の該当コンストラクタ

```java
public FutureTask(Runnable runnable, V result) {
    this.callable = Executors.callable(runnable, result);
}
```

この `Executors.callable(...)` が `Runnable` を `Callable` に変換し、戻り値（ここでは `null`）を持つようにします。

---

## 🧠 最終的な get() の返却値の定義場所

```java
public V get() throws InterruptedException, ExecutionException {
    // 内部で状態待ちと完了値取得
    return report(getDoneValue());
}
```

ここで `getDoneValue()` は、処理完了後に保持された `result` を返すだけなので、

**もともと `null` を渡していればそのまま `null` を返します。**

---

## ✅ 結論まとめ

| 項目 | 内容 |
| --- | --- |
| `Runnable` を `submit()` で実行後 `get()` すると `null` が返る理由 | 実行結果（戻り値）として最初から `null` が設定されているため |
| その設定箇所 | `AbstractExecutorService.newTaskFor(Runnable, null)` |
| 実際の保持処理 | `FutureTask` の `result` フィールドに `null` がセットされる |

---

## ✨ 補足コード（確認用）

```java
Runnable task = () -> System.out.println("Hello");
ExecutorService exec = Executors.newSingleThreadExecutor();
Future<?> f = exec.submit(task);
System.out.println(f.get()); // → null
```

→ これは **Runnable自体が戻り値を持たない**ため、**nullが明示的に設定されている**ことの証拠です。

---

必要であれば、`Executors.callable(...)` や `RunnableAdapter.call()` の内部まで辿って説明可能です。ご希望があればお知らせください。

Q,

下記のコードは、FutureTaskのrunメソッドですが、callメソッドが呼ばれない場合はresult = null になりますが、set(result)にならず、例外処理が走るようです。
上記のように明示的にnullが変数に代入され取得できるようになるコード箇所はありますか？

```java
    public void run() {
        if (state != NEW ||
            !RUNNER.compareAndSet(this, null, Thread.currentThread()))
            return;
        try {
            Callable<V> c = callable;
            if (c != null && state == NEW) {
                V result;
                boolean ran;
                try {
                    result = c.call();
                    ran = true;
                } catch (Throwable ex) {
                    result = null;
                    ran = false;
                    setException(ex);
                }
                if (ran)
                    set(result);
            }
        } finally {
            // runner must be non-null until state is settled to
            // prevent concurrent calls to run()
            runner = null;
            // state must be re-read after nulling runner to prevent
            // leaked interrupts
            int s = state;
            if (s >= INTERRUPTING)
                handlePossibleCancellationInterrupt(s);
        }
    }
```

はい、鋭い観察です。

ご提示いただいたのは `FutureTask#run()` の実装コードですが、**`null` が明示的に `result` としてセットされる箇所**がどこなのか、という疑問に対して、以下のように正確にお答えします。

---

## ✅ 質問の要点整理

> call() が正常に呼ばれなかった or 例外が発生した場合に get() で返る値（result）が null になることがあるが、その null はどこで代入・保持されているのか？
> 

---

## 🔍 該当コードの整理：正常系 vs 異常系

```java
try {
    result = c.call();   // ← Callable 実行（正常に終われば戻り値が result に入る）
    ran = true;
} catch (Throwable ex) {
    result = null;       // ← ★ここで明示的に null をセット
    ran = false;
    setException(ex);    // ← 例外を内部に保持
}
if (ran)
    set(result);         // ← 正常終了時のみ result を確定
```

---

## ✅ ポイント解説

- `set(result)` は正常に `call()` が終了したときだけ呼ばれる。
- 例外が発生した場合は、`setException(ex)` が呼ばれ、`get()` は `ExecutionException` を投げるため、**null は返らない。**
- `null` が `get()` で返るのは、**`Runnable` の場合に `FutureTask` の `result` に最初から null を渡した場合**。

---

## 🔁 「明示的に `null` を result に保持」するケース

これは実は `FutureTask(Runnable, V)` コンストラクタで設定されるケースが本質です：

### ▶ `FutureTask(Runnable runnable, V result)` コンストラクタ

```java
public FutureTask(Runnable runnable, V result) {
    this.callable = Executors.callable(runnable, result);
}
```

### ▶ `Executors.callable(...)` の中身（RunnableAdapter）

```java
static final class RunnableAdapter<T> implements Callable<T> {
    final Runnable task;
    final T result;
    RunnableAdapter(Runnable task, T result) {
        this.task = task;
        this.result = result;
    }
    public T call() {
        task.run();
        return result;  // ← ★ここで null を返すよう設計されている！
    }
}
```

### → つまり：

- `Runnable` を `submit()` した場合、**内部で `null` が `result` としてセットされた `FutureTask` が生成される**。
- その `call()` が `run()` 内で呼ばれ、`set(result)` が実行され、**内部 `result` に `null` が保存される**。

---

## ✅ 結論：`get()` で `null` が返るのはこの条件下

| 条件 | 説明 |
| --- | --- |
| `Runnable` を `ExecutorService.submit()` で送信 | 実行内容は `Runnable` のため戻り値なし |
| `AbstractExecutorService.submit()` が `newTaskFor(runnable, null)` を呼ぶ | 明示的に `null` が result としてセットされる |
| `FutureTask` に `null` が設定される | `RunnableAdapter` の `call()` が `null` を返す |
| `FutureTask#run()` の中で `set(null)` が呼ばれる | これにより `get()` は `null` を返す |

---

## 🔚 補足：例外系の場合は `null` にはならない

例外が起きた場合：

```java
catch (Throwable ex) {
    result = null;
    ran = false;
    setException(ex); // ← ここで例外が記録される
}
```

→ `get()` は `ExecutionException` を投げるため、`null` は返りません。

---

## ✔️ まとめ

| 観点 | 結論 |
| --- | --- |
| 明示的に `null` がセットされる箇所 | `RunnableAdapter.call()` の戻り値として |
| `FutureTask#run()` 内で `set(null)` が呼ばれる条件 | Runnable 実行後の result が null |
| `get()` が `null` を返す唯一の正当ケース | Runnable を submit した場合（正常終了） |

---

必要であれば `RunnableAdapter` の全体像や `Executors.callable(...)` の別パターンなども併せて紹介できます。どうぞ遠慮なくお知らせください。

# Q,

Executorsクラスに下記のcallableメソッドのオーバーロードがありますが、これが関係していますか？

```java
    public static <T> Callable<T> callable(Runnable task, T result) {
        if (task == null)
            throw new NullPointerException();
        return new RunnableAdapter<T>(task, result);
    }

    public static Callable<Object> callable(Runnable task) {
        if (task == null)
            throw new NullPointerException();
        return new RunnableAdapter<Object>(task, null);
    }
```

はい、**まさにこの `Executors.callable(...)` のオーバーロードが、`Runnable` 実行後に `Future.get()` で `null` が返る仕組みの** **中心的な関係箇所です。**

以下、仕組みと流れを丁寧に説明します。

---

## ✅ どちらの `Executors.callable(...)` も `Runnable` を `Callable` に変換するためのメソッド

```java
// 戻り値を明示的に指定するバージョン
public static <T> Callable<T> callable(Runnable task, T result) {
    return new RunnableAdapter<T>(task, result); // ← resultは任意
}

// 戻り値を指定しないバージョン（戻り値＝null）
public static Callable<Object> callable(Runnable task) {
    return new RunnableAdapter<Object>(task, null); // ← resultがnullで固定
}
```

- これらはどちらも `RunnableAdapter<T>` のインスタンスを返します。
- `RunnableAdapter` は内部的に `Callable` を実装しており、`task.run()` 実行後に `result` を返します。

---

## 🔁 `RunnableAdapter<T>` の中身（簡略）

```java
static final class RunnableAdapter<T> implements Callable<T> {
    final Runnable task;
    final T result;

    RunnableAdapter(Runnable task, T result) {
        this.task = task;
        this.result = result;
    }

    public T call() {
        task.run();
        return result; // ← ここが null の正体
    }
}
```

---

## 🔄 `FutureTask(Runnable)` にどうつながるか

```java
// submit(Runnable) の内部
RunnableFuture<Void> ftask = newTaskFor(task, null);

// AbstractExecutorService より
protected <T> RunnableFuture<T> newTaskFor(Runnable runnable, T value) {
    return new FutureTask<T>(runnable, value);
}

// FutureTask コンストラクタ
public FutureTask(Runnable runnable, V result) {
    this.callable = Executors.callable(runnable, result); // ← ここでこの話につながる！
}
```

---

## ✅ 結論：`Executors.callable(Runnable)` が関係している点の要約

| ポイント | 説明 |
| --- | --- |
| `submit(Runnable)` は `Runnable` を `FutureTask` にラップする際、`Executors.callable()` を使って `Callable` に変換する | ✅ |
| 戻り値が必要ないため、`Executors.callable(Runnable)` が呼ばれ、暗黙的に `null` が戻り値として設定される | ✅ |
| 結果、`RunnableAdapter.call()` の戻り値が `null` → `FutureTask.set(null)` に保存され → `get()` で `null` が返る | ✅ |

---

## 📌 補足

### `submit(Runnable)` から `get()` の全体フロー：

```java
submit(Runnable)                         ← ExecutorService.submit()
↓
newTaskFor(runnable, null)              ← AbstractExecutorService
↓
new FutureTask(runnable, null)
↓
Executors.callable(runnable, null)
↓
RunnableAdapter.call() 実行（戻り値 null）
↓
FutureTask.set(null)
↓
Future.get() → null が返る
```

---

## ✅ まとめ

| 質問 | 回答 |
| --- | --- |
| Executors.callable のオーバーロードは `null` 戻り値と関係しているか？ | **はい。直接的に関係しています。** |
| どのコードで `null` が明示的に渡されている？ | `Executors.callable(Runnable)` の `result = null` が鍵です |
| `get()` が `null` を返すのはどのような流れ？ | `RunnableAdapter.call()` → `null` → `set(null)` → `get()` |

---

この理解をベースにすれば、Runnable vs Callable の扱い方や `submit()` と `execute()` の違いも深く納得できるようになります。必要であれば、違いの比較表なども作成できます。