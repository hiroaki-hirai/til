# Consumer （副作用メイン）

---

## 🔷 `Consumer<T>`とは？

`Consumer<T>` は、**Java 8 以降の `java.util.function` パッケージ**に含まれる**関数型インタフェース**で、以下のように定義されています：

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

---

## 🔸 特徴

| 特性 | 説明 |
| --- | --- |
| 引数 | **1つの引数 T を取る** |
| 戻り値 | **返さない（`void`）** |
| 用途 | 主に「処理の実行目的」の関数を渡す場面（＝副作用を伴う処理）で使われる |

---

## 🔸 典型例

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> printer = s -> System.out.println("受け取った値: " + s);

        printer.accept("Hello");  // → 受け取った値: Hello
    }
}
```

この例では、`Consumer<String>` は `accept(String)` メソッドで文字列を受け取り、`System.out.println` しています。

---

## 🔸 よく使われる場面

### 1. **Stream APIと一緒に使う**

```java
List<String> names = List.of("Alice", "Bob", "Charlie");

names.forEach(name -> System.out.println(name));
```

この `forEach` メソッドは `Consumer<T>` を引数に取るので、ラムダ式がそのまま使えます。

---

### 2. **メソッド参照との組み合わせ**

```java
names.forEach(System.out::println);
```

`System.out::println` は `Consumer<String>` に合致するメソッド参照です。

---

## 🔸 `andThen`による連結

`Consumer` は `andThen` メソッドで**複数の処理を直列に実行**できます。

```java
Consumer<String> greet = name -> System.out.println("Hello " + name);
Consumer<String> bye   = name -> System.out.println("Goodbye " + name);

Consumer<String> combined = greet.andThen(bye);

combined.accept("Hiroaki");
// 出力:
// Hello Hiroaki
// Goodbye Hiroaki
```

---

## 🔸 補足：`Consumer`系インタフェース

| インタフェース | 引数 | 戻り値 | 用途 |
| --- | --- | --- | --- |
| `Consumer<T>` | `T` | なし | 単純な処理 |
| `BiConsumer<T, U>` | `T`, `U` | なし | 2つの値を扱う処理 |
| `IntConsumer` など | `int`, `long`等 | なし | プリミティブ専用 |

---

## ✅ まとめ

- `Consumer<T>` は「1つの引数を受け取り、処理だけして結果を返さない」関数型インタフェース。
- **副作用（出力、ログ記録、集計など）を目的とした処理**に多用される。
- `Stream.forEach` などで自然に登場。
- `andThen` により複数の処理の組み合わせも可能。

以下に、`Consumer<T>` を題材とした **Java Silver / Gold 試験風のパターン問題集**を提示します。Java SE 8以降が対象の出題形式を想定し、選択肢ミスや混同しやすい構文にも注意を払った設計です。

---

## ✅ Silverレベル：基本理解と構文

### 【問題1】次のコードの出力として正しいものはどれですか？

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> consumer = s -> System.out.print(s.toUpperCase());
        consumer.accept("java");
    }
}
```

### A. JAVA

### B. java

### C. null

### D. コンパイルエラー

**正解：A**

> accept("java") により "java" を受け取り、toUpperCase() で JAVA を出力。
> 

---

### 【問題2】次のコードの説明として正しいものはどれですか？

```java
Consumer<Integer> c = i -> {
    int result = i * 2;
    System.out.println(result);
};
```

### A. `i` は戻り値として `result` を返す

### B. `Consumer` は値を返す必要がある

### C. `Consumer` は1引数・戻り値なしの関数を表す

### D. `accept()` というメソッド名を変更して使用できる

**正解：C**

> Consumer<T> は「T型の引数を受け取り、戻り値はない」関数型インタフェース。
> 

---

## ✅ Goldレベル：応用と応答関数の連結

