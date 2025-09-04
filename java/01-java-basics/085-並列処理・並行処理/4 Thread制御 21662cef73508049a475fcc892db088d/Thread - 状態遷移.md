# Thread - 状態遷移

# Q,

Thread 状態遷移（NEW → RUNNABLE → TERMINATED）について解説してください。

はい、`Thread` の状態遷移は Java のスレッド制御の中でも非常に重要な基礎知識です。

ここでは、**スレッドライフサイクル（Thread Lifecycle）として、代表的な状態とその遷移トリガー**を図解イメージ＋解説でまとめます。

---

## 🔷 1. 【図解】スレッドの状態遷移（主要な5状態）

```java
 ┌──────────────┐
 │   NEW        │
 └────┬─────────┘
      │ start()
      ▼
 ┌──────────────┐
 │ RUNNABLE     │  ←────┐
 └────┬─────────┘       │
      │（OSによって       │ notify() / notifyAll()
      │ 実行可能と判断） ▼
      ▼             ┌──────────────┐
 ┌──────────────┐   │  BLOCKED     │
 │ RUNNING      │   └──────────────┘
 └────┬─────────┘        ▲
      │                 │ synchronized lock取得失敗
      │sleep(), wait(), │
      │join()           │
      ▼                 │
 ┌──────────────┐       │
 │ WAITING       │──────┘
 └────┬─────────┘
      │ join(timeout), sleep(timeout), wait(timeout)
      ▼
 ┌──────────────┐
 │ TIMED_WAITING│
 └────┬─────────┘
      │ タイムアウト、完了
      ▼
 ┌──────────────┐
 │ TERMINATED   │
 └──────────────┘
```

---

## 🔷 2. 各スレッド状態の意味とトリガー

### ✅ `NEW`

- スレッドが生成された直後（`new Thread()` した状態）。
- **まだ `start()` されていない**ので、OSスレッドも割り当てられていない。

```java
Thread t = new Thread(); // NEW 状態
```

---

### ✅ `RUNNABLE`

- `start()` により、スレッドが **実行可能状態** に入る。
- *実行中（RUNNING）か、CPU待ち（READY）**の状態をまとめて `RUNNABLE` と呼ぶ（JVMレベルでは区別しない）。

```java
t.start(); // RUNNABLE に遷移
```

---

### ✅ `RUNNING`

- OSによって CPU が割り当てられ、実際に `run()` メソッドの実行中。
- Java API では `RUNNING` 状態は `RUNNABLE` に含まれる（区別しない）。

---

### ✅ `BLOCKED`

- `synchronized` によるロックを取得できず、**ブロックされている状態**。
- 他スレッドがロックを持っており、解放されるまで待機。

---

### ✅ `WAITING`

- `wait()` / `join()`（タイムアウトなし）などで、**明示的に無期限停止**。
- 外部から `notify()` または `join()` の対象スレッドが終了しない限り復帰しない。

---

### ✅ `TIMED_WAITING`

- `sleep(ms)`、`wait(ms)`、`join(ms)` など、**一定時間だけ待機**。
- 時間経過後に自動で `RUNNABLE` に戻る。

---

### ✅ `TERMINATED`

- スレッドの `run()` メソッドが終了した状態。
- `正常終了`または`例外終了`のどちらでもこの状態に移行。

```java
System.out.println(t.getState()); // → TERMINATED
```

---

## 🔷 3. 状態確認方法

```java
Thread t = new Thread(() -> { /* 処理 */ });
System.out.println(t.getState()); // NEW
t.start();
System.out.println(t.getState()); // RUNNABLE など
```

---

## ✅ 状態まとめ表

| 状態 | 説明 | 移行のきっかけ |
| --- | --- | --- |
| `NEW` | 生成済みだが未起動 | `new Thread()` |
| `RUNNABLE` | 実行可能な状態（RUNNING含む） | `start()`、待機解除 |
| `BLOCKED` | 他のスレッドが保持するロックを待っている | `synchronized` ブロック進入時にロック取得失敗 |
| `WAITING` | 無期限待機 | `wait()`、`join()` |
| `TIMED_WAITING` | 一定時間の待機 | `sleep()`, `wait(ms)`, `join(ms)` |
| `TERMINATED` | 実行完了または例外終了 | `run()` 完了、例外スロー |

---

## 🔚 補足：実務で重要な見どころ

- **RUNNABLE状態 = 常にCPUを使ってるわけではない**（CPU割り当てはOSが決める）。
- デッドロックなどは、`BLOCKED` でずっと止まっている状態で検出できることもあります。

---

必要であれば、`Thread.State` enum を使った監視ツールコードや、デッドロック検出パターンも紹介できます！

# Q,

Thread.sleep(5000);
sleepメソッドは、スレッドすべてが５ｓ間待機する認識はあっていますか？

Hiroakiさんのご質問に対する結論は：

> ❌ スレッドすべてが待機するわけではありません。
> 
> 
> ✅ **`Thread.sleep(5000)` を呼び出した**「**そのスレッドだけが**」**5秒間待機（ブロック）**します。
> 

---

## ✅ 解説：`Thread.sleep()` の基本動作

- `Thread.sleep(n)` は、**「現在実行中のスレッド」を一時停止**させるメソッドです。
- 他のスレッドは **影響を受けずに並行して処理を継続**できます。

---

### 🔷 例：2つのスレッドでsleepを確認

```java
public class Main {
    public static void main(String[] args) {
        // 新しいスレッド（別スレッド）
        new Thread(() -> {
            System.out.println("別スレッド: 開始");
            try {
                Thread.sleep(5000); // ← このスレッドだけが停止
            } catch (InterruptedException e) {}
            System.out.println("別スレッド: 再開");
        }).start();

        // メインスレッド
        System.out.println("メインスレッド: 処理中");
    }
}
```

### 🔹 出力（概略）

```java
メインスレッド: 処理中
別スレッド: 開始
（5秒待つ）
別スレッド: 再開
```

👉 `Thread.sleep()` は「**今、動いているスレッド**だけを止める」ものです。

---

## ⚠️ よくある誤解：「スリープ＝すべて停止」は誤り

| 誤解 | 実際 |
| --- | --- |
| `Thread.sleep()` → 全体が止まる | ❌ すべてのスレッドが止まるわけではない |
| スレッドプール内でも全停止？ | ❌ そのスレッドのみスリープする |
| `main()` で sleep → 全停止？ | ⭕ ただし、**mainスレッドは止まる**が他スレッドは動く |

---

## ✅ まとめ

| 観点 | 結論 |
| --- | --- |
| 対象スレッド | `Thread.sleep()` を呼び出したスレッドだけ |
| 他スレッドへの影響 | なし（並行して動く） |
| 実務での注意点 | sleepで「待つ」設計は基本非推奨。非同期やwait/notify、Futureでの管理が好まれる |

---

さらに興味があれば、`sleep()` と `wait()` の違いや、`sleep()` を避ける設計パターン（非同期ポーリング、スケジューリングなど）についてもご紹介できます！