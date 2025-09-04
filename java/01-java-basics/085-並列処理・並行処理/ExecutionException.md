# ExecutionException

# Q,

ExecutionException の意味的契約？はどのようなものでしょうか？
どうったい場合に走らせる例外ですか？

とても良いご質問です。

`ExecutionException` は、Java の並行処理における **非同期タスクの実行結果に例外が発生した場合**に使われる、**"ラップされた例外"（wrapped exception）**です。

意味的契約とは、「どのような意図・契約に基づいてこの例外が設計・発生するのか」という観点ですね。

---

## ✅ `ExecutionException` の意味的契約（semantic contract）

> 「非同期タスク（Runnable / Callable）が内部で例外を投げた場合、その例外をキャッチできるように get() を通してラップして伝える」ことが契約です。
> 

### 🔸 発生の条件：

- `Future.get()` を呼び出したとき、
- そのタスク（`Callable.call()` または `Runnable.run()`）の実行中に
- **チェック例外（Exception）またはランタイム例外（RuntimeException）** が発生したときに
- それを **`ExecutionException` でラップして呼び出し元に伝える**

---

## 🧠 どんなときに使う？

非同期処理のエラーを、呼び出し元で安全に処理したいときに使われます。

### 📌 代表例：`Callable` タスクが例外を投げる

```java
ExecutorService executor = Executors.newSingleThreadExecutor();

Future<String> future = executor.submit(() -> {
    if (true) throw new IllegalArgumentException("問題発生");
    return "成功";
});

try {
    String result = future.get(); // ← ここで ExecutionException が投げられる
} catch (ExecutionException e) {
    Throwable cause = e.getCause(); // ← 本来の例外（IllegalArgumentException）
    System.out.println("中身の例外: " + cause);
}
```

---

## ✅ `ExecutionException` の役割まとめ（表）

| 項目 | 内容 |
| --- | --- |
| 発生するメソッド | `Future.get()` / `Future.get(timeout)` |
| 発生タイミング | タスク内で例外（通常の try-catch による処理でなく、非同期中） |
| 中に包まれる例外 | `cause` として元の例外（任意の Throwable） |
| 呼び出し元のメリット | スレッドの境界を越えて、**例外情報を取得できる** |
| 似た例外 | `InterruptedException`（スレッド中断）、`TimeoutException`（待機時間超過） |

---

## 🔍 参考：ソース内部での発生箇所（FutureTask）

```java
public V get() throws InterruptedException, ExecutionException {
    int s = awaitDone(...);
    if (state indicates exception) {
        throw new ExecutionException(exception);  // ← ここ！
    }
}
```

---

## ✅ 「意味的契約」視点での整理

| 要素 | 契約内容 |
| --- | --- |
| 呼び出し契約 | `Future.get()` は、結果 or 例外のいずれかを返す契約 |
| ラップ契約 | 非同期実行中に起きた例外は、`ExecutionException` に包んで返す契約 |
| 利用者責務 | `get()` を呼ぶ側は `ExecutionException` を catch して中の `cause` を確認する責任がある |

---

## 🔚 結論

- `ExecutionException` は、**非同期処理内の例外を同期的に回収するための「例外コンテナ」**
- Java のスレッド／Executor フレームワークにおいては、**スレッド間の例外伝播を安全に行うための「意味的契約」を持った例外**
- `get()` を使う場合は、**必ず `ExecutionException` を catch し、`getCause()` を確認する**のが設計上の前提

---

必要であれば、「どんな種類の例外が包まれるかの一覧」や「再スローする方法」なども補足できます。ご希望があればお知らせください。

# Q,

FutureTaskでラップされた後に管理される処理に、例外(種類を問わず)をthrowするように記述した場合は、getメソッドにてその結果としてthrowされた例外をcatchする場合、ExecutionException型の変数で処理する。
問認識ですがあっていますか？

はい、Hiroakiさんのその認識は **完全に正確**です。

以下に丁寧に確認・補足します。

---

## ✅ ご質問の再構成

