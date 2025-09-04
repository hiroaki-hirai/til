# BinaryOperator

ここでは `BinaryOperator<T>` について、**基本構文・`BiFunction`との違い・使いどころ・実務的な応用**を含めて体系的に解説します。

---

## 🔷 BinaryOperator<T> とは？

`BinaryOperator<T>` は、Java 8 で導入された関数型インターフェイスで、

> 「同じ型の2つの引数を取り、同じ型の結果を返す」 処理を記述するために使います。
> 

```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T, T, T> {
    static <T> BinaryOperator<T> minBy(Comparator<? super T> comparator)
    static <T> BinaryOperator<T> maxBy(Comparator<? super T> comparator)
}
```

- `T`：引数と戻り値の共通の型
- 親インターフェイス：`BiFunction<T, T, T>`
- つまり `BiFunction` の特殊ケース（引数・戻り値すべて同じ型）

---

## 🔸 主な用途

| 用途 | 具体例 |
| --- | --- |
| 数値の計算 | 加算・乗算・最大値・最小値など |
| 文字列の結合 | `"Hello" + "World"` のような処理 |
| Streamの集約 | `reduce(BinaryOperator)` との併用 |

---

## 🔸 基本例

### ① 整数の加算

```java
import java.util.function.BinaryOperator;

BinaryOperator<Integer> add = (a, b) -> a + b;
System.out.println(add.apply(3, 5));  // 出力: 8
```

### ② 文字列の連結

```java
BinaryOperator<String> concat = (s1, s2) -> s1 + s2;
System.out.println(concat.apply("Java", "17"));  // 出力: Java17
```

---

## 🔸 `BiFunction` との違い

| インターフェイス | 引数の型 | 戻り値の型 | 説明 |
| --- | --- | --- | --- |
| `BiFunction<T, U, R>` | T, U（異なってよい） | R | 汎用的な関数インターフェイス |
| `BinaryOperator<T>` | T, T | T | 型がすべて同じときに使う特化形 |

💡 `BiFunction<T, T, T>` を使っても同じことはできますが、`BinaryOperator<T>` を使うと「同じ型の計算処理」であることが明示的に伝わります。

---

## 🔸 Streamとの連携（`reduce()`）

```java
List<Integer> nums = List.of(1, 2, 3, 4);

int sum = nums.stream()
              .reduce(0, (a, b) -> a + b);  // BinaryOperator<Integer>
System.out.println(sum);  // 出力: 10
```

---

## 🔸 `minBy`, `maxBy`（Comparatorとの併用）

```java
BinaryOperator<String> shorter = BinaryOperator.minBy(Comparator.comparingInt(String::length));
System.out.println(shorter.apply("Java", "Spring"));  // 出力: Java
```

- `minBy` → 小さい方を返す
- `maxBy` → 大きい方を返す

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 📌 目的 | 同じ型の 2 つの引数に対して処理を行い、同じ型の結果を返す |
| 🧱 親インターフェイス | `BiFunction<T, T, T>` |
| ✅ 主な用途 | 数値演算、文字列結合、`reduce()`、大小比較（`minBy` / `maxBy`） |
| 💡 利点 | 処理対象が「同一型」であることを明確に伝えられる |

---

## 🚀 次のステップ案

- BinaryOperator を使った **Silver / Gold 風のパターン問題**
- `minBy`, `maxBy` を使ったストリーム応用（実務的）
- `BiFunction` / `BinaryOperator` の比較・変換練習問題

続けて問題に進みますか？それとも応用パターンを見てみたいですか？

以下に **`BinaryOperator<T>` に関する Java Silver / Gold スタイルのパターン問題**を用意しました。基本的な構文、`reduce()`との併用、`minBy`/`maxBy`の利用まで含んでいます。

---

## 🧠 BinaryOperator パターン問題（Java Silver / Gold 風）

---

### 【問1】基本構文（Silverレベル）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BinaryOperator;

public class Main {
    public static void main(String[] args) {
        BinaryOperator<Integer> multiply = (a, b) -> a * b;
        System.out.println(multiply.apply(2, 4));
    }
}
```

### 選択肢：

A. 6

B. 8

C. 24

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問1 | **B** | `2 * 4 = 8`。基本的な `BinaryOperator<Integer>` の使用例 |

---

### 【問2】文字列の結合（Silver〜Gold）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.function.BinaryOperator;

public class Main {
    public static void main(String[] args) {
        BinaryOperator<String> join = (a, b) -> a + "-" + b;
        System.out.println(join.apply("Java", "17"));
    }
}
```

