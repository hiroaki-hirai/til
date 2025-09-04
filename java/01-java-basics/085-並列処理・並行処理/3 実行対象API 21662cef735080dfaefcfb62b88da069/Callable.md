# Callable

Javaの並列・非同期処理で重要な`Callable`インタフェースについて、以下のように丁寧に解説します。

---

## ✅ Callableインターフェイスとは？

### 🔹 定義

`Callable<V>` は Java で **戻り値あり & 例外スロー可能なタスク** を表す**関数型インターフェース**です。

```java
@FunctionalInterface
public interface Callable<V> {
    V call() throws Exception;
}
```

---

## ✅ 特徴（Runnableとの違い）

| 比較項目 | `Runnable` | `Callable<V>` |
| --- | --- | --- |
| 戻り値 | なし (`void run()`) | あり (`V call()`) |
| 例外 | チェック例外を投げられない | `throws Exception` 可 |
| スレッド実行 | `Thread`, `ExecutorService` | `ExecutorService.submit()` 専用 |
| 使いどころ | 処理だけしたいとき | 処理の**結果**が欲しいとき |

---

## ✅ 基本使用例（ExecutorServiceと組み合わせる）

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Callable<String> task = () -> {
            Thread.sleep(1000);  // 疑似的な重い処理
            return "Finished!";
        };

        Future<String> future = executor.submit(task);

        System.out.println("Waiting...");
        String result = future.get();  // 結果を待機（ブロッキング）
        System.out.println(result);

        executor.shutdown();
    }
}
```

---

## ✅ ポイント解説

| 機能 | 内容 |
| --- | --- |
| `Callable<T>` | `call()` メソッドが `T` 型の戻り値を返す |
| `Future<T>` | 非同期タスクの結果を管理するラッパー |
| `submit(Callable)` | `ExecutorService` 経由で実行。戻り値は `Future<T>` |
| `future.get()` | 戻り値を取得（※待機するのでブロッキング） |
| `Exception` 対応 | `call()` はチェック例外をスローできる → try-catch or throws が必要 |

---

## ✅ 実務的な使いどころ

| シーン | 説明 |
| --- | --- |
| Web API呼び出し | API結果を非同期で取得したいとき（結果が必要） |
| 並列データ処理 | 複数の`Callable`を使ってデータを並列集計 |
| 複雑な処理のリトライ制御 | 例外スローで`try-catch`による明確なエラー制御が可能 |

---

## ✅ 複数Callableの並列処理（invokeAll）

```java
List<Callable<String>> tasks = List.of(
    () -> "A", () -> "B", () -> "C"
);

ExecutorService executor = Executors.newFixedThreadPool(3);
List<Future<String>> results = executor.invokeAll(tasks);

