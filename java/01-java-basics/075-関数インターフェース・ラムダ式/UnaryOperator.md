# UnaryOperator

ここでは `UnaryOperator<T>` について、`Function` との関係や使い方、実務での応用まで含めて体系的に解説します。

---

## 🔷 UnaryOperator<T> とは？

`UnaryOperator<T>` は、Java 8 で導入された関数型インターフェイスで、

> 「T型の値を受け取り、同じT型の値を返す処理」を定義するために使います。
> 

```java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {
    static <T> UnaryOperator<T> identity() {
        return t -> t;
    }
}
```

### ✅ 特徴

- `Function<T, R>` の**特殊型**であり、`T == R` のときに使うと意図が明確になります。
- **「値を変換するが型は変わらない」処理**に最適です。

---

## 🔸 使用例

### 文字列を大文字に変換する `UnaryOperator`

```java
import java.util.function.UnaryOperator;

UnaryOperator<String> toUpper = s -> s.toUpperCase();
System.out.println(toUpper.apply("java"));  // 出力: JAVA
```

### 数値を2倍にする `UnaryOperator`

```java
UnaryOperator<Integer> doubler = x -> x * 2;
System.out.println(doubler.apply(10));  // 出力: 20
```

---

## 🔸 `Function` との違い

| インターフェイス | 引数型 | 戻り値型 | 用途 |
| --- | --- | --- | --- |
| `Function<T, R>` | T | R | 変換（型が変わる可能性あり） |
| `UnaryOperator<T>` | T | T | 同じ型での変換処理 |

💡 `UnaryOperator<T>` は、`Function<T, T>` と**完全に等価**ですが、

明示的に「型が変わらない」ことを表すために用いられます。

---

## 🔸 よく使われる場面

### ① `List.replaceAll(UnaryOperator<E>)`

```java
List<String> list = new ArrayList<>(List.of("a", "b", "c"));

list.replaceAll(s -> s.toUpperCase());
System.out.println(list);  // [A, B, C]
```

ここでは各要素を大文字に変換する `UnaryOperator<String>` を使っています。

### ② Function合成（`andThen`, `compose`）

```java
UnaryOperator<String> trim = s -> s.trim();
UnaryOperator<String> quote = s -> "\"" + s + "\"";

UnaryOperator<String> pipeline = trim.andThen(quote);

System.out.println(pipeline.apply("  hello  "));  // "hello"
```

---

## 🔸 `UnaryOperator.identity()`

```java
UnaryOperator<String> identity = UnaryOperator.identity();
System.out.println(identity.apply("same"));  // 出力: same
```

- 引数をそのまま返す「恒等関数」
- デフォルト実装として提供されており、必要に応じて使えます

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 🔧 目的 | 「同じ型の変換処理」を明確に表す |
| 🔄 構造 | `Function<T, T>` の特化版 |
| 🎯 使用例 | `List.replaceAll()`, 関数合成 |
| 🔍 利点 | 読み手に「型が変わらない変換」であると明確に伝わる |
| 🛠️ 便利機能 | `UnaryOperator.identity()` は恒等関数 |

---

## 🚀 学習の次ステップ提案

- `UnaryOperator` を使った **パターン問題（Silver / Gold スタイル）**
- `UnaryOperator` vs `Function` の使い分けトレーニング
- `UnaryOperator` をチェーンして「テキスト整形パイプライン」作成（実務的）

以下に **`UnaryOperator<T>` に関する Java Silver / Gold 試験スタイルのパターン問題** を作成しました。`apply()` の基本使用から、`andThen()` や `replaceAll()` との組み合わせまで含めています。

---

## 🧠 UnaryOperator パターン問題（Java Silver / Gold 風）

---

### 【問1】基本構文（Silverレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.UnaryOperator;

public class Main {
    public static void main(String[] args) {
        UnaryOperator<Integer> square = x -> x * x;
        System.out.println(square.apply(5));
    }
}
```

### 選択肢：

A. 10

B. 25

C. コンパイルエラー

D. 実行時例外

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問1 | **B** | `5 * 5 = 25`。`UnaryOperator<Integer>` で `apply(5)` を実行 |

---

### 【問2】文字列変換（Silver〜Gold）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.UnaryOperator;

public class Main {
    public static void main(String[] args) {
        UnaryOperator<String> quote = s -> "\"" + s + "\"";
        System.out.println(quote.apply("Java"));
    }
}
```

### 選択肢：

A. "Java"

B. "Java"

C. "Java

D. Java"

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問2 | **A** | 文字列 `"Java"` を `"` で囲む → `"Java"`。文字列リテラルとして出力される |

---

### 【問3】andThen 合成（Gold）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.UnaryOperator;

public class Main {
    public static void main(String[] args) {
        UnaryOperator<String> trim = s -> s.trim();
        UnaryOperator<String> toUpper = s -> s.toUpperCase();

        UnaryOperator<String> composed = trim.andThen(toUpper);

        System.out.println(composed.apply("  java  "));
    }
}
```

### 選択肢：

A.   JAVA

B. JAVA

C. java

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問3 | **B** | `"  java  "` → `trim()` で `"java"` → `toUpperCase()` で `"JAVA"` |

---

### 【問4】Listとの連携（Gold応用）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.*;
import java.util.function.UnaryOperator;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(List.of("a", "b", "c"));
        list.replaceAll(s -> s + s);
        System.out.println(list);
    }
}
```

### 選択肢：

A. [a, b, c]

B. [aa, bb, cc]

C. [ab, bc, ca]

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問4 | **B** | `"a" → "aa"`, `"b" → "bb"`, `"c" → "cc"` → `replaceAll` は `UnaryOperator<String>` を受け取る |

---

## 🔍 よく問われるポイント（試験的に重要）

- `UnaryOperator<T>` は `Function<T, T>` のサブタイプ → 引数と戻り値の型が同じであるときに使う
- `List#replaceAll(UnaryOperator<E>)` に渡せる
- 合成（`andThen`, `compose`）は `Function` 同様に利用できる

---

## 🧩 次のステップ（ご希望あれば）

- 穴埋め形式（`UnaryOperator<String> = ???;` を補う）
- `UnaryOperator` チェーンでテキスト整形パイプライン作成問題
- `Function` / `UnaryOperator` / `Predicate` の混合比較問題