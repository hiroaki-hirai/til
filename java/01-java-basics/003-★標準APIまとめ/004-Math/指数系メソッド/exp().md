# exp()

`Math.exp()` は、Javaで **ネイピア数（自然対数の底）e ≒ 2.71828 のべき乗を計算する**メソッドです。

指数関数や対数関数の分野で頻出する、**数学的に非常に重要な関数**の1つです。

---

## ✅ `Math.exp()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `exp()` |
| 用途 | **e（自然対数の底）を指定した指数で累乗する** |
| 修飾子 | `public static`（staticメソッド） |
| 戻り値の型 | `double` |

---

## 🔹 メソッド定義（シグネチャ）

```java
public static double exp(double a)
```

- 引数 `a`：e の指数（e^a）
- 戻り値：**e の a 乗**

---

## 🔹 使用例

```java
System.out.println(Math.exp(1));    // → 2.718281828459045（e^1）
System.out.println(Math.exp(2));    // → 7.3890560989306495（e^2）
System.out.println(Math.exp(0));    // → 1.0（e^0 = 1）
System.out.println(Math.exp(-1));   // → 0.36787944117144233（1/e）
```

---

## 🔸 特徴と注意点

| 観点 | 内容 |
| --- | --- |
| 基本定義 | `Math.exp(a)` は **e^a** を返す |
| e（ネイピア数） | 約 2.718281828459045 |
| 戻り値の型 | `double` 型（指数が整数でも戻り値は常に `double`） |
| 負の指数 | `Math.exp(-a)` → `1 / (e^a)`（指数関数の性質） |
| 精度 | 浮動小数点の誤差に注意が必要 |

---

## 🔹 関連メソッド

| メソッド | 概要 | 例 |
| --- | --- | --- |
| `Math.exp(a)` | `e^a` を返す | `Math.exp(2)` → `e² ≒ 7.39` |
| `Math.log(a)` | **自然対数 logₑ(a)** を返す | `Math.log(e)` → `1.0` |
| `Math.pow(a, b)` | `a` の `b` 乗 | `Math.pow(e, 2)` → 同じ |

※ `exp` と `log` は**逆関数の関係**です：

> Math.exp(Math.log(x)) == x（誤差がなければ）
> 

---

## 🔹 実用例

① 指数関数的増加（人口・感染症モデルなど）

```java
double time = 5;
double result = Math.exp(time);  // e^5 → 約 148.41
```

② ソフトマックス関数などの機械学習の計算

```java
double x1 = 1.0;
double x2 = 2.0;
double sum = Math.exp(x1) + Math.exp(x2);
double p1 = Math.exp(x1) / sum;
double p2 = Math.exp(x2) / sum;
```

---

## ✅ よくある誤解と注意点

| 誤解 | 実際は… |
| --- | --- |
| `exp(x)` は「x の累乗」？ | ❌ `e` の `x` 乗（底は固定で e） |
| `exp(x)` と `pow(x, e)` は同じ？ | ❌ 逆です：`exp(x)` = `pow(e, x)` |
| `exp(0)` はエラーになる？ | ❌ `1.0` を返す（e^0 = 1） |
| 負の数を入れるとエラー？ | ❌ `exp(-x)` は `1/e^x` となる（正しい） |

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 目的 | `e` の任意の指数乗（e^x）を計算 |
| 数学的な用途 | 指数関数・統計・物理・金融モデリングなど |
| 戻り値 | 常に `double` |
| 関連メソッド | `Math.log()`（自然対数） |

---

## 🔺 `Math.exp()` 引っかけ問題集（全5問）

---

### ❓ 問題1：戻り値の型

```java
var result = Math.exp(2);
System.out.println(((Object)result).getClass().getSimpleName());
```

A. `Integer`

B. `Double`

C. `Float`

D. `BigDecimal`

**答え**：B. `Double`

**解説**：

`Math.exp(double a)` は戻り値が **常に `double`**。

たとえ指数が整数でも、結果は `7.389...` のような浮動小数点数になります。

---

### ❓ 問題2：指数の意味

```java
System.out.println(Math.exp(0));
```

A. `0.0`

B. `1.0`

C. `NaN`

D. コンパイルエラー

**答え**：B. `1.0`

**解説**：

指数関数の基本性質：

> e^0 = 1
> 
> 
> → `Math.exp(0)` は **必ず `1.0`** を返します。
> 

---

### ❓ 問題3：負の指数の意味

```java
System.out.println(Math.exp(-1));
```

A. `-2.718...`

B. `0.0`

C. `1 / e ≒ 0.3678`

D. コンパイルエラー

**答え**：C. `1 / e ≒ 0.3678`

**解説**：

負の指数は「**逆数**」になります。

→ `e^(-1)` = `1 / e` ≒ `0.367879...`

→ 符号がついて負になるわけではない点に注意！

---

### ❓ 問題4：`Math.pow()`との混同

```java
System.out.println(Math.pow(Math.E, 2));
System.out.println(Math.exp(2));
```

2つの出力は一致するか？

A. 常に一致する

B. 近いが微妙にずれる

C. 全く異なる値

D. 実行時エラー

**答え**：A. 常に一致する

**解説**：

`Math.exp(x)` は `Math.pow(e, x)` と等価です。

ただし `exp(x)` の方が **やや精度や速度が高い**可能性があります。

---

### ❓ 問題5：自然対数との関係

```java
double x = 5.0;
System.out.println(Math.exp(Math.log(x)));
```

A. `1.0`

B. `x` のまま（5.0）

C. `NaN`

D. コンパイルエラー

**答え**：B. `5.0`

**解説**：

`log()` と `exp()` は**逆関数の関係**。

> exp(log(x)) = x（誤差がなければ）
> 
> 
> → `5.0` がそのまま戻ります。
> 

---

## ✅ よくある誤解ポイントまとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `Math.exp(x)` は `x の累乗`？ | ❌ `e` の `x` 乗を返す（底は固定） |
| `exp(-x)` は負の数になる？ | ❌ 正の逆数になる（例：`exp(-1)` = `1 / e`） |
| `exp(x)` の戻り値は整数？ | ❌ 常に `double`（誤差含む浮動小数点） |
| `exp(log(x))` は x と一致しない？ | ❌ 原則一致（誤差なければ） |