for (Future<String> future : results) {
    System.out.println(future.get());
}
executor.shutdown();
```

---

## ✅ まとめ

| 概要 | 説明 |
| --- | --- |
| `Callable<V>` | 戻り値あり、例外スロー可能な関数型インターフェース |
| 主な用途 | 並列・非同期処理でのタスク定義（`Future`と併用） |
| 比較 | `Runnable`より高機能（戻り値 + 例外） |

---

必要であれば、`Runnable`との統一的な使い方や、`CompletableFuture`との併用例などもご紹介できます。興味ありますか？

# Q,

Callable実装は、Runnable実装とは異なり、戻り値を予定している点が大きな違いという認識はあっていますか？

はい、Hiroakiさん、

その認識は**完全に正しいです。**

---

## ✅ 結論：

> ✔️ Callableは、Runnableとは異なり「戻り値を返すことが前提」になっている点が最も大きな違いです。
> 

この違いにより、**タスクの結果を非同期に受け取る設計**が可能になります。

---

## ✅ Runnable vs Callable の比較表

| 比較項目 | `Runnable` | `Callable<V>` |
| --- | --- | --- |
| 戻り値 | ❌ 返せない (`void run()`) | ✅ 返せる (`V call()`) |
| 例外スロー | ❌ チェック例外は投げられない | ✅ `throws Exception` 可能 |
| 実行メソッド | `void run()` | `V call()` |
| 使用される場面 | 「処理だけ」必要な非同期タスク | 「処理＋結果」必要な非同期タスク |
| 結果の取得方法 | 結果がないため受け取らない | `Future.get()` などで受け取れる |
| 主な使用場所 | `Thread`, `Executor.execute()` | `ExecutorService.submit()` |

---

## ✅ 具体例で違いを確認

### 🔸 Runnable の例（戻り値なし）

```java
Runnable task = () -> System.out.println("処理中...");
new Thread(task).start();
```

- 実行されるが、**結果は戻ってこない**。

---

### 🔸 Callable の例（戻り値あり）

```java
Callable<String> task = () -> {
    Thread.sleep(1000);
    return "完了しました";
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(task);

String result = future.get();  // ★ 結果を受け取れる！
System.out.println(result);    // → "完了しました"
executor.shutdown();
```

---

## ✅ まとめ

| 要点 | 内容 |
| --- | --- |
| 最大の違い | `Runnable`は戻り値なし、`Callable`は戻り値あり |
| 戻り値を必要とする場面で | `Callable`を使い、`Future`で結果を取得 |
| 両者の役割 | `Runnable`: 副作用系のタスク / `Callable`: 結果を持つ処理 |

---

もしご希望であれば、「CallableとFutureの例外処理のコツ」や「複数Callableの同時実行（`invokeAll()`）」も解説できます！

# Q,

Callable実装にてその具象クラスにて唯一のメソッドの処理結果として戻ってきた値を受け取る役割が、Futureクラス内のgetメソッド。
ほかにFutureクラスの役割はありますか？

はい、Hiroakiさん、

とても良い視点です。

おっしゃる通り、`Callable` の戻り値を受け取るのが `Future.get()` の代表的な役割ですが、**`Future` の本当の価値は「戻り値の取得」だけでなく、「非同期タスクの**状態制御・管理」**にあります。**

---

## ✅ `Future` の主な役割

| 機能カテゴリ | メソッド | 内容・役割 |
| --- | --- | --- |
| 🔹 戻り値の取得 | `get()` | 終了まで**待機して**戻り値を取得（ブロッキング） |
|  | `get(timeout, unit)` | 指定時間だけ待って結果を取得（タイムアウトつき） |
| 🔹 タスクの状態確認 | `isDone()` | タスクが完了したかどうかを確認 |
|  | `isCancelled()` | キャンセルされたかどうかを確認 |
| 🔹 タスクのキャンセル | `cancel(boolean mayInterrupt)` | 実行中のタスクを**キャンセル要求**（オプションで割り込み可能） |

---

## ✅ それぞれの用途と使いどころ

### ① `get()` / `get(timeout, unit)`：結果を受け取る

- 非同期タスクの結果を取得
- `Callable.call()` の戻り値がここに届く
- **`get()` はブロックされる**ので、完了していない場合は待たされる

---

### ② `isDone()`：完了確認（非ブロッキング）

- 処理が終わっているかを確認する（終わっていれば `true`）
- `get()` で待つのではなく、**ポーリング（状態監視）で使う**

```java
if (future.isDone()) {
    System.out.println("完了しています！");
}
```

---

### ③ `cancel()`：キャンセル機能

```java
future.cancel(true);  // trueなら割り込みも試みる
```

- 実行前なら → **実行されずにキャンセル**
- 実行中なら → 割り込みがかかるかも（スレッド側が対応していれば）

---

### ④ `isCancelled()`：キャンセルされたか確認

- `cancel()` 呼び出し後に `true` を返す
- 終了状態を精査する場合に使う

---

## ✅ 実務的な価値まとめ

| シナリオ | 使用メソッド | 意味 |
| --- | --- | --- |
| タスクの完了待ち＋結果取得 | `get()` | 完了まで待って戻り値を得る |
| 非同期の進捗確認（UI連携など） | `isDone()` | 完了しているかポーリングできる |
| ユーザーが途中でキャンセル | `cancel(true)` + `isCancelled()` | 実行中の処理を安全に止める（ただし割り込みが必要） |

---

## ✅ まとめ

### Futureの役割まとめ

---

✅Callableの**戻り値の受け取り（get）**

---

✅**非同期処理の進行状況の監視**（isDone）

---

✅**タスクの中断・キャンセル制御**（cancel, isCancelled）

---

→ 「結果を待つ」だけでなく「制御もできるオブジェクト」

---

さらに拡張された制御が必要であれば、**`FutureTask`**や**`CompletableFuture`**もご紹介できます！

ご興味があればぜひお声がけください。