# signum()

`Math.signum()` は Java において、**数値の符号（正・負・ゼロ）を判定するためのメソッド**です。

数値そのものを変えずに「正か負か（またはゼロか）」だけを知りたい場合に便利です。

---

## ✅ `Math.signum()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `signum()` |
| 用途 | **数値の符号を表す値（-1.0, 0.0, 1.0）を返す** |
| 戻り値の型 | `double` または `float`（オーバーロードあり） |

---

## 🔹 シグネチャ（定義）

```java
public static double signum(double a)
public static float signum(float a)
```

---

## 🔹 動作仕様（戻り値）

| 入力値 `a` | 戻り値 | 説明 |
| --- | --- | --- |
| `a > 0` | `1.0`（または `1.0f`） | 正の値 |
| `a == 0` | `0.0`（または `0.0f`） | ゼロ（符号なし） |
| `a < 0` | `-1.0`（または `-1.0f`） | 負の値 |
| `a == -0.0` | `-0.0`（※） | 符号付きゼロ（IEEE準拠） |
| `a == NaN` | `NaN` | 非数（Not a Number）はそのまま返る |

---

## 🔹 使用例

```java
System.out.println(Math.signum(10));      // → 1.0
System.out.println(Math.signum(-5));      // → -1.0
System.out.println(Math.signum(0));       // → 0.0
System.out.println(Math.signum(-0.0));    // → -0.0
System.out.println(Math.signum(Double.NaN)); // → NaN
```

---

## 🔸 `Math.signum()` の用途・活用シーン

| 用途 | 説明 |
| --- | --- |
| 値の正負だけを知りたい | `if文`や`switch`で `1`, `0`, `-1` によって処理分岐可能 |
| ベクトルの方向判定 | 正負方向の判断に便利（たとえば2点間の相対方向） |
| 符号付きの変化量制御 | 符号だけで方向を示したい場合に利用（物理計算、アニメーションなど） |

---

## ✅ よくある誤解と注意点

| 誤解内容 | 実際は… |
| --- | --- |
| `signum()` は数値を絶対値に変換する？ | ❌ それは `Math.abs()` の役割 |
| `signum(0)` は `-1.0`？ | ❌ `0.0`（ゼロは符号なし） |
| `signum()` の戻り値は `int`？ | ❌ `double` または `float`（明示的キャストが必要） |
| `signum(-0.0)` と `signum(0.0)` は同じ？ | ⛔ `-0.0` は `-0.0` を返すが、数値としてはほぼ等価 |

---

## 🔁 他の符号関連メソッドとの比較

| メソッド | 内容 |
| --- | --- |
| `Math.abs(x)` | 絶対値を返す（常に正の値） |
| `Math.signum(x)` | 符号を表す値を返す |
| `Integer.signum(x)` | int 専用の符号取得（戻り値は int 型） |

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 主な用途 | 数値の符号を調べる（正負0） |
| 戻り値の型 | `double` or `float`（数値のまま） |
| 入力が NaN の場合 | 戻り値も NaN |
| 使いやすさ | 分岐・方向判定に便利 |

---

## 🔺 `Math.signum()` 引っかけ問題集（全5問）

---

### ❓ 問題1：戻り値の型は？

```java
var result = Math.signum(-10);
System.out.println(((Object)result).getClass().getSimpleName());
```

**答え**：C. `Double`

**解説**：

`Math.signum(double a)` の戻り値は **`double`** 型。

※ `Math.signum(float a)` もあるが、引数が double の場合は戻りも double。

---

### ❓ 問題2：0と-0の違い

```java
System.out.println(Math.signum(0.0));
System.out.println(Math.signum(-0.0));
```

出力として正しい組み合わせは？

A. `0.0` と `0.0`

B. `0.0` と `-0.0`

C. `0.0` と `1.0`

D. コンパイルエラー

**答え**：B. `0.0` と `-0.0`

**解説**：

Javaでは IEEE 754 に準拠しており、**正のゼロと負のゼロを区別します**。

→ `signum(-0.0)` は `-0.0` を返す！

---

### ❓ 問題3：`Math.signum()` の用途

```java
System.out.println(Math.signum(-123.456));
```

A. `-123.456`

B. `123.456`

C. `-1.0`

D. `NaN`

**答え**：C. `-1.0`

**解説**：

`signum()` は符号（正負0）を返すだけ。

値そのものではなく、**方向を示す `-1.0` / `0.0` / `1.0`** が返ります。

---

### ❓ 問題4：NaNの扱い

```java
System.out.println(Math.signum(Double.NaN));
```

A. `0.0`

B. `NaN`

C. `1.0`

D. コンパイルエラー

**答え**：B. `NaN`

**解説**：

`signum(NaN)` の戻りも **`NaN`**。

`NaN` の計算結果はすべて `NaN` になるのが Java の仕様。

---

### ❓ 問題5：`Math.abs()` との違い

```java
System.out.println(Math.abs(-3.5));
System.out.println(Math.signum(-3.5));
```

出力の正しい組み合わせは？

A. `3.5` と `-3.5`

B. `-3.5` と `-1.0`

C. `3.5` と `-1.0`

D. `-3.5` と `1.0`

**答え**：C. `3.5` と `-1.0`

**解説**：

- `Math.abs(x)` → 絶対値（符号を取る）
- `Math.signum(x)` → 符号だけを返す（値は `1.0`, `1.0`, `0.0`）
    
    → 混同しやすいので注意！
    

---

## ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `signum()` は絶対値を返す？ | ❌ `Math.abs()` の役割です |
| `signum(0)` と `signum(-0.0)` は同じ？ | ❌ 値としては `0.0` と `-0.0`（IEEE準拠で異なる） |
| `signum()` の戻り値は `int`？ | ❌ `double` または `float` |
| `signum()` は NaN を 0 に変える？ | ❌ `NaN` のまま返す |