### 【問題3】次のコードの実行結果として正しいものをすべて選べ。

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> c1 = s -> System.out.print("A:" + s + " ");
        Consumer<String> c2 = s -> System.out.print("B:" + s.length() + " ");

        c1.andThen(c2).accept("Go");
    }
}
```

### A. A:Go

### B. B:2

### C. A:Go B:2

### D. B:2 A:Go

**正解：C**

> andThen は c1の後にc2を実行する。出力順は A:Go B:2。
> 

---

### 【問題4】次の記述のうち、`Consumer` の使用として**不適切なもの**はどれか？

### A. `Consumer<String>` を使ってログを出力する

### B. `Consumer<Integer>` を使って数値を倍にして出力する

### C. `Consumer<Double>` を使って計算結果を返す

### D. `Consumer<List<String>>` を使ってリストを表示する

**正解：C**

> Consumer は 戻り値を返さない。結果を返すなら Function<T,R> を使う。
> 

---

### 【問題5】以下のコードが出力する内容として正しいものを選べ：

```java
List<String> list = List.of("a", "bb", "ccc");
Consumer<String> printLength = s -> System.out.print(s.length() + " ");
list.forEach(printLength);
```

### A. 1 2 3

### B. a bb ccc

### C. 3 2 1

### D. コンパイルエラー

**正解：A**

> forEach() は Consumer<T> を受け取り、各要素の length() を出力している。
> 

---

## 🔸補足：間違いやすいポイント

| 誘導パターン | 解説 |
| --- | --- |
| 戻り値を使ってしまう | `Consumer` は `void` 戻り型。結果を返す場合は `Function` |
| `andThen` の順序 | `c1.andThen(c2)` の順序を逆に考えるミス |
| `accept()` 呼び忘れ | ラムダ定義しただけで使っていないケース |

# Q,

Builderクラスのセッター的メソッドをConsumerを受け取るメソッドに置き換えることで、Builderクラスのフィールドの指定をエントリーポイント側で設定できることから、Builderクラス側のフィールドへの代入処理をConsumerを受け取るメソッド1つに集約できる認識はあっていますか？

はい、Hiroakiさん、非常に鋭い発想です。

**結論から言えば、ご認識は本質的に正しいです。**

---

## ✅ ご質問の要点

> Builderクラスの複数のセッター的メソッドを、Consumer<Builder> を受け取る1つのメソッドに集約できるか？
> 

→ ✔️ **はい、それは可能であり、設計的にも意味があります。**

`Consumer<Builder>` によって、**エントリーポイント側からフィールド操作の内容を注入**できるため、Builder クラス側には **汎用的な1つの設定メソッド**のみを残せます。

---

## ✅ イメージコード：Consumerを使ったBuilderの簡素化

### 🔸 元のBuilderパターン（典型）

```java
User user = new User.Builder()
    .name("Hiroaki")
    .age(30)
    .build();
```

↑ Builderクラス側には `name()` や `age()` のような複数のセッターが必要。

---

### 🔸 Consumerを使った1メソッド集約版

```java
User user = new User.Builder()
    .configure(b -> {
        b.name = "Hiroaki";
        b.age = 30;
    })
    .build();
