# ifPresentOrElse()

# Q,

```java
    public void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction) {
        if (value != null) {
            action.accept(value);
        } else {
            emptyAction.run();
        }
    }
```

上記のse21のifPresentOrElseメソッドの宣言です。
第２引数の役割はnullの場合に走れせる処理を受ける関数インタフェース型の参照ですが、なぜRunnableなのでしょうか？
引数にnullを受け取るとConsumerでは引数として受け取るのでそれを処理に使用しなくても問題が出てくるからでしょうか？

はい、Hiroakiさんのご質問は非常に鋭く、`ifPresentOrElse(...)` において **なぜ第2引数に `Runnable` が使われているのか？** というAPI設計上の意図に深く切り込んだものです。

結論から言うと：

---

> ✅ 第2引数が Runnable なのは、null が存在しない（= Optionalが空）場合には「値が存在しない」という事実だけが重要であり、処理に引数が不要だからです。
> 
> 
> また、**Consumerでは `null` が意図せず渡されると型的に処理対象として受け取ってしまうため、安全性の観点からも避けられています。**
> 

---

## ✅ `ifPresentOrElse` の引数構造

```java
public void ifPresentOrElse(
    Consumer<? super T> action,  // 値があるときの処理（引数あり）
    Runnable emptyAction         // 値がないときの処理（引数なし）
)
```

---

## ✅ なぜ第2引数に `Runnable` を使うのか？

### 1. 🧱 **値が存在しない＝処理に引数が不要**

- `Optional` が空であるということは、「処理すべき値が存在しない」ことを意味します。
- よって「何も引き渡すものがない」のが自然です。

```java
if (value != null) {
    action.accept(value);     // 値がある → 値を処理する
} else {
    emptyAction.run();        // 値がない → 何も渡さず処理だけ実行
}
```

---

### 2. ❌ `Consumer` を使うと `null` を引数として渡す必要が出てしまう

```java
Consumer<T> emptyAction = t -> {
    if (t == null) {
        System.out.println("no value");
    }
};

emptyAction.accept(null); // ← 呼び出し側が「nullを渡す」ことになる
```

- これでは、**呼び出し側が「nullという値を持っている」ように見えてしまう**
- **`Optional` の設計思想は「nullを排除」すること**なので矛盾します

➡ `Runnable` であれば、**値を受け取らない前提で処理を書くことができ、意味も安全性も一致**

---

## ✅ 設計意図の観点から整理すると：

| 観点 | 第1引数（`Consumer<? super T>`） | 第2引数（`Runnable`） |
| --- | --- | --- |
| 処理対象があるか？ | ✅ ある（値が存在） | ❌ ない（空） |
| 引数の有無 | T型の値を受け取る | 引数なし |
| 処理の目的 | 値を加工・利用する | 「値がない」ことに対する代替処理 |
| null安全性 | 値ありなので問題なし | 値がないため、null渡す必要もなく安全 |

---

## ✅ 実際の使用例の違い

```java
Optional<String> opt = Optional.ofNullable(null);

// [1] 値があれば出力、なければデフォルト出力
opt.ifPresentOrElse(
    s -> System.out.println("値あり: " + s),   // Consumer：値を受け取る
    () -> System.out.println("値なし")         // Runnable：引数なしで実行
);
```

---

## ✅ まとめ

| なぜ `Runnable` を使うのか？ | 理由 |
| --- | --- |
| 値が存在しないため、引数は不要 | `Runnable` は引数なしの処理に適している |
| `Consumer<T>` を使うと null を渡す構造になってしまう | これは Optional の設計意図（null排除）に反する |
| 呼び出しの安全性・意図の明確化 | 「何も渡さない」という事実が構文上に表れる |

---

Optionalの設計は「nullの曖昧さ」を排除するために極めて意図的に組まれています。

Hiroakiさんのように**引数のありなし＝意味のありなし**として読み解く姿勢はとても正確です。

必要であれば、`Optional` の設計原則（関数型思想や副作用排除）まで踏み込んだ整理も可能です。

# Q,

