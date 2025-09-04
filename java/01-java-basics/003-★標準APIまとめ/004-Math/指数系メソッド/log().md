# log()

`Math.log()` は Java の標準クラス `java.lang.Math` に含まれるメソッドで、**自然対数（ネイピア数 e を底とする対数）を求める**ために使います。

---

## ✅ `Math.log()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `log()` |
| 用途 | **自然対数（底 e ≒ 2.718）の対数を返す** |
| 戻り値の型 | `double` |
| 引数の型 | `double` |
| 定義 | `public static double log(double a)` |

---

## 🔹 そもそも「自然対数」とは？

- 「e」を底（基数）とする対数：Math.log(x)=loge(x)
    
    Math.log(x)=log⁡e(x)\text{Math.log}(x) = \log_e(x)
    
- 例：Math.log(e)≒1.0Math.log(1)=0.0
    
    Math.log(e)≒1.0\text{Math.log}(e) ≒ 1.0
    
    Math.log(1)=0.0\text{Math.log}(1) = 0.0
    

---

## 🔹 使用例

```java
System.out.println(Math.log(1));            // → 0.0
System.out.println(Math.log(Math.E));       // → 1.0
System.out.println(Math.log(10));           // → 約 2.302585092994046
System.out.println(Math.log(0.5));          // → 負の値（約 -0.693）
```

---

## 🔸 特殊値の扱い

| 入力値 | 結果 | 説明 |
| --- | --- | --- |
| `Math.log(1)` | `0.0` | e⁰ = 1 だから |
| `Math.log(Math.E)` | `1.0` | e¹ = e |
| `Math.log(0)` | `-Infinity` | 極限的に小さい値に近づく |
| `Math.log(-1)` | `NaN` | 定義できない（実数の範囲外） |
| `Math.log(Double.POSITIVE_INFINITY)` | `Infinity` | 無限大に発散 |
| `Math.log(Double.NaN)` | `NaN` | 計算不能 |

---

## 🔸 関連メソッドとの比較

| メソッド | 説明 | 例 |
| --- | --- | --- |
| `Math.log(x)` | **自然対数（底 e）** | `Math.log(Math.E)` → 1.0 |
| `Math.log10(x)` | **常用対数（底 10）** | `Math.log10(100)` → 2.0 |
| `Math.exp(x)` | **e の x乗**（指数関数） | `Math.exp(1)` → ≒ 2.718 |
| `Math.pow(e, x)` | `Math.exp(x)` と同じ（e^x） | `Math.pow(Math.E, 2)` → ≒ 7.389 |

---

## 🔁 `log()` と `exp()` は逆関数！

```java
double x = 5.0;
System.out.println(Math.exp(Math.log(x))); // → 5.0（理論上）
System.out.println(Math.log(Math.exp(x))); // → 5.0（理論上）
```

※ 浮動小数点の精度により多少の誤差が出る場合があります。

---

## ✅ よくある誤解と注意点

| 誤解 | 正しい理解 |
| --- | --- |
| `Math.log()` は log10？ | ❌ → `Math.log()` は自然対数（底 e）、logₑ(x) |
| `Math.log(0)` は 0？ | ❌ → `-Infinity`（0 に近づくと対数は無限に小さくなる） |
| `Math.log(-1)` は `-1`？ | ❌ → `NaN`（負の数の対数は実数範囲では未定義） |
| `Math.log(x)` は always ≥ 0？ | ❌ → 0 < x < 1 の場合はマイナスになる（例：log(0.5) ≒ -0.693） |

---

## ✅ 使用場面（実務やアルゴリズム）

- 機械学習（クロスエントロピー、損失関数）
- 金融計算（対数収益率）
- 数値スケーリング（データの正規化・変換）
- アルゴリズム（対数計算量の評価）

---

## 🔺 `Math.log()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：戻り値の型は？

```java
var result = Math.log(10);
System.out.println(((Object)result).getClass().getSimpleName());
```

A. `Float`

B. `Double`

C. `Integer`

D. `Long`

**答え**：B. `Double`

**解説**：

`Math.log()` の戻り値は **常に `double` 型** です。

対数計算は浮動小数点精度で行われるため、整数が返されるケースでも型は `Double` になります。

---

### ❓ 問題 2：`Math.log(0)` の結果は？

```java
System.out.println(Math.log(0));
```

A. `0.0`

B. `-Infinity`

C. `NaN`

D. コンパイルエラー

**答え**：B. `-Infinity`

**解説**：

数学的に logₑ(0) は **極限で -∞** に近づきます。

Java でも `-Infinity` を返します（例外は発生しません）。

---

### ❓ 問題 3：負の引数の扱い

```java
System.out.println(Math.log(-1));
```

A. `-1.0`

B. `NaN`

C. `0.0`

D. `例外が発生する`

**答え**：B. `NaN`

**解説**：

負の数の自然対数は実数としては未定義なので、Javaでは **`NaN`（Not a Number）** が返されます。

例外は発生しませんが、注意が必要です。

---

### ❓ 問題 4：`Math.log()` と `Math.log10()` の違い

```java
System.out.println(Math.log(100));
System.out.println(Math.log10(100));
```

正しい出力に近い組み合わせは？

A. `4.605...` と `2.0`

B. `2.0` と `4.605...`

C. 両方 `2.0`

D. コンパイルエラー

**答え**：A. `4.605...` と `2.0`

**解説**：

- `Math.log(100)` は **自然対数 logₑ(100)** → 約 4.605
- `Math.log10(100)` は **常用対数 log₁₀(100)** → `2.0`
    
    → 試験ではこの違いが非常に問われやすいです！
    

---

### ❓ 問題 5：逆関数としての理解

```java
double x = 3.0;
System.out.println(Math.exp(Math.log(x)));
```

出力は？

A. `3.0`

B. `log(x)`

C. `e^x`

D. `NaN`

**答え**：A. `3.0`

**解説**：

`Math.log(x)` は `e` を底とした対数 → その結果に `Math.exp()` をかけると元に戻る：

> exp(log(x)) = x（※理論上。実際は多少の誤差あり）
> 

---

## ✅ よくある誤解ポイントまとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `Math.log(x)` は `log₁₀(x)`？ | ❌ `Math.log()` は**自然対数（logₑ）** |
| `Math.log(0)` は `0.0`？ | ❌ `-Infinity` が返る |
| `Math.log(-1)` は `例外` になる？ | ❌ 例外は出ず、`NaN` が返る |
| `exp(log(x))` は `e^x`？ | ❌ `exp(log(x)) = x`（`exp` と `log` は逆関数） |