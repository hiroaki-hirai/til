# toDegrees()

`Math.toDegrees()` は Java において、**ラジアン（radian）を度数法の角度（degree）に変換するメソッド**です。

三角関数（`Math.sin()`, `Math.cos()` など）はラジアンを使うため、**人間が理解しやすい「度」に変換したいとき**に便利です。

---

## ✅ `Math.toDegrees()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `toDegrees()` |
| 用途 | **ラジアン → 度（degree）に変換する** |
| 戻り値の型 | `double` |

---

## 🔹 シグネチャ（定義）

```java
public static double toDegrees(double angrad)
```

- `angrad`: ラジアン値（弧度法）
- 戻り値：度（°）で表した角度（double 型）

---

## 🔹 使用例

```java
System.out.println(Math.toDegrees(0));             // → 0.0
System.out.println(Math.toDegrees(Math.PI));       // → 180.0
System.out.println(Math.toDegrees(Math.PI / 2));   // → 90.0
System.out.println(Math.toDegrees(2 * Math.PI));   // → 360.0
```

---

## 🔹 ラジアン → 度 の変換式

内部的には、次のように計算されます：

```java
degree = radian × (180 / π)
```

例：

- `Math.PI ≒ 3.14159` → `3.14159 × 180 / π = 180`

---

## 🔁 対になるメソッド：`Math.toRadians()`

| メソッド | 変換内容 | 主な用途 |
| --- | --- | --- |
| `Math.toRadians(x)` | 度 → ラジアン | 三角関数に渡す角度を変換 |
| `Math.toDegrees(x)` | ラジアン → 度 | 計算結果を人間にわかりやすく表示 |

---

## 🔹 三角関数と組み合わせた例

```java
double rad = Math.atan2(1, 1); // 結果 ≒ π/4
double deg = Math.toDegrees(rad);
System.out.println(deg); // → 約 45.0（度に変換）
```

---

## 🔸 特殊な値の扱い

| 入力 | 出力 |
| --- | --- |
| `0.0` | `0.0` |
| `Math.PI` | `180.0` |
| `Double.NaN` | `NaN` |
| `Double.POSITIVE_INFINITY` | `Infinity` |
| `-Math.PI / 2` | `-90.0` |

---

## ✅ よくある誤解と注意点

| 誤解内容 | 正しい理解 |
| --- | --- |
| `toDegrees()` は角度をラジアンに変換？ | ❌ 度に変換する（逆変換は `toRadians()`） |
| `toDegrees(Math.PI)` は 3.14？ | ❌ 180.0（度に変換されている） |
| `Math.sin(90)` を度数で扱いたい？ | ❌ `Math.sin(Math.toRadians(90))` を使う必要あり |
| 入力値に制限がある？ | ❌ どんな `double` 値も処理可能（範囲制限なし） |

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 主な用途 | 三角関数の結果（ラジアン）を人間に読みやすい度に変換 |
| 対になるメソッド | `Math.toRadians()` |
| 戻り値の型 | 常に `double` |
| 実務での使用シーン | グラフィックス、ゲーム、ロボティクス、三角関数の結果表示 |

---

## 🔺 `Math.toDegrees()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：`Math.toDegrees(Math.PI)` の結果は？

```java
System.out.println(Math.toDegrees(Math.PI));
```

A. `3.14159...`

B. `180.0`

C. `90.0`

D. コンパイルエラー

**答え**：B. `180.0`

**解説**：

`πラジアン = 180度`。

`toDegrees()` は **ラジアンを度に変換**するメソッドなので、**3.14159… → 180.0**。

---

### ❓ 問題 2：`Math.toDegrees()` の戻り値型

```java
var deg = Math.toDegrees(Math.PI / 2);
System.out.println(((Object)deg).getClass().getSimpleName());
```

A. `Float`

B. `Double`

C. `Integer`

D. `Long`

**答え**：B. `Double`

**解説**：

戻り値は常に **`double`** 型です。

引数が `int` や `float` であっても、戻りは `double` に昇格します。

---

### ❓ 問題 3：`Math.sin()` との併用における誤用

```java
System.out.println(Math.sin(Math.toDegrees(Math.PI / 2)));
```

A. `1.0`

B. `0.0`

C. `例外が発生する`

D. `NaN`

**答え**：B. `0.0`

**解説**：

これは **誤用例**。

`toDegrees()` はラジアン → 度変換ですが、`Math.sin()` は**ラジアン**で計算されます。

結果：`Math.sin(90.0)`（90ラジアン）は ≠ `sin(90度)` → **期待通りに 1.0 にならない！**

✅ 正しいのは：

```java
Math.sin(Math.toRadians(90)) // → 1.0
```

---

### ❓ 問題 4：負のラジアンの変換

```java
System.out.println(Math.toDegrees(-Math.PI / 2));
```

A. `-90.0`

B. `90.0`

C. `NaN`

D. コンパイルエラー

**答え**：A. `-90.0`

**解説**：

- `π/2` ラジアン = `90度`。

→ **正負は変換後も保持される**。符号に注意。

---

### ❓ 問題 5：特殊値 NaN の扱い

```java
System.out.println(Math.toDegrees(Double.NaN));
```

A. `0.0`

B. `NaN`

C. `Infinity`

D. コンパイルエラー

**答え**：B. `NaN`

**解説**：

Javaの `Math` クラスは、**NaNを引数にするとNaNを返す**のが基本ルール。

→ `Math.toDegrees()` も例外ではありません。

---

## ✅ よくある誤解まとめ

| 誤解内容 | 実際の仕様 |
| --- | --- |
| `Math.toDegrees()` は度 → ラジアン？ | ❌ ラジアン → 度 に変換（逆は `toRadians()`） |
| `toDegrees()` の戻り値は整数？ | ❌ 常に `double` 型 |
| `Math.sin(toDegrees(x))` で正確な値？ | ❌ `sin()` に渡すにはラジアンが必要（逆の変換が必要） |
| 負のラジアンは扱えない？ | ❌ 負のラジアンも正常に変換される（符号も維持） |