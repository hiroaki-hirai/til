# Function

ここでは `Function<T, R>` インターフェイスについて、**基本構文から応用・比較まで**体系的に解説します。

---

## 🔷 `Function<T, R>` とは？

`Function<T, R>` は、Java 8 で導入された**関数型インターフェイス**で、

**「T型の入力を受け取り、R型の結果を返す」**処理を記述するために使います。

- `T`：入力の型（引数）
- `R`：出力の型（戻り値）
- `apply(T t)` メソッドで処理を実行します

---

## 🔸 基本例

### 文字列を大文字に変換する Function

```java
Function<String, String> toUpper = s -> s.toUpperCase();
System.out.println(toUpper.apply("java"));  // 出力: JAVA
```

---

## 🔸 主な用途

| 用途 | 具体例 |
| --- | --- |
| 値の変換 | 文字列 → 整数、オブジェクト → プロパティ抽出など |
| Stream API | `map()` に渡して変換処理を記述 |
| カスタムロジックの引数化 | 戻り値のある処理のパラメータ化（高階関数的に） |

```java
List<String> words = List.of("java", "function", "stream");

List<Integer> lengths = words.stream()
                             .map(s -> s.length())  // Function<String, Integer>
                             .collect(Collectors.toList());

System.out.println(lengths);  // [4, 8, 6]
```

---

## 🔸 defaultメソッドによる合成（`andThen`, `compose`）

| メソッド | 処理順 | 説明 |
| --- | --- | --- |
| `andThen` | 自分 → 引数 | f.andThen(g)：fを実行してからg |
| `compose` | 引数 → 自分 | f.compose(g)：gを実行してからf |

```java
Function<String, String> trim = s -> s.trim();
Function<String, Integer> length = s -> s.length();

Function<String, Integer> trimThenLength = trim.andThen(length);

System.out.println(trimThenLength.apply("  hello  "));  // 5
```

---

## 🔸 他の関数型インターフェイスとの違い

| インターフェイス | 引数 | 戻り値 | 用途 |
| --- | --- | --- | --- |
| `Function<T, R>` | 1 | あり (R) | 値の変換 |
| `Predicate<T>` | 1 | boolean | 条件判定 |
| `Consumer<T>` | 1 | void | 実行（副作用あり） |
| `Supplier<T>` | なし | あり (T) | 値の生成 |
| `UnaryOperator<T>` | T | T | T→T変換（Functionの特殊形） |
| `BiFunction<T, U, R>` | 2 | R | 2引数の値変換 |

---

## 🔸 よくある使い方のパターン

- オブジェクトのプロパティ抽出：

```java
Function<Person, String> getName = p -> p.getName();
```

- 任意の処理の合成（パイプライン）：

```java
Function<String, String> pipeline = s -> s.trim().toUpperCase();
```

- `Map` の value変換：

```java
Map<String, Integer> map = Map.of("A", 1, "B", 2);
map.replaceAll((k, v) -> v * 10);  // BiFunction
```

---

## ✅ まとめ

- `Function<T, R>` は「**1つの入力から何かを計算して返す処理**」に最適
- `map()` などの変換処理で広く使われる
- `andThen()` / `compose()` により関数の連結が可能
- `Predicate` や `Consumer` などとの違いに注意

以下に `Function<T, R>` に関する **Java Silver / Gold 試験スタイルのパターン問題**を用意しました。基本構文から `andThen` / `compose` の理解を試すものまで含まれています。

---

## 🧠 Function<T, R> に関するパターン問題

---

### 【問1】基本構文（Silverレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, Integer> strLen = s -> s.length();
        System.out.println(strLen.apply("Java"));
    }
}
```

### 選択肢：

A. 3

B. 4

C. コンパイルエラー

D. 実行時例外

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問1 | **B** | `"Java"` の長さは 4 → `Function<String, Integer>` の `apply("Java")` は 4 を返す |

---

### 【問2】Streamとの連携（Silver〜Gold）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("a", "bb", "ccc");
        Function<String, Integer> len = s -> s.length();

        List<Integer> result = list.stream()
                                   .map(len)
                                   .collect(Collectors.toList());

        System.out.println(result);
    }
}
```

### 選択肢：

A. [1, 2, 3]

B. [3, 2, 1]

C. [a, bb, ccc]

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問2 | **A** | `"a", "bb", "ccc"` → 各要素の長さを `map()` で取得 → `[1, 2, 3]` |

---

### 【問3】andThen の理解（Goldレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> trim = s -> s.trim();
        Function<String, Integer> len = s -> s.length();

        Function<String, Integer> pipeline = trim.andThen(len);

        System.out.println(pipeline.apply("  hello  "));
    }
}
```

### 選択肢：

A. 7

B. 5

C. コンパイルエラー

D. 実行時例外

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問3 | **B** | `"  hello  "` → `trim()` で `"hello"` → `length()` で 5 |

---

### 【問4】compose の理解（Gold応用）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> addPrefix = s -> "Hello " + s;
        Function<String, String> toUpper = s -> s.toUpperCase();

        Function<String, String> composed = addPrefix.compose(toUpper);

        System.out.println(composed.apply("world"));
    }
}
```

### 選択肢：

A. Hello world

B. HELLO WORLD

C. Hello WORLD

D. WORLD Hello

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問4 | **C** | `"world"` → `toUpper()` → `"WORLD"` → `addPrefix()` → `"Hello WORLD"` |

---

## 📝 補足：`andThen` vs `compose` の順序比較

```java
f.andThen(g).apply(x) → g(f(x))  // fを先に、gを後に  
f.compose(g).apply(x) → f(g(x))  // gを先に、fを後に  
```

---

ご希望があれば、以下のような発展問題も用意できます：

- `UnaryOperator` や `BiFunction` との使い分け問題
- `Function` のカスタム高階関数応用
- 穴埋め式のコード問題や順序を問う問題（ラムダ式チェーン）