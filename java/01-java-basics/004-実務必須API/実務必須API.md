# 実務必須API

# Q,

CompletableFutureの役割を見ると、実務的に必須のAPIという認識です。
他に今まで出てきたAPI以外に必須のAPIはありますか？

はい、Hiroakiさんのご認識は非常に適切です。

`CompletableFuture` は、**現代Javaにおける非同期・並行処理の必須API**です。

それに加えて、**実務で「避けて通れない・現場で頻出」なAPI群**は他にもあります。

---

## ✅ 【実務で必須レベル】のAPI分類と代表例

以下、今まで出てきた `FutureTask` や `ThreadPoolExecutor` を含めつつ、**実務で欠かせない API群**を「目的別」に整理します。

---

### 🧩 1. **スレッド制御・非同期処理**

| API | 必須度 | 補足 |
| --- | --- | --- |
| `Executor` / `ExecutorService` | ★★★ | タスク実行の抽象インタフェース |
| `ThreadPoolExecutor` | ★★★ | 実務でのスレッドプールのコア実装 |
| `ScheduledExecutorService` | ★★★ | 遅延実行・定期実行のために不可欠 |
| `Future` / `FutureTask` | ★★★ | 戻り値・例外・キャンセルの制御 |
| `CompletableFuture` | ★★★★ | **現代Javaでの主力非同期API**（Java 8以降） |
| `ForkJoinPool` | ★★☆ | 再帰的並列や `parallelStream()` の基盤として重要 |

---

### 🧠 2. **関数型インタフェース（非同期やStreamで多用）**

| API | 必須度 | 補足 |
| --- | --- | --- |
| `Runnable` / `Callable<T>` | ★★★ | 非同期処理の基本単位 |
| `Function<T, R>` / `Consumer<T>` / `Supplier<T>` / `Predicate<T>` | ★★★★ | **Stream・CompletableFuture・ラムダ式で必須級** |
| `BiFunction`, `UnaryOperator`, `BinaryOperator` | ★★☆ | 関数合成や比較演算などでよく出る |

---

### 📦 3. **Stream API（コレクション処理の標準）**

| API | 必須度 | 補足 |
| --- | --- | --- |
| `Stream<T>` | ★★★★ | コレクション操作の主力。実務で多用 |
| `Collectors` | ★★★★ | `toList()`, `groupingBy()`, `joining()` など |
| `Optional<T>` | ★★★ | null安全に関する補助API（戻り値に多用） |

---

### 🔐 4. **同期制御・ロック**

| API | 必須度 | 補足 |
| --- | --- | --- |
| `ReentrantLock` / `Lock` | ★★☆ | `synchronized` より柔軟に制御可能 |
| `CountDownLatch` | ★★☆ | 並行処理の完了待ちに使用（`invokeAll()`類似用途） |
| `Semaphore` / `CyclicBarrier` / `Phaser` | ★☆☆ | 高度な同期制御が必要なケース限定で使用 |

---

### 🧰 5. **補助ユーティリティ／ファクトリ群**

| API | 必須度 | 補足 |
| --- | --- | --- |
| `Executors` | ★★★ | スレッドプール生成・callableラップ・デフォルトファクトリ |
| `TimeUnit` | ★★★ | 秒／ミリ秒指定にて必須 |
| `ThreadFactory` | ★★☆ | スレッド命名・デーモン制御など |

---

## ✅ 実務観点での優先度（順序）

| 優先度 | 項目 |
| --- | --- |
| 🎯 最優先 | `CompletableFuture` / `ExecutorService` / `Callable` / `Function` 系 |
| 🔧 次点 | `FutureTask` / `ScheduledExecutorService` / `ThreadPoolExecutor` |
| ⛓️ 補助 | `Stream` / `Collectors` / `Optional`（非同期と連携させるケース多） |
| 🔒 条件付き | `Lock`, `CountDownLatch`, `ForkJoinPool`（マルチスレッド最適化時） |

---

## 🧭 実務タスクのマッピング（どう使い分けるか）

| 実務ケース | 推奨API |
| --- | --- |
| APIやDBに並列で問い合わせたい | `CompletableFuture.supplyAsync()`, `thenCombine()` |
| タスクを一定間隔で実行 | `ScheduledExecutorService.scheduleAtFixedRate()` |
| 依存関係のある非同期フロー | `thenCompose()`, `handle()` |
| 10個の処理を並列で投げ、全完了を待ちたい | `invokeAll()` または `CompletableFuture.allOf()` |
| 処理中に例外が起きたときのフォールバック | `CompletableFuture.exceptionally()`, `handle()` |

---

## 🔚 まとめ

> CompletableFuture は「非同期の流れ」を宣言的に構築する中核API
> 
> 
> そして、それを支える・補助するために以下のAPI群が必須級になります：
> 
- **非同期制御系**：`ExecutorService`, `ScheduledExecutorService`, `ThreadPoolExecutor`
- **状態管理系**：`Future`, `FutureTask`
- **関数構築系**：`Function`, `Supplier`, `Consumer`, `Predicate`
- **非同期連携系**：`Stream`, `Optional`
- **同期制御系**：`Lock`, `CountDownLatch`（必要に応じて）

---

必要であれば「目的別 APIマッピングシート」や「非同期設計での典型構文パターン集」も作成できます。どうされますか？