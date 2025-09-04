# Predicate

Predicateインターフェイスは、Java 8以降に導入された**関数型インターフェイス**の一つで、特に**条件判定（真偽値の返却）**を行う処理に使われます。以下に詳しく解説します。

---

### 🔷 Predicateインターフェイスとは？

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

- `T` は判定対象の型（ジェネリクス）
- `test(T t)` メソッドで条件を判定し、`true` または `false` を返す
- ラムダ式やメソッド参照で記述することが多い

---

### 🔸 主な用途

- `Stream#filter()` の条件指定
- 条件チェックの共通化（再利用性アップ）
- 関数の引数として条件を渡す（高階関数のパターン）

---

### 🔸 使用例

### 例1：`Stream`との併用（偶数だけをフィルタ）

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6);

List<Integer> even = list.stream()
                         .filter(n -> n % 2 == 0) // Predicate<Integer>
                         .collect(Collectors.toList());

System.out.println(even); // [2, 4, 6]
```

### 例2：Predicateの変数として使う

```java
Predicate<String> isLong = s -> s.length() > 5;

System.out.println(isLong.test("hello"));      // false
System.out.println(isLong.test("predicate"));  // true
```

---

### 🔸 メソッド連結（defaultメソッド）

Predicateには条件を組み合わせるための**defaultメソッド**が用意されています。

| メソッド | 説明 |
| --- | --- |
| `and(Predicate)` | AND条件（両方trueでtrue） |
| `or(Predicate)` | OR条件（どちらかtrueでtrue） |
| `negate()` | NOT条件（true → false, false → true） |

```java
Predicate<String> isNotEmpty = s -> !s.isEmpty();
Predicate<String> startsWithA = s -> s.startsWith("A");

Predicate<String> complex = isNotEmpty.and(startsWithA);

System.out.println(complex.test("Apple")); // true
System.out.println(complex.test(""));      // false
System.out.println(complex.test("Banana"));// false
```

---

### 🔸 よく使われる標準Predicate

```java
Predicate<Object> nonNull = Objects::nonNull;
Predicate<String> isEmpty = String::isEmpty;
```

---

### 🔹 他の関数型インターフェイスとの違い（比較）

| インターフェイス | 引数 | 戻り値 | 用途 |
| --- | --- | --- | --- |
| `Predicate<T>` | T | boolean | 条件判定 |
| `Function<T, R>` | T | R | 変換処理 |
| `Consumer<T>` | T | void | 処理の実行 |
| `Supplier<T>` | なし | T | 値の供給 |

---

### ✅ まとめ

- `Predicate<T>`は「条件チェック」に特化した関数型インターフェイス
- ラムダ式と併せて、処理の柔軟性・再利用性が向上
- `Stream#filter()`などで非常に多用される
- `and()`, `or()`, `negate()`により複雑な条件も構築可能

---

さらに応用編として「複数の条件でフィルタリングしたいとき」「Predicateのリストを使って複数のルールを適用したいとき」なども紹介できますが、ここまでの内容で一度区切りましょうか？

以下に **Java Silver / Gold 試験スタイル**に合わせた `Predicate` に関する**パターン問題**を作成しました。Silverのレベルから入り、Gold相当のやや応用まで含んでいます。

---

## 🧠 Predicate に関するパターン問題（選択式）

---

### 【問1】基本問題（Silverレベル）

