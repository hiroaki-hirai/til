# BiFunction

ここでは `BiFunction<T, U, R>` インターフェイスについて、**基本構文・使い方・Functionとの違い・応用例**を含めて体系的に解説します。

---

## 🔷 `BiFunction<T, U, R>` とは？

`BiFunction` は、**2つの引数を受け取り、1つの戻り値を返す**ための関数型インターフェイスです。

```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
    R apply(T t, U u);
}
```

- `T`：第1引数の型
- `U`：第2引数の型
- `R`：戻り値の型
- `apply(T t, U u)` で処理を実行

---

## 🔸 どんなときに使う？

- 2つの値を使って何かを**計算**・**生成**・**変換**したいとき
- 例えば：
    - 2つの整数を加算する
    - ユーザー情報（名前＋年齢）から `Person` インスタンスを作る
    - `Map#replaceAll()` の処理に使用

---

## 🔸 基本例：2つの数の加算

```java
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;

System.out.println(add.apply(3, 5));  // 出力: 8
```

---

## 🔸 応用例：文字列と数値を結合

```java
BiFunction<String, Integer, String> label = (name, age) -> name + " is " + age + " years old.";

System.out.println(label.apply("Alice", 30));  // Alice is 30 years old.
```

---

## 🔸 関数の合成：`andThen(Function<R, V>)`

戻り値（R）にさらに処理を加える場合、以下のように合成できます。

```java
BiFunction<String, String, String> concat = (a, b) -> a + b;
Function<String, Integer> length = s -> s.length();

BiFunction<String, String, Integer> combined = concat.andThen(length);

System.out.println(combined.apply("Hello", "World"));  // 出力: 10
```

- `concat` → "HelloWorld"
- `length` → 10

---

## 🔸 `Function` / `BiFunction` / `UnaryOperator` の違い

| インターフェイス | 引数の数 | 戻り値 | 典型用途 |
| --- | --- | --- | --- |
| `Function<T, R>` | 1 | R | 変換処理 |
| `BiFunction<T, U, R>` | 2 | R | 2引数の変換・結合処理 |
| `UnaryOperator<T>` | 1（T） | T | T → Tの変換（Functionの特殊形） |

---

## 🔸 実務での活用例

### Mapの値を変換（`Map#replaceAll()`）

```java
Map<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);

// key と value を使って変換
map.replaceAll((k, v) -> v * 10);

System.out.println(map);  // {A=10, B=20}
```

※ `replaceAll(BiFunction<K, V, V>)` で使われる

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 🌟 特徴 | 2引数 → 1戻り値の処理が書ける |
| 🎯 目的 | 計算・結合・インスタンス生成など |
| 🔗 合成 | `andThen()` で処理の連結が可能 |
| 📦 活用例 | `Map#replaceAll()` など |

---

## 🚀 次におすすめの学習

- `BiFunction` に関する **Silver/Goldスタイルのパターン問題**
- `BiFunction` と `Predicate` の組み合わせ応用
- `BiFunction` を使ったオブジェクト生成ファクトリ関数

以下に **`BiFunction<T, U, R>` に関する Java Silver / Gold 試験スタイルのパターン問題** をご用意しました。基本構文から、合成（`andThen`）や実務利用（Mapとの連携）までを網羅しています。

---

## 🧠 `BiFunction` パターン問題（Java Silver / Gold 風）

---

### 【問1】基本構文（Silverレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {
        BiFunction<Integer, Integer, Integer> multiply = (a, b) -> a * b;

        System.out.println(multiply.apply(3, 4));
    }
}
```

### 選択肢：

A. 7

B. 12

C. コンパイルエラー

D. 実行時例外

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問1 | **B** | 3 × 4 = 12。単純な multiply 関数の実行結果 |

---

### 【問2】文字列と数値の結合（Silver〜Gold）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {
        BiFunction<String, Integer, String> label = (name, age) -> name + " (" + age + ")";

        System.out.println(label.apply("Hiro", 30));
    }
}
```

### 選択肢：

A. Hiro 30

B. Hiro(30)

C. Hiro (30)

D. Hiro - 30

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問2 | **C** | `"Hiro" + " (" + 30 + ")"` → `"Hiro (30)"` |

---

### 【問3】andThenの利用（Goldレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BiFunction;
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        BiFunction<String, String, String> concat = (a, b) -> a + b;
        Function<String, Integer> length = s -> s.length();

        BiFunction<String, String, Integer> composed = concat.andThen(length);

        System.out.println(composed.apply("abc", "defg"));
    }
}
```

### 選択肢：

A. 7

B. 6

C. コンパイルエラー

D. defgabc

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問3 | **A** | `"abc" + "defg"` → `"abcdefg"` → `.length()` = 7 |

---

### 【問4】Mapとの連携（Gold応用）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.*;
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("A", 10);
        map.put("B", 20);

        map.replaceAll((key, value) -> value + 5);

        System.out.println(map);
    }
}
```

### 選択肢：

A. {A=15, B=25}

B. {A=5, B=15}

C. {A=10, B=20}

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問4 | **A** | `replaceAll((k, v) -> v + 5)` → 値をそれぞれ +5 → {A=15, B=25} |

---

### 📝 補足

- `andThen()` による合成は **「最初にBiFunction → 次にFunction」** の順です。
- `Map.replaceAll()` は `BiFunction<K, V, V>` を受け取ります。

---

## 🚀 ご希望に応じて

- 穴埋め式（`apply` の型やメソッド名を書かせる形式）
- `BiFunction` を使ったオブジェクト生成（例：名前＋IDから `User` インスタンス）
- `BiFunction` を `Function<T, Function<U, R>>` にカリー化する発展問題