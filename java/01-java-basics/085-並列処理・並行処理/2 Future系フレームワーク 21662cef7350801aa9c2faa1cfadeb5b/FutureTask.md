# FutureTask

# Q,

FutureTaskの役割について確認です。
RunnableにしろCallableにしろ、実行自体はThreadPoolExecutorクラスが担い、その処理内容自体はRunnableやCallableの具象クラスにて定義されている認識です。
処理内容もその処理の実行も担当が決まっているのに、別にクラス（FutureTaskクラス）にその処理を保持させる理由と役割が不明確です。
処理が開始される前や後の状態を管理したりその状態を返したりするような補助・補佐的な役割という認識はあっていますか？

はい、Hiroakiさんのご認識は**本質的に非常に正確**です。

その上で、**FutureTaskの役割は「実行の補助」だけではなく、実行の管理と結果の中継・制御まで含めた“実行ライフサイクル管理”の中核的な存在**です。

以下で整理して解説します。

---

## ✅ 結論：FutureTaskの本質的な役割

> ✔️ FutureTask は「タスクの実行をラップし、状態の管理と結果の取得・通知機構を提供する」実行管理ユニットです。
> 
> 
> ✔️ RunnableやCallableの処理自体は**保持しているだけ**だが、その「実行前・実行中・実行後」の**状態遷移を管理・可視化・制御**するという極めて重要な補佐役です。
> 

---

## 🔍 なぜFutureTaskが必要なのか？

### 🔹 1. Runnable / Callable には「状態管理」の責務がない

| 機能 | Runnable / Callable | FutureTask |
| --- | --- | --- |
| 処理定義（何をするか） | ✅ 担当 | ✅ ラップして保持 |
| 処理をいつ開始するか | ❌ 不可制御 | ✅ `run()` をトリガーに明示制御 |
| 結果の保存・取得 | ❌ できない | ✅ `get()` で取得可能 |
| 処理中の状態 | ❌ 分からない | ✅ `isDone()`, `isCancelled()` で確認可 |
| 中断・キャンセル | ❌ 不可能 | ✅ `cancel()` が可能 |

🔁 → **RunnableやCallableは「実行内容」だけを定義する軽量なオブジェクト**

🔁 → そこに「実行結果」「状態遷移」「例外情報」を統合して扱う**「実行制御のハブ」**が必要 ⇒ それが `FutureTask`

---

## 🔧 FutureTaskの役割を整理すると

| 機能分類 | 内容 |
| --- | --- |
| 🧠 保持 | `Runnable` や `Callable` の具象クラス（実処理） |
| 🚀 実行 | `run()` を持ち、スレッドから起動可能（＝Runnable） |
| 📦 結果保持 | `call()` の戻り値を内部に保存し、`get()` で取り出せる |
| 💥 例外管理 | 例外が起きれば `ExecutionException` に変換して保持 |
| 🔄 状態制御 | `NEW → RUNNING → COMPLETED / CANCELLED` などの状態管理 |
| 🛑 中断対応 | `cancel()`, `isCancelled()` などでキャンセル制御が可能 |

---

## 🎯 状態管理に特化した中間レイヤーとしての位置づけ

```java
[Callable / Runnable] ← 処理定義のみ（何をするか）
       ↓
[FutureTask] ← 状態・結果・例外を管理（どう実行されたか）
       ↓
[ThreadPoolExecutor] ← 実際にスレッド上で run() を呼ぶ（いつ・どこで実行するか）
```

👉 Hiroakiさんの表現で言えば、まさに：

> ✅ 「実行対象（Runnable / Callable）」と「実行環境（ThreadPoolExecutor）」の橋渡しをし、実行結果や状態を補助・制御する役割
> 

---

## 📌 重要な補足：なぜFutureではなくFutureTaskなのか？

- `Future` はインタフェースで、あくまで「結果を非同期で受け取る器」
- `FutureTask` はその **実装＋実行機能付きのハイブリッドオブジェクト**

```java
public interface RunnableFuture<V> extends Runnable, Future<V> {
    void run();
}
```

→ `FutureTask` は `RunnableFuture` の唯一の標準実装です

---

## ✅ まとめ

| 観点 | Hiroakiさんの認識 | 解説 |
| --- | --- | --- |
| 実際の処理は Callable / Runnable に書かれる | ✔️ | 正しい |
| ThreadPoolExecutor はそれをスレッドに載せて実行 | ✔️ | 正しい |
| FutureTask の役割は補佐的？ | ✔️ 方向性は合っているが → より正確には「実行制御と状態・結果管理の中核」 |  |
| なぜラップして持つのか？ | ✔️ 実行管理・中断・例外・getなど全て集約するため |  |