次のコードの出力として正しいものを1つ選びなさい。

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<String> isShort = s -> s.length() < 5;
        System.out.println(isShort.test("Java"));
    }
}
```

### 選択肢：

A. true

B. false

C. コンパイルエラー

D. 実行時例外

|  | 問題 | 正解 | 解説 |
| --- | --- | --- | --- |
| 問1 | 基本 | A | "Java" の長さは4 → `< 5` なので `true`。 |

---

### 【問2】filterとの連携（Silver〜Gold）

次のコードの出力として正しいものを1つ選びなさい。

```java
import java.util.*;
import java.util.function.Predicate;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Amy", "Bob", "Alex", "Anna");
        Predicate<String> startsWithA = s -> s.startsWith("A");

        List<String> result = names.stream()
                                   .filter(startsWithA)
                                   .collect(Collectors.toList());

        System.out.println(result);
    }
}
```

### 選択肢：

A. [Amy, Alex, Anna]

B. [Amy, Anna]

C. [Alex, Anna]

D. コンパイルエラー

|  | 問題 | 正解 | 解説 |
| --- | --- | --- | --- |
| 問2 | filter | **A** | `startsWith("A")` を満たすのは `"Amy", "Alex", "Anna"` → よって `[Amy, Alex, Anna]` が出力される。 |

---

### 【問3】Predicateの合成（Gold）

次のコードの出力として正しいものを1つ選びなさい。

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<String> p1 = s -> s.length() >= 3;
        Predicate<String> p2 = s -> s.contains("x");

        Predicate<String> combined = p1.and(p2);
        System.out.println(combined.test("ex"));
    }
}
```

### 選択肢：

A. true

B. false

C. コンパイルエラー

D. 実行時例外

|  | 問題 | 正解 | 解説 |
| --- | --- | --- | --- |
| 問3 | and | B | "ex" の長さは2（条件1に不一致）→ and条件では両方一致が必要 → `false`。 |

---

### 【問4】negateの利用（Gold）

次のコードの出力として正しいものを1つ選びなさい。

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<String> isEmpty = String::isEmpty;
        Predicate<String> isNotEmpty = isEmpty.negate();

        System.out.println(isNotEmpty.test(" "));
    }
}
```

### 選択肢：

A. true

B. false

C. コンパイルエラー

D. 実行時例外

|  | 問題 | 正解 | 解説 |
| --- | --- | --- | --- |
| 問4 | negate | A | 空文字列ではない（空白1文字）→ isEmpty は false → negateで true。 |

# Q,

PredicateをFanctionで代用することはできますか？

はい、**`Predicate<T>` を `Function<T, Boolean>` で代用することは技術的には可能**です。ただし、**用途と意図が異なる**ため、「代用できるか？」という質問に対しては以下のように整理するのが適切です。

---

## ✅ 結論

| 観点 | 答え |
| --- | --- |
| **構文的に代用できるか？** | ✅ 可能です（`Function<T, Boolean>` を使えば） |
| **意味論的（設計的）に適切か？** | ⚠️ 原則として代用すべきではありません。`Predicate` を使う方が意図が明確です。 |

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

→ **真偽値を返す関数型インタフェース**。フィルター処理や条件判定などに使われます。

---

### ● `Function<T, Boolean>` との違い

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

- `Function<T, Boolean>` も、**Tを受け取ってBooleanを返す**という点では構文的に同じように使えます。
- しかし、**戻り値の型が`boolean`ではなく`Boolean`（ラッパー型）になる**ため、オートボクシング／アンボクシングが発生します。
- さらに、`Function`は**何かを変換する関数**という意味合いが強く、`Predicate`のような**条件判定の意味を意図的に伝えたいときには不適切**です。

---

### ✅ 例：代用コード（可能だけど非推奨）

```java
Predicate<String> isEmpty = s -> s.isEmpty();

System.out.println(isEmpty.test("")); // true
```

→ `test()` というメソッド名のおかげで、**これは「テスト＝判定」だと明確に分かる**。

---

## ✅ まとめ：いつ `Function` を使い、いつ `Predicate` を使うべき？

| 用途 | インタフェース | 備考 |
| --- | --- | --- |
| 変換処理（T → R） | `Function<T, R>` | 例：`String` → `Integer` |
| 真偽判定（条件） | `Predicate<T>` | 例：`String` が空かどうか |

→ **意味に沿ったインタフェースを使うことが、読みやすく・バグを防ぎやすいコードになります。**

---

必要であれば、`Predicate` を使った `filter` 処理の具体例や、`Function` との組み合わせ（`andThen` や `compose`）の使用例も紹介できますよ。興味ありますか？