```

### 👉 このように `configure(Consumer<Builder> action)` という1つのメソッドにまとめると：

- `b` はBuilder自身
- 外部から **処理（ラムダ）を注入**して、Builderのフィールドを操作

---

## ✅ 実装コード例

```java
public class User {
    private final String name;
    private final int age;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }

    public static class Builder {
        private String name;
        private int age;

        public Builder configure(Consumer<Builder> config) {
            config.accept(this);
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

---

## ✅ メリットと注意点

| メリット | 説明 |
| --- | --- |
| ✅ コード量削減 | `name()` や `age()` のようなsetter的メソッドが不要 |
| ✅ 柔軟性 | 任意の設定をエントリーポイントで自由に記述できる |
| ✅ APIをシンプルに | Builderのインタフェースが `configure()` に一本化 |

---

### ⚠ 注意点（＝デメリットにもなりうる）

| デメリット | 説明 |
| --- | --- |
| 型安全性がやや下がる | ラムダ内で誤って存在しないフィールドやロジックを記述しても補完が効きづらい場合がある |
| IDE補完やガイドが減る | セッターが個別にあると、フィールドごとに補完・JavaDocが可能だが、Consumer内はそれがやや弱くなる |
| 構造がDSL的に見えづらくなる | チェーンでの "自然言語的な構造" ではなくなる（好みの分かれる設計） |

---

## ✅ 結論：ご認識は正しく、応用的なBuilderの最適化アイデア

- `Consumer<Builder>` を使えば、Builderのセッター群を**1つの`configure()`メソッドに集約可能**
- DSL的な設計から、**より柔軟で関数的な設計へとシフト**する形
- **実務では好みにより選択**される（保守性 vs 柔軟性のトレードオフ）

# Q,

Builderパターン内にConsumerを導入する場合でも、Consumer自体は、引数を受け取り保持する役割のみで、その値を戻す場合はその外部のメソッドに値を戻す役割を担わせていました。
通常、Consumerの役割は、引数を保持し、それを出力するなどその場で処理させる場合が多いのか、それとも、Builderクラス内でのConsumer利用のように引数保持のみの役割で、その他の役割は外部に任せるパターンが多いのか、実務ではどのように使用されていますか？

素晴らしいご質問です、Hiroakiさん。

`Consumer<T>` の**役割と使われ方の違い**について、**「関数の注入」vs「即時実行」**という観点で非常に的確に捉えられています。

---

## ✅ 結論：実務での `Consumer` の使われ方は **2系統** に分かれます

| 系統 | 主な目的 | 使用場所 | 代表例 |
| --- | --- | --- | --- |
| ① **即時実行型** | 処理をその場で実行する | Stream, forEach など | `list.forEach(System.out::println);` |
| ② **設定注入型（Builder的）** | 設定や構成情報を「遅延注入」する | Builder, 設定APIなど | `.configure(builder -> builder.name = "X")` |

---

## ✅ ① 即時実行型 Consumer（主流・直感的）

### 🔸 用途：

- 関数として渡された処理をその場で**副作用として実行**（ログ、出力、集計など）

### 🔸 実例：

```java
List<String> names = List.of("A", "B", "C");

names.forEach(s -> System.out.println(s)); // Consumer<String>
```

- 引数は即時で使われ、保持されない
- **ワンショット処理**（典型的なユースケース）

---

## ✅ ② 設定注入型 Consumer（Builder的応用）

### 🔸 用途：

- 「ある対象（例：Builderや設定オブジェクト）」に対して**値を注入・設定する目的で渡す**
- 実際の動作（例：インスタンス化や値取得）は**後続で実行される**

### 🔸 実例：

```java
User user = new User.Builder()
    .configure(b -> {
        b.name = "Hiroaki";
        b.age = 30;
    })
    .build();
```

- `Consumer<Builder>` は **「引数を保持するだけ」**
- 実行後の値の受け取り・利用は `build()` 側の責任
- **状態構成専用として使われるパターン**

---

## ✅ どちらが主流か？

| シーン | 主流の使い方 | 理由 |
| --- | --- | --- |
| **Stream API / List処理** | 即時実行型が多い | データ変換・副作用処理 |
| **設定オブジェクト・Builder** | 注入型が有効 | 柔軟な構成をラムダで定義できる |
| **UIやDSL的設計（例：Swing, Config API）** | 注入型が好まれる | 直感的な構築構文が書ける |

---

## ✅ 実務ではどう選ばれるか？

| 判断軸 | 使用パターン |
| --- | --- |
| 「処理の実行が目的」 | 即時実行（forEach, logなど） |
| 「構成情報の設定が目的」 | 注入型（Builder, Config DSLなど） |
| 「処理後に戻り値が必要」 | → `Function<T, R>` を使う（Consumerではできない） |

---

## ✅ 補足：Consumerの役割＝「入力を受け取って、何か副作用を起こす」

- `Consumer` 自体は「保持」も「戻り値」も**責務として持ちません**
- それをどう使うかは、**呼び出し側の構造設計次第**

---

## ✅ 結論：Hiroakiさんのご認識は非常に的確です

- Builderの `Consumer` は **引数の保持・注入** に特化した使い方（＝設定の委譲）
- 一方で、`forEach` や `Logger` などでは **即時処理型** として使われる
- 実務では**目的ごとに適切に使い分けられており、どちらも一般的な用途**

# Q,

Consumerを引数にとる副作用メインのメソッドには、forEach以外でありますか？

はい、Hiroakiさん、鋭い視点です！

`Consumer` を引数にとり、**「副作用を目的とする」メソッド**は `forEach` 以外にも複数存在します。

以下に代表的なものを **カテゴリ別＋具体例つき** でまとめます。

---

## ✅ 結論：`Consumer` を受け取り、副作用（出力・記録・変更など）を目的とするメソッドは意外と多くあります。

---

## 🔹 1. `Optional` 系

### `ifPresent(Consumer<? super T> action)`

- 値が存在する場合のみ副作用を実行
- **副作用が主目的**の代表例です

```java
Optional<String> name = Optional.of("Taro");
name.ifPresent(System.out::println); // ← 値があれば出力（副作用）
```

---

### `ifPresentOrElse(Consumer<? super T>, Runnable)`

- 値の有無に応じて処理を分岐（いずれも副作用）

```java
name.ifPresentOrElse(
    System.out::println,                 // 値あり
    () -> System.out.println("なし")     // 値なし
);
```

---

## 🔹 2. `Stream` 系

### `peek(Consumer<? super T>)`

- **中間操作**として副作用を挿入する（ログなど）

```java
List<String> result = List.of("a", "b", "c").stream()
    .peek(s -> System.out.println("通過: " + s)) // 副作用
    .map(String::toUpperCase)
    .toList();
```

> peek() はデバッグやログ記録用で、純粋関数的には非推奨な使い方になることもあります。
> 

---

### `forEach(Consumer<? super T>)`（終端操作）

```java
stream.forEach(System.out::println); // 完全に副作用のみ
```

---

## 🔹 3. `Map` 系（Java 8以降）

### `forEach(BiConsumer<? super K, ? super V>)`

```java
Map<String, Integer> map = Map.of("apple", 1, "banana", 2);

map.forEach((key, val) -> System.out.println(key + "=" + val));
```

- `BiConsumer`（2引数）ですが、副作用が目的の `forEach`

---

## 🔹 4. `Iterable`・`Collection` の `removeIf(Predicate)`

- `Consumer` ではないですが、副作用（コレクションからの削除）を起こすという意味で似た構造です。

```java
list.removeIf(s -> s.startsWith("a")); // 状態変更
```

---

## ✅ まとめ表

| メソッド名 | 所属 | 説明 | 副作用の例 |
| --- | --- | --- | --- |
| `forEach` | `Iterable`, `Stream` | 各要素に対して処理 | 出力・ログなど |
| `ifPresent` | `Optional` | 値があれば実行 | ログ、通知など |
| `ifPresentOrElse` | `Optional` | 値あり/なし両方対応 | 条件付き出力 |
| `peek` | `Stream` | 中間操作での副作用 | デバッグログ |
| `Map.forEach` | `Map` | キー・バリュー両方で処理 | 状態出力 |
| `removeIf` | `Collection` | 条件を満たす要素を削除 | コレクション変更 |

---

## ✅ さらに：**自作クラスでの活用**

自作の `ListProcessor` クラスなどでも、副作用目的の `accept(Consumer<T>)` メソッドを定義可能です：

```java
class ListProcessor<T> {
    private final List<T> data;
    ListProcessor(List<T> data) { this.data = data; }

    public void processEach(Consumer<? super T> action) {
        data.forEach(action); // 自作forEach的なもの
    }
}
```

---

必要であれば、**ConsumerとFunctionを切り替えることで「副作用なし」に変える例**もご紹介できます！