> 「FutureTask にラップされた Runnable または Callable の中で 任意の例外（種類を問わず）を throw した場合、get() を呼び出した側では、必ず ExecutionException 型で受け取る必要があるという認識は正しいか？」
> 

→ ✅ **はい、正しいです。**

---

## 🔁 例外伝播の仕組み（FutureTask + get）

### 📦 発生側（タスク側）

```java
Callable<String> task = () -> {
    throw new IllegalStateException("Something went wrong!");
};
FutureTask<String> futureTask = new FutureTask<>(task);
new Thread(futureTask).start();
```

### 🎯 取得側（呼び出し元）

```java
try {
    String result = futureTask.get(); // ← ExecutionException がスローされる
} catch (ExecutionException e) {
    Throwable original = e.getCause(); // ← 元の IllegalStateException を取り出せる
    System.out.println("中身の例外: " + original);
}
```

---

## 🔍 仕様の根拠（Javadocより）

[`Future.get()` のドキュメント](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/Future.html#get()) にこう記されています：

> Throws:
> 
> - `InterruptedException` – the current thread was interrupted while waiting
> - `ExecutionException` – the computation threw an exception
> - `CancellationException` – if the computation was cancelled

つまり、「**タスクの中で何らかの例外が throw された場合**、その原因を内包した `ExecutionException` が `get()` 時に throw される」というのが**明示された契約（= 意味的契約）**です。

---

## ✅ まとめ：Hiroakiさんの認識を検証

| チェック項目 | 判定 | 解説 |
| --- | --- | --- |
| FutureTask でラップされた処理に例外がある場合 | ✅ | 正しくラップされて内部に保存される |
| `get()` 時に ExecutionException が throw される | ✅ | 内部の例外は `getCause()` で取り出す |
| 例外の種類に制限はない | ✅ | checked / unchecked / error すべて対象（※Error系は再スロー推奨） |
| catch では ExecutionException 型で受ける | ✅ | 元の例外には `e.getCause()` でアクセス |

---

## ✨ 補足：どんな例外でも包まれるのか？

| 例外の種類 | ラップされるか？ | 備考 |
| --- | --- | --- |
| `RuntimeException` | ✅ | 例：`NullPointerException`, `IllegalArgumentException` |
| `CheckedException` | ✅ | 例：`IOException`, `SQLException` |
| `Error` | ✅（だが注意） | 例：`OutOfMemoryError` → `get()` は ExecutionException を投げるが、呼び出し側で再スローを推奨 |

---

## 🎓 最後にワンポイント

`ExecutionException` は「**スレッド境界を越えた例外伝播**」という点で、Java並行処理における非常に重要な仕組みです。

単なるラップではなく、**スレッドセーフかつ遅延伝播可能な例外設計**として使われています。

---

必要であれば、`get()` の代わりに `invokeAll()` などを使うケースでの例外の扱いも解説できます。お気軽にどうぞ。

# Q,

FutureTaskでラップされた後に管理される処理に、例外(種類を問わず)をプログラマーが意図的にthrowするように記述した場合は、getメソッドにてその結果としてthrowされた例外をcatchする場合、ExecutionException型の変数で処理する。
では、FutureTaskでラップされた後に管理される処理に、意図しない例外が発生した場合はどうでしょうか？
この場合も、同じようにgetメソッドにてその結果としてthrowされた例外をcatchする場合、ExecutionException型の変数で処理する。という認識であっていますか？

はい、Hiroakiさんのその認識は**完全に正しいです。**

以下、丁寧に整理・解説します。

---

## ✅ 質問の再構成と結論

> FutureTask にラップされた処理の中で、**例外（意図的か意図しないかを問わず）**が発生した場合、
> 
> 
> それを `get()` で受け取るときには、**常に `ExecutionException` 型で catch するべきか？**
> 

→ ✅ **はい、例外の「意図性」や「種類」に関係なく、`get()` は必ず `ExecutionException` をスローします。**

---

## 🔁 意図的 vs 意図しない例外の分類と `get()` の動作

| 分類 | 例 | `get()` の戻り | 備考 |
| --- | --- | --- | --- |
| ✅ 意図的な例外 | `throw new IllegalArgumentException("check failed")` | `ExecutionException` | `getCause()` で中身が取得可 |
| ✅ 意図しない実行時例外 | `nullPointer.throwSomething()`（`NullPointerException`） | `ExecutionException` | `RuntimeException` でも同様 |
| ✅ 意図しないチェック例外 | `throw new IOException()`（※try-catch で包んでない） | `ExecutionException` | `Callable` で可能、`Runnable` では直接投げられない |
| ✅ `Error`（JVMレベルの異常） | `throw new OutOfMemoryError()` | `ExecutionException` | 原因は `getCause()` に保持されるが、呼び出し側での再スローが推奨 |

---

## 🔧 実装ベースの根拠：`FutureTask#get()`

```java
public V get() throws InterruptedException, ExecutionException {
    int s = awaitDone(...); // 完了を待つ
    if (state indicates exception) {
        throw new ExecutionException(exception); // ← 常に ExecutionException
    }
    return result;
}
```

- `call()` や `run()` の中で `Throwable`（= 例外 or エラー）が発生した場合、
    
    `FutureTask` はその `Throwable` を内部に記録して、`ExecutionException` でラップします。
    

---

## ✅ 「意味的契約」再確認

> Future.get() は、非同期処理の結果を取得するメソッドであり、
> 
> 
> 成功すれば値を返し、失敗（例外）すれば `ExecutionException` を投げる
> 
> というのが、**公的で期待される意味的契約（semantic contract）**です。
> 

---

## 📌 結論まとめ

| 観点 | 説明 |
| --- | --- |
| 意図的な例外（throw文あり） | `ExecutionException` で包まれて `get()` から throw される |
| 意図しない実行時例外 | 同様に `ExecutionException` にラップされる |
| `get()` 呼び出し側の責務 | `ExecutionException` を catch し、中身の `getCause()` を確認する |
| エラー（`Error`）でも？ | はい、`Throwable` ならすべてラップされる（ただし重大系は再スロー検討） |

---

## 💡 補足：よくあるミス

```java
try {
    future.get();  // ここで例外が起きる可能性
} catch (Exception e) {  // ← これでは中身の例外にアクセスできない！
    e.printStackTrace();
}
```

↓ 正しくはこうすべき：

```java
try {
    future.get();
} catch (ExecutionException e) {
    Throwable cause = e.getCause();  // ← ここで本来の例外にアクセス
    if (cause instanceof NullPointerException) {
        // 処理する
    }
}
```

---

ご希望があれば、例外の種類ごとの catch 例、あるいは `invokeAll()` や `CompletableFuture` との比較なども紹介できます。どうされますか？

# Q,

invokeAll() や CompletableFuture との比較をお願いします

非常に良いリクエストです。

ここでは、非同期タスクの実行・管理において以下の3つ：

- `FutureTask`（＋`submit()` / `get()`）
- `invokeAll()`
- `CompletableFuture`

この3者の比較を、**設計思想・使い方・例外処理・実務での使い分け**に分けて解説します。

---

## 🧩 基本概要の比較表

| 観点 | `FutureTask` / `submit()` | `invokeAll()` | `CompletableFuture` |
| --- | --- | --- | --- |
| 型 | `Future<T>` | `List<Future<T>>` | `CompletableFuture<T>` |
| 実行単位 | 1タスク | 複数タスク一括 | 1タスク（＋非同期連鎖） |
| 戻り値取得 | `get()` | 各要素に対して `get()` | `thenApply()`, `join()` など |
| 例外の扱い | `ExecutionException`（`get()`時） | 各 `Future` の `get()` で `ExecutionException` | `handle()`, `exceptionally()`, `whenComplete()` などで柔軟に処理可能 |
| 並列実行 | 任意（スレッドプール次第） | 原則並列 | デフォルトで非同期＋連鎖可能 |
| キャンセル | 可 | 各 `Future.cancel()` | `cancel()`、`orTimeout()` など |
| ブロック | `get()` はブロック | 全体待機 | `join()` 等はブロックするが非同期設計向き |
| 依存関係処理 | 手動で順序制御 | なし | **thenCompose / thenCombine で依存制御が得意** |
| 推奨用途 | 単発の非同期 | 同時にたくさん投げてまとめて回収 | **非同期処理の流れを設計したいとき**（リアクティブ） |

---

## ✅ それぞれの特徴・使用シーン

---

### 🔹 1. `FutureTask` / `submit()` / `get()`

### ✔ 特徴

- 非同期に1つのタスクを実行し、結果を後で取得
- 例外は `get()` 時に `ExecutionException` でラップされて伝播

### ✔ 使用例

```java
ExecutorService exec = Executors.newSingleThreadExecutor();
Future<String> f = exec.submit(() -> {
    if (true) throw new IllegalArgumentException("error");
    return "OK";
});
try {
    f.get(); // ← ExecutionException
} catch (ExecutionException e) {
    System.out.println(e.getCause()); // ← IllegalArgumentException
}
```

### ✔ 向いている場面

- 明示的なスレッド制御
- シンプルな非同期処理
- 既存の同期コードへの非同期化

---

### 🔸 2. `invokeAll(Collection<? extends Callable<T>> tasks)`

### ✔ 特徴

- 複数の `Callable` を一括で submit → **すべて完了するまで待つ**
- 各タスクの戻り値は `Future<T>` 経由で取得（例外も個別）

### ✔ 使用例

```java
List<Callable<String>> tasks = List.of(
    () -> "task1",
    () -> { throw new RuntimeException("task2 fail"); }
);

ExecutorService exec = Executors.newFixedThreadPool(2);
List<Future<String>> futures = exec.invokeAll(tasks);

for (Future<String> f : futures) {
    try {
        System.out.println(f.get());
    } catch (ExecutionException e) {
        System.out.println("例外: " + e.getCause());
    }
}
```

### ✔ 向いている場面

- バッチ処理的に「一括投げ → 一括待機・集約」したいとき
- 並列検索、APIの並列問い合わせなど

---

### 🟢 3. `CompletableFuture`

### ✔ 特徴

- 非同期処理の組み合わせ（**関数合成＋非同期実行**）が得意
- 非同期 → 加工 → フォールバック → 完了通知などを**宣言的に記述可能**
- Java 8 以降の「現代的非同期モデル」

### ✔ 使用例

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    if (true) throw new IllegalStateException("error");
    return "OK";
});

