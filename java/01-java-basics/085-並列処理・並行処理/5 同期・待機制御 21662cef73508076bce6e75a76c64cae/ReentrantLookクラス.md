# ReentrantLookクラス

# Q,

ReentrantLookについて解説してください

はい、Hiroakiさんのご質問にお答えして、`ReentrantLock` について詳しく解説します。これは `java.util.concurrent.locks` パッケージに含まれる、**高機能な排他制御用ロッククラス**で、`synchronized` に代わる選択肢として実務で非常によく使われます。

---

## 🔷 ReentrantLock とは？

### 📌 定義：

> 同一スレッドが繰り返しロックを取得可能（＝再入可能）な排他ロッククラス
> 
- `synchronized` と同じく、**クリティカルセクションを排他制御**する
- さらに、**明示的なロック/アンロック**, **タイムアウト**, **割り込み対応**, **フェアネス制御**などの**柔軟な機能**を提供

---

## ✅ 基本的な使い方

```java
import java.util.concurrent.locks.ReentrantLock;

public class Counter {
    private final ReentrantLock lock = new ReentrantLock();
    private int count = 0;

    public void increment() {
        lock.lock();           // 明示的にロック取得
        try {
            count++;
        } finally {
            lock.unlock();     // 必ずアンロック！（例外防止のためfinallyで）
        }
    }

    public int getCount() {
        return count;
    }
}
```

---

## 🔁 synchronized との違いまとめ

| 比較項目 | `synchronized` | `ReentrantLock` |
| --- | --- | --- |
| 書き方 | 修飾子／ブロック構文 | 明示的に `lock()` / `unlock()` |
| 再入可能性 | ✅ 可能 | ✅ 可能 |
| タイムアウト付きロック | ❌ なし | ✅ あり（`tryLock(timeout)`） |
| 割り込み対応 | ❌ 不可 | ✅ 可能（`lockInterruptibly()`） |
| 公平性（順番制御） | ❌ JVM依存 | ✅ フェアモードあり |
| 条件待ち制御 | ❌ `wait()` / `notify()` だけ | ✅ `Condition` による高度な制御 |
| 可読性 | シンプル | やや複雑だが柔軟 |

---

## 🔶 主なメソッド

| メソッド | 説明 |
| --- | --- |
| `lock()` | ロックを取得（取得までブロック） |
| `unlock()` | ロックを解放（`try/finally`で必ず解放） |
| `tryLock()` | 即時にロック取得を試み、成功すれば true |
| `tryLock(long timeout, TimeUnit unit)` | 一定時間内にロック取得を試みる |
| `lockInterruptibly()` | ロック待機中に割り込み可能 |
| `isLocked()` | 他スレッドによりロックされているか確認 |
| `isHeldByCurrentThread()` | 現在のスレッドがロック保持中か確認 |

---

## 🧩 フェアロック（公平性）

### 通常（デフォルト）

```java
new ReentrantLock();  // 非フェア（割り込みOK、性能重視）
```

### フェアモード（先着順）

```java
new ReentrantLock(true);  // フェアロック（順番保証）
```

→ 後から来たスレッドは先に処理されているスレッドを待たされない

---

## 📘 再入可能（Reentrant）とは？

```java
lock.lock();
try {
    methodA();  // この中でまた lock.lock() しても問題なし
} finally {
    lock.unlock();
}
```

→ **同一スレッドが何度 lock() してもブロックされずに再入できる**

---

## 🔁 条件変数（`Condition`）との連携

`synchronized` の `wait()` / `notify()` に相当する機能

```java
Lock lock = new ReentrantLock();
Condition condition = lock.newCondition();

lock.lock();
try {
    condition.await();       // 待機
    condition.signal();      // シグナル送信
} finally {
    lock.unlock();
}
```

→ より細かく**複数の待機条件を分けて管理**できる点が強みです。

---

## 🔚 まとめ

| 特性 | 内容 |
| --- | --- |
| スレッドセーフ | ✅ 排他制御に使用できる |
| 再入可能 | ✅ 同一スレッドが何度でも lock() できる |
| 明示的制御 | ✅ `lock()` / `unlock()` 必須（try-finally推奨） |
| 柔軟性 | ✅ フェア制御・タイムアウト・割り込み対応が可能 |
| 対象用途 | 高度なスレッド制御が必要なケース（条件分岐、制御の分離など） |

---

ご希望があれば、`synchronized` を `ReentrantLock` に書き換える演習問題や、`Condition` を使った待機・通知モデルの例もご紹介できます！

# Q,

RunnableやCallableにて１つ１つの処理を定義していたところ、ReentrantLookクラスを使用すると１つ１つ定義していた処理をまとめることができるイメージでしょうか？
メソッドをまとめて制御できることと、処理の特定性のようなところで混乱しています。

とても鋭いご質問です。Hiroakiさんの混乱は自然なもので、「**Runnable / Callable による処理の“粒度”**と、**ReentrantLock による“制御の単位”**」が頭の中で重なって見えている状態だと思います。

そこで以下に、**両者の役割と使い分け**を明確に整理します。

---

## 🔷 結論の先取り

> ✅ **Runnable / Callable は「何をするか（処理の定義）」<br>
> 
> 
> ✅ **ReentrantLock は「**どの範囲を排他制御するか（実行の制御）**」
> 

---

## 🔸【1】Runnable / Callable：処理の単位を定義

- **役割**：スレッドで並行実行させる「処理の中身」を定義
- `Runnable` → 戻り値なし（`void run()`）
- `Callable<T>` → 戻り値あり（`T call()`）

### ✅ イメージ：

```java
Runnable task = () -> {
    System.out.println("処理A");
};
executor.submit(task);
```