### 選択肢：

A. Java17

B. Java-17

C. Java - 17

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問2 | **B** | `"Java" + "-" + "17"` → `"Java-17"` |

---

### 【問3】Stream + reduce（Gold）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.*;
import java.util.function.BinaryOperator;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3, 4);
        BinaryOperator<Integer> sum = (a, b) -> a + b;

        int result = list.stream().reduce(0, sum);
        System.out.println(result);
    }
}
```

### 選択肢：

A. 9

B. 10

C. 11

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問3 | **B** | `reduce(0, sum)` → 0 + 1 + 2 + 3 + 4 = **10** |

---

### 【問4】minBy（Gold応用）

次のコードの出力として正しいものを 1 つ選びなさい。

```java
import java.util.Comparator;
import java.util.function.BinaryOperator;

public class Main {
    public static void main(String[] args) {
        BinaryOperator<String> shorter = BinaryOperator.minBy(Comparator.comparingInt(String::length));
        System.out.println(shorter.apply("Spring", "Boot"));
    }
}
```

### 選択肢：

A. Boot

B. Spring

C. 4

D. コンパイルエラー

| 問題 | 正解 | 解説 |
| --- | --- | --- |
| 問4 | **A** | `minBy(length)` → `"Boot"`（長さ4） vs `"Spring"`（長さ6） → 短い `"Boot"` |

---

## 🔍 補足学習ポイント

- `reduce()` の初期値と結合関数は `BinaryOperator<T>` で書ける（試験頻出）
- `minBy()` / `maxBy()` の引数には `Comparator<? super T>` が必要
- 文字列連結や加算など「**同じ型の2値→1値**」処理は BinaryOperator の代表例

---

## 🚀 ご希望に応じて…

- `minBy`, `maxBy` を List ストリーム上で使う応用問題
- `BiFunction` との使い分け・変換クイズ
- 穴埋め問題や「選択肢並べ替え」形式の応用出題

# Q,

BinaryOperator<int> といった型の指定はできますか？

いい質問です。

結論から言うと：

---

## ❌ `BinaryOperator<int>` は使用できません。

---

## 🔍 理由：プリミティブ型 `int` はジェネリクスに使えないから

Java の **ジェネリクス（例：BinaryOperator<T>）** は、**オブジェクト型（参照型）** のみを扱います。

`int` や `double` のような **プリミティブ型** はジェネリクスの型パラメータには使えません。

そのため以下のような記述は **コンパイルエラー** になります：

```java
BinaryOperator<int> op = (a, b) -> a + b; // ❌ エラー
```

---

## ✅ 正しい書き方：ラッパークラスを使う

プリミティブ型には、それぞれに対応する **ラッパークラス** があるため、それを使います。

```java
BinaryOperator<Integer> op = (a, b) -> a + b;
```

---

## 🎯 代替：プリミティブ特化の関数型インタフェースを使う

Java 8 以降では、**パフォーマンス向上のために** プリミティブ型専用の関数型インタフェースが用意されています。

### ✅ `IntBinaryOperator`（int → int → int）

```java
import java.util.function.IntBinaryOperator;

IntBinaryOperator op = (a, b) -> a + b;
int result = op.applyAsInt(5, 3); // → 8
```

他にも以下のようなものがあります：

| プリミティブ型 | 二項演算インタフェース名 |
| --- | --- |
| `int` | `IntBinaryOperator` |
| `long` | `LongBinaryOperator` |
| `double` | `DoubleBinaryOperator` |

---

## ✅ まとめ

| 記述 | 可否 | 理由 |
| --- | --- | --- |
| `BinaryOperator<int>` | ❌ | ジェネリクスでプリミティブ型は使えない |
| `BinaryOperator<Integer>` | ✅ | ラッパークラスを使えばOK |
| `IntBinaryOperator` | ✅ | プリミティブ専用インタフェース |

---

ご希望であれば、これらを使ったパターン問題や、`BinaryOperator` と `IntBinaryOperator` のパフォーマンス・用途比較などもご紹介できます！