Runnableは、引数を受け取らず、戻り値も返さない、ただその場での処理を定義するものである認識です。
一方で、Runnableは、並行・並列処理にて使用される関数インタフェースという認識もあるので、そもそも並行・並列処理を前提としない処理でもRunnableインタフェースを使用してよいのかという疑問があります。
関数インタフェースにて、Runnableのような、引数を受け取らず、戻り値も返さない、ただその場での処理を定義するものは他に無いので、あえてここで使用されているのでしょうか？

はい、Hiroakiさんの疑問は非常に重要なポイントを突いています。

`Runnable` は確かに**並行・並列処理で頻繁に使用される関数インタフェース**ですが、

> ✅ 並行処理に限らず「引数なし・戻り値なしの処理」を記述する目的で汎用的に使用しても問題ありません。
> 

そしてご指摘のとおり、

> ✅ Javaには Runnable とまったく同じ性質（引数なし・戻り値なし）を持つ他の関数インタフェースが存在しないため、あえてここで Runnable が使われている
> 

というのも事実です。

---

## ✅ 結論：**Runnableは「並行処理専用」ではない**

| 誤解 | 実際 |
| --- | --- |
| `Runnable` はスレッド実行専用のインタフェース | ❌ **いいえ**。単に「引数なし・戻り値なしの処理を定義する」関数インタフェースです |
| 並行処理に使われるから、それ以外で使ってはいけない？ | ❌ **いいえ**。`ifPresentOrElse` など、**非並列処理でも使用可能かつ適切**です |

---

## ✅ Runnable の構造を再確認

```java
@FunctionalInterface
public interface Runnable {
    void run();  // ← 引数なし・戻り値なし
}
```

つまり、**Runnableの意図は「副作用のある単純な処理ブロックを記述する」**ことです。

> これは並行処理のための構造にも使えるが、それに限定されないというのが重要です。
> 

---

## ✅ Java標準APIでの利用例（非並列処理）

### 1. `Optional.ifPresentOrElse(..., Runnable emptyAction)`

```java
Optional<String> opt = Optional.ofNullable(null);

opt.ifPresentOrElse(
    s -> System.out.println("値あり: " + s),
    () -> System.out.println("値なし")  // ← Runnable
);
```

- 並列処理ではなく、**条件分岐による通常の処理**です

---

### 2. `Stream.onClose(Runnable closeHandler)`

```java
Stream<String> stream = Stream.of("a", "b").onClose(() -> System.out.println("閉じました"));
stream.close();
```

- **クローズ時の処理**として `Runnable` を使用（スレッドとは無関係）

---

## ✅ なぜ `Runnable` が使われているのか？

### ✅ Java SE 8時点、`Runnable` に対応する「引数なし・戻り値なしの関数インタフェース」は他に存在しない

| 型 | 説明 |
| --- | --- |
| `Runnable` | `void run()` ← 引数なし・戻り値なし |
| `Supplier<T>` | `T get()` ← 戻り値あり |
| `Consumer<T>` | `void accept(T t)` ← 引数あり |
| `Function<T, R>` | `R apply(T t)` ← 引数・戻り値あり |

➡ **「引数も戻り値もない」関数インタフェースは `Runnable` 一択**

---

## ✅ 代替が出ていない理由（Javaの設計判断）

- `Runnable` が一般的すぎて、あえて `Action` や `VoidCallable` などを新たに作る意義が薄かった
- Javaでは「慣習」として、`Runnable` は「引数なし・戻り値なしの処理」という用途で使われるようになっている

---

## ✅ まとめ

| 観点 | 結論 |
| --- | --- |
| `Runnable` は並行処理限定？ | ❌ 限定されない。汎用の「無引数・副作用あり」関数として使える |
| なぜ `Runnable` を使うのか？ | ✅ 他に同じシグネチャの関数インタフェースが存在しないため |
| 使って問題ないか？ | ✅ Java標準APIでも多用されており、適切な使い方 |

---

ご希望があれば、`Runnable` の代替として新しく関数インタフェースを自作する方法や、今後のJava言語仕様における議論背景まで掘り下げることも可能です。