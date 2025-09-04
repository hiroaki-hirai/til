# max()

`Math.max()` は Java における **2つの数値のうち大きい方を返すメソッド**です。

基本的なメソッドですが、**オーバーロードの種類が多く、型変換の罠や精度の問題が試験で狙われやすいポイント**です。

---

## ✅ `Math.max()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `max()` |
| 用途 | **2つの値のうち、より大きい値を返す** |
| 戻り値の型 | 引数と同じ（オーバーロードで型別対応） |

---

## 🔹 シグネチャ（オーバーロード一覧）

```java
public static int max(int a, int b)
public static long max(long a, long b)
public static float max(float a, float b)
public static double max(double a, double b)
```

✅ **プリミティブ型（4種類）それぞれに対応**しています。

---

## 🔹 使用例

```java
System.out.println(Math.max(10, 20));        // → 20
System.out.println(Math.max(5.5, 2.2));      // → 5.5
System.out.println(Math.max(-3, -7));        // → -3
System.out.println(Math.max(4.0f, 7.0f));    // → 7.0
```

---

## 🔸 特殊値の扱い（浮動小数点）

| 式 | 結果 | 説明 |
| --- | --- | --- |
| `Math.max(Double.NaN, 10)` | `10.0` | `NaN` ではない方を優先 |
| `Math.max(Float.NaN, -5)` | `-5.0` | 同上 |
| `Math.max(Double.NaN, NaN)` | `NaN` | 両方 `NaN` のときのみ `NaN` |

---

## 🔹 型変換の注意点

```java
System.out.println(Math.max(10, 10L));     // コンパイルエラー！
```

✅ 上記は **`int` と `long` の混合** → どちらの `max()` を使うか不明 → 明示的にキャストする必要があります。

```java
System.out.println(Math.max(10, (int)10L));   // OK
System.out.println(Math.max((long)10, 10L));  // OK
```

---

## 🔁 `Math.min()` との対比

| メソッド | 戻り値 | 用途 |
| --- | --- | --- |
| `Math.max(a, b)` | aとbのうち**大きい方** | 最大値の取得 |
| `Math.min(a, b)` | aとbのうち**小さい方** | 最小値の取得 |

---

## ✅ よくある誤解と注意点

| 誤解内容 | 正しい理解 |
| --- | --- |
| `Math.max()` は整数専用？ | ❌ 浮動小数点 (`float`, `double`) にも対応 |
| 異なる型を混ぜてもOK？ | ❌ 型が違うとコンパイルエラー（キャストが必要） |
| `max(NaN, x)` は常に `NaN`？ | ❌ `x` が返る（NaNでない方を返す） |
| `max()` は複数個の引数をとれる？ | ❌ 2つだけ。3つ以上の場合は**ネストするかStreamで処理** |

---

## ✍️ 応用：複数の値の最大値を求めたいとき

```java
int max = Math.max(Math.max(a, b), c);
```

または Java 8 以降：

```java
int max = Stream.of(a, b, c).max(Integer::compare).get();
```

---

## 🔺 `Math.max()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：オーバーロードと型の一致

```java
System.out.println(Math.max(5, 10L));
```

このコードの結果は？

A. `10`

B. `10L`

C. `コンパイルエラー`

D. `例外発生`

**答え**：C. `コンパイルエラー`

**解説**：

`Math.max()` は**同じ型のペア**でしか動作しません。

→ `int` と `long` を混ぜるとオーバーロード解決できず、**コンパイルエラー**。

✅ 対処例：

```java
System.out.println(Math.max((long)5, 10L)); // OK
```

---

### ❓ 問題 2：NaNを含む比較

```java
System.out.println(Math.max(Double.NaN, 100.0));
```

A. `NaN`

B. `100.0`

C. `0.0`

D. コンパイルエラー

**答え**：B. `100.0`

**解説**：

`Math.max()` は **NaN ではない方を返します**。

→ `Math.max(NaN, x)` = `x`（※ 両方 NaN のときのみ `NaN`）

---

### ❓ 問題 3：`Math.max()` の戻り値型

```java
var result = Math.max(3.0f, 7.0f);
System.out.println(((Object)result).getClass().getSimpleName());
```

A. `Double`

B. `Float`

C. `Integer`

D. `Long`

**答え**：B. `Float`

**解説**：

`Math.max(float, float)` を使った場合、**戻り値の型も `float`**。

`double` と混同しやすいので注意。

---

### ❓ 問題 4：負の値の比較

```java
System.out.println(Math.max(-5, -2));
```

A. `-5`

B. `-2`

C. `0`

D. `2`

**答え**：B. `-2`

**解説**：

最大値なので、**より「大きい」負数**が選ばれます。

（-2 > -5）

---

### ❓ 問題 5：3つ以上の値を比較する場合

```java
int a = 3, b = 9, c = 6;
int max = Math.max(a, Math.max(b, c));
System.out.println(max);
```

A. `6`

B. `9`

C. コンパイルエラー

D. 常に `a` の値

**答え**：B. `9`

**解説**：

`Math.max()` は**2つしか受け取れない**ため、**ネストして使う**必要があります。

→ `Math.max(a, Math.max(b, c))` は正しい使い方。

---

## ✅ よくある誤解まとめ

| 誤解内容 | 実際は… |
| --- | --- |
| `Math.max(int, long)` はOK？ | ❌ コンパイルエラー。型を合わせる必要あり |
| `Math.max()` の戻り値は常に `double`？ | ❌ 引数に応じて `int`, `long`, `float`, `double` に分かれる |
| `max(NaN, x)` は `NaN` を返す？ | ❌ NaN でない方を返す |
| `Math.max(a, b, c)` のように使える？ | ❌ 2つまで。3つ以上は **ネストして処理** |