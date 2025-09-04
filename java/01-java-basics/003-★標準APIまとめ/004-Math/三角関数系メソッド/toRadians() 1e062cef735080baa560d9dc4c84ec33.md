# toRadians()

`Math.toRadians()` は Java において、**角度（度）をラジアン（弧度法）に変換するメソッド**です。

三角関数（sin, cos, tan など）は **ラジアン単位**で計算するため、

通常の「度数法」（90度、180度など）を使いたい場合には**toRadians()で事前変換**する必要があります。

---

## ✅ `Math.toRadians()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `toRadians()` |
| 用途 | **度数法の角度をラジアン（弧度法）に変換する** |
| 戻り値の型 | `double` |

---

## 🔹 シグネチャ（定義）

```java
public static double toRadians(double angdeg)
```

- `angdeg`：角度（degree、度数法単位）
- 戻り値：ラジアン（radian、弧度法単位）

---

## 🔹 使用例

```java
System.out.println(Math.toRadians(0));     // → 0.0
System.out.println(Math.toRadians(90));    // → 約1.5708（π/2）
System.out.println(Math.toRadians(180));   // → 約3.1415（π）
System.out.println(Math.toRadians(360));   // → 約6.2831（2π）
```

---

## 🔹 角度からラジアンへの変換式

`Math.toRadians(angdeg)` は、内部的に次の式で計算されます：

```java
radian = angdeg × (π / 180)
```

- 1周360度は、ラジアンでは2π（約6.2831）に相当します。
- 90度は π/2 ≈ 1.5708 ラジアン。

---

## 🔸 関連メソッド

| メソッド | 変換内容 |
| --- | --- |
| `Math.toRadians(x)` | **度 → ラジアン** |
| `Math.toDegrees(x)` | **ラジアン → 度** |

---

## 🔹 使用例（sin, cosとの併用）

三角関数は **ラジアンで計算する**ため、角度で入力する場合は必ず変換が必要です。

```java
double angle = 90;
double radians = Math.toRadians(angle);
double sinValue = Math.sin(radians);

System.out.println(sinValue); // → 1.0（sin(90°) = 1）
```

✅ 直接 `Math.sin(90)` と書くと**意図しない結果**になるので注意！

---

## ✅ よくある誤解と注意点

| 誤解内容 | 正しい理解 |
| --- | --- |
| `Math.sin(90)` は 1.0？ | ❌ 90ラジアンでsinを取るので、全然違う値になる |
| `toRadians()` の戻り値は整数？ | ❌ 常に `double` 型（小数点あり） |
| ラジアンは常に 0～2π？ | ❌ 負のラジアンもあり得る |
| `toRadians()` は度→度の変換？ | ❌ 度→ラジアン（単位変換） |

---

## ✅ 実務での用途

- 三角関数（sin, cos, tan）計算に使う（必須）
- ゲームプログラミング（座標回転）
- グラフィックス処理（角度による回転）
- ロボティクス・物理シミュレーション（角運動計算）

---

## 🔺 `Math.toRadians()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：`Math.toRadians(180)` の結果は？

```java
System.out.println(Math.toRadians(180));
```

A. `180.0`

B. `3.141592653589793`

C. `π`

D. `1.570796...`

**答え**：B. `3.141592653589793`

**解説**：

`180° = π ラジアン`。Javaでは実数（`double`）として返されます。

**AとCは紛らわしいけどNG**、Dはπ/2の値。

---

### ❓ 問題 2：`Math.sin(90)` の出力は？

```java
System.out.println(Math.sin(90));
```

A. `1.0`

B. `0.893996...`

C. `0.0`

D. コンパイルエラー

**答え**：B. `0.893996...`

**解説**：

`Math.sin()` は **ラジアン単位**で計算される。

`sin(90)` は「90ラジアンのsin」→ **想定と全く違う値**が出る。

✅ 正しくは：

```java
Math.sin(Math.toRadians(90)) // → 1.0
```

---

### ❓ 問題 3：戻り値の型

```java
var r = Math.toRadians(45);
System.out.println(((Object)r).getClass().getSimpleName());
```

A. `Float`

B. `Double`

C. `Integer`

D. `Long`

**答え**：B. `Double`

**解説**：

`Math.toRadians()` の戻り値は常に `double`。

整数を渡しても戻り値は浮動小数点型（例：0.785398...）

---

### ❓ 問題 4：toDegreesとの混同

```java
double angle = Math.toDegrees(Math.PI);
System.out.println(angle);
```

この結果に最も近いものは？

A. `180.0`

B. `3.14`

C. `90.0`

D. `1.57`

**答え**：A. `180.0`

**解説**：

これは **ラジアン→度数** の変換です。

πラジアン = 180度。`toDegrees()` と `toRadians()` を**混同しないこと！**

---

### ❓ 問題 5：範囲外の角度（360度以上）

```java
System.out.println(Math.toRadians(720));
```

A. `NaN`

B. `0.0`

C. `12.566370...`

D. `360.0`

**答え**：C. `12.566370...`

**解説**：

`720度 = 2周 = 4π ラジアン ≒ 12.566`

→ `toRadians()` に **範囲の制限はない**。大きな値もそのまま変換されます。

---

## ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `Math.toRadians(x)` は角度を返す？ | ❌ 度をラジアンに変換（単位変換） |
| `sin(90)` は 1.0？ | ❌ 90ラジアンのsin → 正しい答えは `sin(toRadians(90))` |
| `toRadians()` の戻りは整数？ | ❌ 常に `double` 型 |
| 360度以上を与えるとエラーになる？ | ❌ 範囲は制限なし。何度でも変換可能 |