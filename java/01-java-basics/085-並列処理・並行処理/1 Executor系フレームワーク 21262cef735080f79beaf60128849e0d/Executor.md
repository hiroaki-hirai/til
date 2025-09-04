# Executor

`Executor`はJavaの**並行処理（concurrency）を抽象化**するための**インタフェース**で、`java.util.concurrent`パッケージに含まれています。

---

## 🔷 `Executor`インタフェースの基本

```java
public interface Executor {
    void execute(Runnable command);
}
```

---

## 🔹 1. `execute(Runnable command)` の解説

- **関数インタフェースではありません。**
    - `@FunctionalInterface`ではないため、ラムダ式やメソッド参照で直接使う目的のインタフェースではありません（ただし、メソッドが1つなので実質的には可能です）。
- **引数は `Runnable` 型のみ**で、`Callable` などは使えません（※Callableを使いたい場合は`ExecutorService`を使います）。
- **戻り値なし（`void`）**
    - タスクの完了状態や結果を取得する手段は提供されません。

---

## 🔹 2. `Runnable`実装でのみ使用？

はい、**`Executor`は`Runnable`専用**のインタフェースです。

```java
Executor executor = command -> {
    new Thread(command).start(); // 実装例：毎回新しいスレッドで実行
};

executor.execute(() -> System.out.println("Hello from a separate thread"));
```

---

## 🔹 3. `Executor` の役割・位置づけ

| 観点 | 内容 |
| --- | --- |
| ✅ 目的 | タスクの実行戦略（スレッドの生成・管理）を分離して、呼び出し元に意識させないようにすること |
| 🎯 使いどころ | スレッドの生成処理（`new Thread(...)`）をアプリの各所にベタ書きしないための設計 |
| 🔧 柔軟性 | 同じ `Runnable` を、 - 新しいスレッドで実行 - スレッドプールで実行 - 同期実行 など自由に切り替え可能 |
| 📦 実装例 | `ExecutorService`, `ThreadPoolExecutor`, `ScheduledExecutorService` などが `Executor` を実装 |

---

## 🔹 4. 補足：`ExecutorService`との違い

| インタフェース名 | 主な用途 | 機能 |
| --- | --- | --- |
| `Executor` | Runnableタスクの実行 | `execute()` |
| `ExecutorService` | タスクの管理・制御 | `submit()`, `invokeAll()`, `shutdown()` など |

---

## 🔚 まとめ

- `Executor` は **Runnable専用のタスク実行インタフェース**
- **関数インタフェースではないが、実質1メソッドなのでラムダでも使える**
- タスクの「**実行戦略の切り替え**」を柔軟にするための設計上の抽象
- 実務では、`ExecutorService` などの **より高機能な派生インタフェース**を利用するケースが多いです

---

ご希望あれば、`Executor` を使ったカスタム実装例や、`ExecutorService` との実務比較などもご用意できます。

# Q,

ThreadクラスのstartメソッドとRunnable実装による具象クラスの注入をラップして、エントリーポイントにてそれぞれのスレッドの開始の順番を決めるように実行するようなイメージでしょうか？

はい、Hiroakiさんのイメージは非常に的確です。

`Executor`インタフェースやその実装は、**`Thread#start()`の直接使用と`Runnable`の実装を"ラップ・抽象化"して、スレッドの開始方法を統一的に管理する仕組み**です。

以下、わかりやすく構造・目的・メリットを図式的にまとめます。

---

## 🔷 `Thread` + `Runnable` の従来のスタイル

```java
Runnable task = new MyTask();
Thread thread = new Thread(task);
thread.start();
```

- `Thread`のインスタンス化と起動を**呼び出し側が直接行う**
- 起動順序や管理を**手動で制御**
- **コードがスレッドに強く依存**し、抽象化が難しい

---

## 🔷 `Executor` によるラッピング・委譲の形

```java
public class Main {
    public static void main(String[] args) {
        Executor executor = command -> new Thread(command).start(); // ★ラップ

        executor.execute(new TaskA());
        executor.execute(new TaskB());
    }
}
```

- `Runnable`の実装を `execute()` に渡すだけでよくなる（注入）
- 呼び出し側が`Thread`を意識しなくてよい
- **実行戦略（startする順番や方法）を Executor 実装に委譲**

---

## 🔷 「順番を決める」ための工夫

例えば、複数の`Runnable`実装クラスを並べて順番に実行したり、戦略的に制御できます：

```java
List<Runnable> tasks = List.of(new TaskA(), new TaskB(), new TaskC());

Executor executor = command -> new Thread(command).start();

for (Runnable task : tasks) {
    executor.execute(task); // 順番制御はここで可能
}
```

または **ExecutorService（スレッドプール）** を使えば、**同時実行数を制御**したり、**終了待ち処理**を記述したりもできます。

---

## ✅ 整理すると：

| 比較軸 | 従来のThread方式 | Executor方式 |
| --- | --- | --- |
| 実行戦略の制御 | 呼び出し側が明示的に記述 | Executor側に委譲・集中 |
| 柔軟性 | 少ない（固定） | 高い（戦略差し替え可能） |
| テスト性・拡張性 | 低い | 高い（DIや戦略パターンに親和） |
| 目的 | 即時のスレッド起動 | 実行管理の一元化・抽象化 |

---

## 🧩 DIやStrategyパターンとの親和性

`Executor` は「スレッド実行の戦略」を注入可能にしているので、

- **Strategyパターン的**
- **DI（依存性注入）との親和性が高い**

というのも、実行方法を呼び出し元が決めるのではなく、**実行する仕組みを別クラスに任せる**（＝疎結合）設計だからです。

---

必要であれば、下記のような観点でも展開できます：

- `ExecutorService`との違いと進化系
- 複数のRunnableを順番に（直列 or 並列）実行する構造例
- 戦略パターンとExecutorの関連
- スレッドの優先度・キャンセル制御との関連

どれか気になる視点があれば、深堀りします！