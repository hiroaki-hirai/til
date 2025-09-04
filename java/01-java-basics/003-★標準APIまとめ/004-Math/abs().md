# abs()

## 🔹 基本説明

**絶対値**とは、数直線上で0からの距離を表す値です。

たとえば `-10` の絶対値は `10`、`10` の絶対値は `10` です。

---

## 🔹 シグネチャ（メソッド定義）

```java
public static int abs(int a)
public static long abs(long a)
public static float abs(float a)
public static double abs(double a)
```

※ オーバーロードされていて、**引数の型に応じて適切な戻り値の型**が選ばれます。

---

## 🔹 使用例

```java
System.out.println(Math.abs(-5));       // 出力: 5（int）
System.out.println(Math.abs(-123.45));  // 出力: 123.45（double）
System.out.println(Math.abs(10));       // 出力: 10（正の数もそのまま）
```

## 🔹 注意点

1. **オーバーフロー**に注意（intの場合）
    
    `Math.abs(Integer.MIN_VALUE)` の場合、戻り値も `Integer.MIN_VALUE` になります（-2147483648）。
    
    理由：`int` の正の最大値は 2147483647 なので、+2147483648 にできないためです。
    

```java
System.out.println(Math.abs(Integer.MIN_VALUE)); // -2147483648
```

1. **NaNやInfinity**（float/double）
    - `Math.abs(Double.NaN)` → `NaN`
    - `Math.abs(Double.POSITIVE_INFINITY)` → `Infinity`

---

## 🔹 まとめ表

| 引数の型 | 戻り値の型 | 特徴 |
| --- | --- | --- |
| int | int | Integer.MIN_VALUEで注意 |
| long | long | Long.MIN_VALUEで同様の注意点あり |
| float | float | NaNやInfinityも対応 |
| double | double | 精度の高い計算が可能 |

## 🟢【パターン①：基本問題】

### 問題1

次のコードの出力結果は？

```java
int a = -10;
System.out.println(Math.abs(a));
```

**答え**：`10`

**解説**：負の整数 -10 の絶対値は 10 です。

---

### 問題2

次のコードの出力結果は？

```java
double d = -3.14;
System.out.println(Math.abs(d));
```

**答え**：`3.14`

**解説**：`double` 型の負数でも同様に絶対値が返されます。

---

## 🟡【パターン②：応用問題】

### 問題3

次のコードの出力結果は？

```java
int x = 5;
int y = 15;
int diff = Math.abs(x - y);
System.out.println(diff);
```

**答え**：`10`

**解説**：`5 - 15 = -10`、絶対値をとることで `10` になります。

→ 数値の差を正として扱いたいときに便利な使い方です。

---

### 問題4

次のコードの出力結果は？

```java
float f1 = -0.0f;
System.out.println(Math.abs(f1));
```

**答え**：`0.0`

**解説**：`-0.0f` と `0.0f` は数値的には等しいと見なされ、`abs` を適用しても `0.0` になります。

---

## 🔴【パターン③：注意点問題（オーバーフローなど）】

### 問題5

次のコードの出力結果は？

```java
System.out.println(Math.abs(Integer.MIN_VALUE));
```

**答え**：`-2147483648`

**解説**：`int` の最小値 `-2147483648` の絶対値は理論的に `2147483648` ですが、それは `int` の範囲を超えるため、オーバーフローが起きて **元の値が返される**仕様です。

---

### 問題6

次のコードの出力結果は？

```java
double nan = Double.NaN;
System.out.println(Math.abs(nan));
```

**答え**：`NaN`

**解説**：`NaN`（Not a Number）に対して `abs()` を使っても `NaN` のままです。

---

## 🔺【引っかけ問題①：オーバーフローの罠】

### 問題1

```java
int min = Integer.MIN_VALUE;
System.out.println(Math.abs(min) == -min);
```

このコードの出力結果はどうなる？

A. `true`

B. `false`

C. コンパイルエラー

D. 例外が発生する

**答え**：B. `false`

**解説**：

`Integer.MIN_VALUE = -2147483648`

この値の絶対値は `2147483648` ですが、これは `int` の正の最大値 `2147483647` を超えるため **オーバーフロー** が発生し、結果は元の `-2147483648` になります。

つまり `Math.abs(min) == -min` は **`false`** になります。

---

## 🔺【引っかけ問題②：浮動小数点の負のゼロ】

### 問題2

```java
float f = -0.0f;
System.out.println(Math.abs(f) == 0.0f);
```

このコードの出力は？

A. `true`

B. `false`

C. コンパイルエラー

D. 例外が発生する

**答え**：A. `true`

**解説**：

Java では `-0.0f == 0.0f` は **`true`** と評価されます。

`Math.abs(-0.0f)` は `0.0f` になるので、比較結果は `true` です。

→ `-0.0` は表示上だけの違いで、数値としては同じ扱いです。

---

## 🔺【引っかけ問題③：演算順序とint除算】

### 問題3

```java
System.out.println(Math.abs(5 / -2));
```

このコードの出力は？

A. `2.5`

B. `2`

C. `-2`

D. `2.0`

**答え**：B. `2`

**解説**：

`5 / -2` は `int / int` のため、**整数除算**が行われ `-2` になります。

その後 `Math.abs(-2)` により `2` が出力されます。

→ **浮動小数点にしたい場合は `5.0 / -2` のように明示する必要あり**。

---

## 🔺【引っかけ問題④：文字列との混同】

### 問題4

```java
System.out.println(Math.abs("−100"));
```

このコードの出力は？

A. `100`

B. `-100`

C. コンパイルエラー

D. 実行時エラー（NumberFormatException）

**答え**：C. コンパイルエラー

**解説**：

`Math.abs()` は `int`・`long`・`float`・`double` の **数値型の引数のみ**を受け取ります。

文字列 `"−100"` をそのまま渡すと、**コンパイルエラー**になります。

---

## 🔺【引っかけ問題⑤：変数型の思い込み】

### 問題5

```java
var x = Math.abs(-10.5);
System.out.println(((Object)x).getClass().getSimpleName());
```

このコードの出力は？

A. `int`

B. `float`

C. `double`

D. `long`

**答え**：C. `Double`

**解説**：

- `10.5` はリテラルとして **`double`型** なので、`Math.abs(double a)` が呼び出されます。

その結果も `double` なので、`x` の型も `double`、つまりボクシングされた型は `Double`。

---

# Math.abs() 試験対策用チェックシート

---

## 🔹 基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `abs()` |
| 用途 | 与えられた数値の「絶対値（正の値）」を返す |
| 修飾子 | `public static`（インスタンス不要で呼び出せる） |

---

## 🔹 オーバーロード一覧

| シグネチャ | 説明 |
| --- | --- |
| `int abs(int a)` | `int` の絶対値 |
| `long abs(long a)` | `long` の絶対値 |
| `float abs(float a)` | `float` の絶対値 |
| `double abs(double a)` | `double` の絶対値 |

---

## ✅ チェック項目（覚えておくべきポイント）

- `Math.abs()` は **staticメソッド**
- 引数の**型によって戻り値も変わる**
- `Integer.MIN_VALUE` の扱いに注意（そのまま返る）
- `-0.0` や `NaN` の取り扱いも問われる可能性あり
- `String` や非数値型には使用不可（コンパイルエラー）