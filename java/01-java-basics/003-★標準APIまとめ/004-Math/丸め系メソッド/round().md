# round()

---

## ✅ `Math.round()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.Math`（インポート不要） |
| 用途 | 小数点以下を四捨五入して整数にする |
| 修飾子 | `public static` |

---

## 🔹 メソッドシグネチャ（オーバーロード）

```java
public static int round(float a)   // float → int
public static long round(double a) // double → long
```

---

## 🔹 使用例と出力

```java
System.out.println(Math.round(10.4f));   // → 10
System.out.println(Math.round(10.5f));   // → 11
System.out.println(Math.round(-10.5f));  // → -10
System.out.println(Math.round(-10.6f));  // → -11
```

---

## 🔸 特徴と注意点

| 観点 | 内容 |
| --- | --- |
| 基準値 | **小数点以下が0.5以上 → 切り上げ、未満 → 切り捨て** |
| 負の値 | `-10.5` → `-10`（小数点以下0.5は「絶対値で切り上げ、符号を残す」） |
| float/doubleの違い | 戻り値が `float` なら `int` に、`double` なら `long` に変換される |
| 精度 | 計算誤差により `0.4999999` のような値は `0` になる（誤差考慮が必要） |

---

## 🔹 正負の値の丸め挙動（表）

| 値 | 結果 |
| --- | --- |
| `1.4` | `1` |
| `1.5` | `2` |
| `-1.4` | `-1` |
| `-1.5` | `-1` |
| `-1.6` | `-2` |

---

## 🔹 よくある誤解

### ❌ 誤解：「0.5は必ず切り捨て」

➡ **`Math.round(0.5)` は `1`**（切り上げになります）

---

## 🔹 その他の丸め系メソッドとの違い

| メソッド | 概要 | 例（1.5） | 例（-1.5） |
| --- | --- | --- | --- |
| `Math.round(x)` | 四捨五入 | 2 | -1 |
| `Math.floor(x)` | 小さい方向に切り下げ（常に下） | 1 | -2 |
| `Math.ceil(x)` | 大きい方向に切り上げ（常に上） | 2 | -1 |
| `Math.rint(x)` | 最近接の整数（同点は偶数へ） | 2.0 | -2.0 |

---

## ✅ まとめ

- `Math.round()` は「**四捨五入で整数に変換**」
- `float → int`、`double → long` に変換される
- 負の数の扱いも **「0.5切り上げ」** だが **符号は維持**
- `ceil()` や `floor()` との違いに注意

---

## 🔺 Math.round() の引っかけ問題集（全5問）

---

### 🧠 問題1：floatかdoubleか？

```java
var x = Math.round(2.5);
System.out.println(((Object)x).getClass().getSimpleName());
```

A. `Integer`

B. `Long`

C. `Double`

D. `Float`

**答え**：B. `Long`

**解説**：

`2.5` は `double` 型のリテラル。

→ `Math.round(double a)` が呼ばれて、戻り値は `long`。

`var x` は `long` になるので、ボクシングされると `Long` になります。

---

### 🧠 問題2：小数点以下ちょうど0.5

```java
System.out.println(Math.round(-1.5));
```

A. `-2`

B. `-1`

C. `0`

D. `例外が発生する`

**答え**：B. `-1`

**解説**：

Java の `round()` は「**絶対値で四捨五入 → 符号をそのまま**」という動作。

- `1.5` → `1.5` → `2` → `2` に見えるが、実際は `1.5` → `1`

（JLSの仕様により、0.5のときは絶対値で切り上げ＋元の符号を保持）

---

### 🧠 問題3：整数リテラルと混同

```java
System.out.println(Math.round(2));
```

A. `2`

B. `2.0`

C. `コンパイルエラー`

D. `例外が発生する`

**答え**：A. `2`

**解説**：

`2` は `int` 型 → `Math.round(int)` は **存在しない**

しかし、Javaは `int` を **`float` に自動昇格**させて `Math.round(float)` が呼ばれ、戻り値は `int`。

つまり結果は `2`（正しく処理される）

---

### 🧠 問題4：変数型と戻り値型

```java
float f = 3.6f;
long result = Math.round(f);
System.out.println(result);
```

このコードは？

A. `3` が出力される

B. `4` が出力される

C. コンパイルエラー

D. 実行時エラー

**答え**：C. コンパイルエラー

**解説**：

`Math.round(float)` の戻り値は `int`

→ `int` を `long` に **暗黙的に代入するのはOK** に思えるが、**戻り値が `int` 型なのに変数が `long`** という型の食い違いが明示されていない場合、**警告やミスを誘発**する。

→ 正しくは `long result = (long)Math.round(f);` のように明示が好ましい

> ✅ JVM的には通る場合もあるが、Silver試験では「型をきっちり使い分ける」ことが問われます。
> 

---

### 🧠 問題5：Math.round vs Math.rint

```java
System.out.println(Math.rint(2.5));
System.out.println(Math.round(2.5));
```

出力として正しい組み合わせは？

A. `2.0` と `2`

B. `2.0` と `3`

C. `3.0` と `2`

D. `3.0` と `3`

**答え**：B. `2.0` と `3`

**解説**：

- `Math.rint(2.5)` → **最近接の偶数に丸める** → `2.0`
- `Math.round(2.5)` → **四捨五入** → `3`

---

## ✅ よくある誤解を整理！

| 誤解ポイント | 正しい理解 |
| --- | --- |
| `round(-1.5)` は `-2` になる？ | → `-1` になる（符号保持） |
| `Math.round()` は常に切り上げ？ | → **四捨五入**（0.5 以上のみ切り上げ） |
| `round(2)` は使えない？ | → `int` → `float` に昇格して使える |
| `rint()` と `round()` の違いが不明確 | → `rint()` は偶数に丸める（2.5 → 2.0）、`round()` は整数へ四捨五入 |

---

## ✅ 丸め系メソッド比較

| メソッド | 動作 | 例（2.5） | 例（-2.5） |
| --- | --- | --- | --- |
| `Math.round(x)` | 四捨五入（0.5以上で切上） | 3 | -2 |
| `Math.floor(x)` | 小さい方向に切り下げ | 2 | -3 |
| `Math.ceil(x)` | 大きい方向に切り上げ | 3 | -2 |
| `Math.rint(x)` | 偶数への丸め（IEEE仕様） | 2.0 | -2.0 |