---

ご希望があれば以下も展開できます：

- `FutureTask` の状態遷移図（NEW → COMPLETED等）
- `cancel()` の動作詳細（割り込み可否の違い）
- `FutureTask` を拡張して独自の監視やログを追加するパターン

お気軽にどうぞ！

# Q,

FutureTaskのcancelメソッドと、ThreadPoolExecutorのshutdownメソッドの違いはどういったものでしょうか？

非常に良いご質問です。

`FutureTask#cancel()` と `ThreadPoolExecutor#shutdown()` はどちらも「処理の停止」に関係しますが、**対象・粒度・目的がまったく異なる**別の機能です。

---

## ✅ 結論：違いの全体像

| 観点 | `FutureTask.cancel()` | `ThreadPoolExecutor.shutdown()` |
| --- | --- | --- |
| 対象 | **1つのタスク（単位）** | **Executor全体（全タスク）** |
| 粒度 | ミクロ（個別） | マクロ（全体） |
| 目的 | 特定のタスクをキャンセル（結果不要・中止） | サービス全体の**正常な停止**手続きを開始 |
| 効果 | 状態：`isCancelled()` が true に、`get()` は例外に | 新規タスクの受付を停止、既存タスクはそのまま処理 |
| 割り込み | オプションでスレッドに割り込み可 | しない（割り込みは `shutdownNow()` で可能） |
| 使用場面 | 個々の処理が不要・失敗・タイムアウトした場合 | アプリケーション終了、Executorの明示的な停止時 |

---

## 🔍 1. `FutureTask.cancel(boolean mayInterruptIfRunning)`

### ✅ 目的：

> このタスクだけをキャンセルしたい。もう結果は要らない。
> 

### ✅ 動作：

- タスクがまだ実行前 → キューから除去して未実行に
- 実行中 → `mayInterruptIfRunning == true` であれば、**スレッドに割り込み**
- 状態遷移：`NEW → CANCELLED`
- `get()` → `CancellationException` を投げる

### ✅ 使用例：

```java
Future<?> future = executor.submit(() -> doWork());
Thread.sleep(1000);
future.cancel(true); // 実行中でも強制キャンセル要求
```

---

## 🔍 2. `ThreadPoolExecutor.shutdown()`

### ✅ 目的：

> Executor全体を正常に終了させる。
> 
> 
> これ以降、新しいタスクは**受け付けない**。
> 

### ✅ 動作：

- **新規タスク受付停止**
- **既にキューにあるタスク、実行中のタスクは最後まで動かす**
- `isShutdown()` → true
- `awaitTermination()` と組み合わせて終了待ち可能

### ✅ 使用例：

```java
executor.shutdown();
if (!executor.awaitTermination(10, TimeUnit.SECONDS)) {
    executor.shutdownNow(); // 強制停止
}
```

---

## 🔧 補足：`shutdown()` vs `shutdownNow()`

| メソッド | 内容 |
| --- | --- |
| `shutdown()` | 新規受付停止＋既存は完了させる（穏やか） |
| `shutdownNow()` | 新規受付停止＋実行中のタスクに**割り込み**＋**キュー破棄**（強制） |

---

## ✅ まとめ（本質比較）

| 構造 | `FutureTask.cancel()` | `ThreadPoolExecutor.shutdown()` |
| --- | --- | --- |
| 停止対象 | **1つのタスク** | **スレッドプール全体** |
| 割り込み | 任意（true指定時） | なし（shutdownNowはあり） |
| 結果取得 | `get()`で例外発生 | タスク完了分は取得可能 |
| 適用タイミング | 特定の仕事を無効化 | アプリケーションやサービスの終了 |
| 副作用 | タスクの状態：`CANCELLED` | Executorの状態：`isShutdown()` / `isTerminated()` に変化 |

---

## 🧠 使い分け指針

| シーン | 推奨 |
| --- | --- |
| タスク単位で不要になった・中止したい | `future.cancel(true)` |
| アプリ終了時に Executor 全体を止めたい | `executor.shutdown()` → `awaitTermination()` |
| 異常終了やタイムアウトで強制停止したい | `executor.shutdownNow()` |

---

さらに深く知りたい場合：

- `cancel()` が効かない場合（実行中の処理が割り込みに応じない）
- `shutdownNow()` によって失われるタスク（返り値のリスト）
- Springでのタスクキャンセルの実践的な組み込み方

などもご案内できます！

ExecutionException の意味的契約？はどのようなものでしょうか？
どうったい場合に走らせる例外ですか？