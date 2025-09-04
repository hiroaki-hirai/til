# BiPredicate

ここでは `BiPredicate` インターフェイスについて、**基本から応用まで**体系的に解説します。

---

## 🔷 `BiPredicate<T, U>` とは？

`BiPredicate` は、**2つの引数**を取り、**boolean値（真偽）を返す関数型インターフェイス**です。

```java
@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
}
```

- `T`, `U` はそれぞれ異なる型の引数
- `test(T t, U u)` メソッドで 2つの値に対する条件判定を行う
- `Predicate<T>` が1引数に対して判定するのに対し、`BiPredicate` は**2引数**を使える点がポイントです

---

## 🔸 例：文字列と整数を比較する

```java
BiPredicate<String, Integer> longerThan = (str, len) -> str.length() > len;

System.out.println(longerThan.test("Java", 3)); // true
System.out.println(longerThan.test("Hi", 5));   // false
```

このように、文字列と長さという**異なる型の2値**を使って判定できます。

---

## 🔸 主な用途・特徴

| 特徴 | 説明 |
| --- | --- |
| 🧪 条件判定 | 2つの値の組に対して条件を確認できる |
| ✅ 再利用性 | 関数として条件を渡すことができる（高階関数） |
| 🔄 合成可能 | `and`, `or`, `negate` を使って複数条件の合成が可能 |

---

## 🔸 defaultメソッド

`Predicate` 同様、以下のメソッドを使って**条件の合成**ができます：

- `and(BiPredicate other)`
- `or(BiPredicate other)`
- `negate()`

### 例：2つの文字列が同じ長さ & 同じ先頭文字かどうか

```java
BiPredicate<String, String> sameLength = (a, b) -> a.length() == b.length();
BiPredicate<String, String> sameStart = (a, b) -> a.charAt(0) == b.charAt(0);

BiPredicate<String, String> combined = sameLength.and(sameStart);

System.out.println(combined.test("Hello", "House")); // true（長さ5、Hで始まる）
System.out.println(combined.test("Cat", "Dog"));     // false（長さは同じだが先頭が異なる）
```

---

## 🔸 `BiPredicate` と `Stream` の関係

`BiPredicate` は `Stream` API 単体では直接使われることはあまりありません（filterは1引数）。

ただし、**独自のフィルタ関数を作成する**場合や、**2値を扱うデータ構造（Map、ペアなど）**での判定に活用できます。

### 例：`Map<K, V>` のエントリに対する条件チェック

```java
Map<String, Integer> scores = Map.of("Alice", 80, "Bob", 65, "Carol", 90);

BiPredicate<String, Integer> highScore = (name, score) -> score >= 70;

scores.forEach((k, v) -> {
    if (highScore.test(k, v)) {
        System.out.println(k + " passed");
    }
});
```

---

## 🔸 `Predicate` との違いまとめ

| インターフェイス | 引数の数 | 返り値 | 用途 |
| --- | --- | --- | --- |
| `Predicate<T>` | 1 | boolean | 単一の値に対する条件チェック |
| `BiPredicate<T,U>` | 2 | boolean | 2つの値にまたがる条件チェック（例：比較・関連性） |

---

## ✅ まとめ

- `BiPredicate<T, U>` は 2つの引数を受けて真偽値を返す関数型インターフェイス
- 条件の合成に便利な `and`, `or`, `negate` メソッドが使える
- `Map` や複数パラメータの比較処理に有効
- `Predicate` より柔軟性が高く、条件の幅を広げたい場合に有用

以下に `BiPredicate` に関する **Java Silver/Gold 試験スタイルのパターン問題** を用意しました。基本的な構文から、defaultメソッドの使い方、応用的な使い方までカバーしています。

---

## 🧠 BiPredicate に関するパターン問題

---

### 【問1】基本構文（Silverレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BiPredicate;

public class Main {
    public static void main(String[] args) {
        BiPredicate<String, Integer> checkLength = (str, len) -> str.length() == len;

        System.out.println(checkLength.test("Java", 4));
    }
}
```

### 選択肢：

A. true

B. false

C. コンパイルエラー

D. 実行時例外

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問1 | A | `"Java"` の長さは 4、引数も 4 → `true` が出力される |

---

### 【問2】defaultメソッド（Goldレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BiPredicate;

public class Main {
    public static void main(String[] args) {
        BiPredicate<String, String> sameStart = (a, b) -> a.charAt(0) == b.charAt(0);
        BiPredicate<String, String> sameLength = (a, b) -> a.length() == b.length();

        BiPredicate<String, String> combined = sameStart.and(sameLength);

        System.out.println(combined.test("Apple", "Alien"));
        System.out.println(combined.test("Dog", "Dino"));
    }
}
```

### 選択肢：

A. true\nfalse

B. false\ntrue

C. true\ntrue

D. false\nfalse

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問2 | **A** | `"Apple", "Alien"` → 先頭文字 `'A'` 一致、長さも 5 で一致 → `true"Dog", "Dino"` → 先頭 `'D'` は一致だが長さが 3 ≠ 4 → `false` |

---

### 【問3】Mapとの連携（Gold応用）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.*;
import java.util.function.BiPredicate;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> scores = Map.of("Alice", 75, "Bob", 60, "Charlie", 85);

        BiPredicate<String, Integer> isPassed = (name, score) -> score >= 70;

        scores.forEach((name, score) -> {
            if (isPassed.test(name, score)) {
                System.out.print(name + " ");
            }
        });
    }
}
```

### 選択肢：

A. Alice Bob Charlie

B. Alice Charlie

C. Charlie

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問3 | B | `>= 70` を満たすのは `"Alice"`（75）と `"Charlie"`（85）→ `"Alice Charlie"` |