→ 「処理A」という**やることそのもの**を定義している。

---

## 🔸【2】ReentrantLock：実行時の排他制御を行う

- **役割**：**どの処理が同時に実行されてはいけないかを制御**
- 「この範囲は他スレッドと同時実行禁止」とするために使う

### ✅ イメージ：

```java
ReentrantLock lock = new ReentrantLock();

Runnable task = () -> {
    lock.lock();
    try {
        System.out.println("排他処理A");
    } finally {
        lock.unlock();
    }
};
```

→ 同じ `lock` を共有していれば、**この中の処理は1スレッドずつしか実行されない**

---

## ✅ では「まとめられる」とはどういう意味か？

### ❌ 誤解されやすい点：

「ReentrantLock を使うと Runnable をまとめられる？」

→ これは **Runnableを“1つに合成できる”という話ではなく**、

### ✅ 正確には：

> 複数の Runnable / Callable の中の「共通のクリティカルセクション（排他領域）」をひとまとめに制御できる
> 

---

### 🔁 例：ReentrantLock によって制御される粒度を揃える

```java
public class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void write() {
        lock.lock();
        try {
            // 書き込み処理A
        } finally {
            lock.unlock();
        }
    }

    public void update() {
        lock.lock();
        try {
            // 書き込み処理B
        } finally {
            lock.unlock();
        }
    }
}
```

上記のように：

- `Runnable taskA = () -> shared.write();`
- `Runnable taskB = () -> shared.update();`

という **異なる Runnable にまたがる処理**も、**同じ lock を使えば一貫して排他制御できる**

---

## ✅ まとめ：役割の違い

| 要素 | 主な役割 | 対象 | 排他制御との関係 |
| --- | --- | --- | --- |
| `Runnable` / `Callable` | 処理の定義（スレッドで実行） | どんな処理をするか | 排他制御は含まない（設計により外から付ける） |
| `ReentrantLock` | 実行時の同時アクセスを防ぐ | どの範囲を保護するか | 保護対象を `lock()` / `unlock()` で囲む |

---

## 💡 実務の設計的視点（混乱を防ぐための補足）

- **Runnable / Callable = 「プレイヤー」**
- **ReentrantLock = 「ルール（1人ずつプレイ）」**

処理（プレイヤー）を多数用意した上で、「このエリアは同時に1人だけ入れる」というルールを ReentrantLock で実現する。

---

ご希望あれば、「複数タスクを排他制御付きで実行するフレームワーク的なテンプレート」も提示可能です！

# Q,

クラスを新規作成して、ReentrantLookをフィールドに注入して使用するイメージでしょうか？
責任分担の観点から。

はい、Hiroakiさんのご認識は**非常に良い設計志向に基づいたもの**で、**正しい方向性**です。

結論から言うと：

> ✅ ReentrantLock は「共有すべき排他制御の責任範囲」に合わせて、クラス内でフィールドとして持つ設計が一般的です。
> 

これは、**排他制御の責任をそのクラス自身が負う**ということでもあり、**カプセル化・責務分担の原則（SRP）**に適った設計です。

---

## 🔷 イメージ図

```java
┌──────────────┐
│  SharedResourceクラス │ ← 責務：リソース操作
├──────────────┤
│ + lock: ReentrantLock │ ← 排他制御をこのクラスで完結
│ + write()             │
│ + update()            │
└──────────────┘
```

---

## ✅ 実装例

```java
import java.util.concurrent.locks.ReentrantLock;

public class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void write() {
        lock.lock();
        try {
            System.out.println("Writing...");
            // クリティカルセクション
        } finally {
            lock.unlock();
        }
    }

    public void update() {
        lock.lock();
        try {
            System.out.println("Updating...");
        } finally {
            lock.unlock();
        }
    }
}
```

上記のように：

- `ReentrantLock` は **外部から注入するのではなく、内部の責任としてクラスが持つ**（自律性の確保）
- 使用者側（Runnableなど）は `resource.write()` のように呼び出すだけでよい（呼び出し元はロックの存在を意識しない）

---

## 🔄 では「注入」の話はどうなるか？

### ✅ 外部から `ReentrantLock` を注入する設計も可能です。

これは「**ロックの粒度を跨いで共有したい場合**」に意味があります。

### 例：複数のクラスにまたがって同じロックで排他したい

```java
public class ResourceA {
    private final ReentrantLock lock;

    public ResourceA(ReentrantLock lock) {
        this.lock = lock;
    }

    public void doA() {
        lock.lock();
        try {
            System.out.println("A...");
        } finally {
            lock.unlock();
        }
    }
}

public class ResourceB {
    private final ReentrantLock lock;

    public ResourceB(ReentrantLock lock) {
        this.lock = lock;
    }

    public void doB() {
        lock.lock();
        try {
            System.out.println("B...");
        } finally {
            lock.unlock();
        }
    }
}
```

```java
ReentrantLock sharedLock = new ReentrantLock();
ResourceA a = new ResourceA(sharedLock);
ResourceB b = new ResourceB(sharedLock);
```

→ AとBが**同時に操作されると競合するリソース**であれば、このように**ロックを注入して共有**する設計も妥当です。

---

## 🏁 結論：責任分担の原則に従った選択

| パターン | 選ぶべき場面 | 設計意図 |
| --- | --- | --- |
| **クラス内部に持つ** | そのクラス単独でリソース操作を完結する | 排他制御もクラスの責任として閉じる（SRP） |
| **外部から注入する** | 複数クラスやコンポーネント間でロック共有 | 排他制御の粒度をまたいで共有する必要がある |

---

ご希望あれば「排他制御の設計パターン（内包型／注入型）」を整理した比較表＋図解もご提供します！