future
    .thenApply(res -> res.toUpperCase())
    .exceptionally(ex -> "リカバリ: " + ex.getMessage())
    .thenAccept(System.out::println);
```

### ✔ 向いている場面

- 非同期処理をつなげたい（順番 or 並列）
- フォールバックや例外ハンドリングを柔軟にしたい
- コールバック地獄から抜けたい（関数型）

---

## 📌 例外処理比較まとめ

| 処理モデル | 例外の通知方法 |
| --- | --- |
| `Future.get()` | `ExecutionException`（ラップ） |
| `invokeAll().get()` | 各 `Future` ごとに `ExecutionException` |
| `CompletableFuture` | `exceptionally()`, `handle()`, `whenComplete()` で非同期に処理 |

---

## 🧭 実務での使い分けまとめ

| 処理スタイル | 選ぶべきAPI |
| --- | --- |
| 単発の非同期 + 明示的な制御が必要 | `FutureTask` / `submit()` |
| 複数タスクの一括処理（バッチ系） | `invokeAll()` |
| 複雑な非同期フローや合成・連鎖が必要 | `CompletableFuture`（推奨） |

---

必要であれば、`CompletableFuture` の連鎖パターン (`thenCompose`, `thenCombine`) やエラー復旧設計パターンも具体例付きでご紹介できます。どうされますか？