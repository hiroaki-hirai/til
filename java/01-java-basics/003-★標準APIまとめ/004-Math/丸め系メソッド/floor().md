# floor()

---

## ✅ `Math.floor()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `floor()` |
| 用途 | 数値を「小さい方向に切り捨てた値」に丸める |
| 修飾子 | `public static`（staticメソッド） |

---

## 🔹 シグネチャ（メソッド定義）

```java
public static double floor(double a)
```

---

## 🔹 動作の特徴（例付き）

| 式 | 結果 | 説明 |
| --- | --- | --- |
| `Math.floor(3.7)` | `3.0` | 小数点以下切り捨て |
| `Math.floor(3.0)` | `3.0` | 整数そのまま |
| `Math.floor(-3.7)` | `-4.0` | より小さい方向に丸める |
| `Math.floor(-3.0)` | `-3.0` | 整数そのまま |
| `Math.floor(-0.5)` | `-1.0` | -0.5 も -1.0 へ |

---

## 🔸 特徴まとめ

| 観点 | 内容 |
| --- | --- |
| 戻り値の型 | `double` |
| 負の値の挙動 | より小さい方向へ丸める（-3.7 → -4.0） |
| 精度 | 端数があっても正確な丸めになる |
| 特殊値 | `NaN` → `NaN`、`Infinity` → そのまま |

---

## 🔹 `Math.floor()` vs 他の丸めメソッドの違い

| メソッド | 動作 | 例（2.5） | 例（-2.5） |
| --- | --- | --- | --- |
| `Math.floor()` | 小さい方向へ切り捨て | `2.0` | `-3.0` |
| `Math.ceil()` | 大きい方向へ切り上げ | `3.0` | `-2.0` |
| `Math.round()` | 四捨五入（0.5以上で切り上げ） | `3` | `-2` |
| `Math.rint()` | 最近接の整数（同点は偶数） | `2.0` | `-2.0` |

---

## 🔹 使用例コード（簡単なサンプル）

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Math.floor(7.8));    // → 7.0
        System.out.println(Math.floor(-2.3));   // → -3.0
    }
}
```

---

## 🔹 注意点

| 観点 | 注意点 |
| --- | --- |
| 戻り値の型 | 常に `double` 型。たとえ整数に見えても `int` ではない |
| 正負の挙動 | **「0方向の切り捨て」ではない** → 常に「小さい方向」へ丸める |
| `cast` の併用 | `int i = (int) Math.floor(x);` のように明示的キャストが必要なこともあり |

---

## ✅ よくある誤解

| 誤解内容 | 実際は… |
| --- | --- |
| `Math.floor(-2.5)` は `-2.0` になる？ | → ❌ `-3.0` になる（小さい方向に切り捨て） |
| `Math.floor()` の戻り値は `int`？ | → ❌ `double`（明示キャストが必要） |

---

## 🔺 Math.floor() の引っかけ問題集（全5問）

---

### ❓ 問題1：戻り値の型

```java
var x = Math.floor(3.7);
System.out.println(((Object)x).getClass().getSimpleName());
```

A. `Double`

B. `Integer`

C. `Float`

D. `Long`

**答え**：A. `Double`

**解説**：

`Math.floor(double a)` は `double` を返すので、`x` の型も `double`。

→ ボクシングされると `Double` になります。

---

### ❓ 問題2：負の小数の切り捨て

```java
System.out.println(Math.floor(-2.3));
```

A. `-2.0`

B. `-3.0`

C. `-2`

D. コンパイルエラー

**答え**：B. `-3.0`

**解説**：

`Math.floor()` は常に「**小さい方向**」に丸めます。

- `2.3` は `3.0` に丸められます（ゼロ方向じゃない！）

---

### ❓ 問題3：cast の落とし穴

```java
int x = Math.floor(4.9);
System.out.println(x);
```

A. `4`

B. `5`

C. コンパイルエラー

D. 実行時エラー

**答え**：C. コンパイルエラー

**解説**：

`Math.floor()` の戻り値は `double` → `int` に代入するには **明示的キャストが必要**。

```java
int x = (int) Math.floor(4.9); // これならOK
```

---

### ❓ 問題4：整数でも double

```java
System.out.println(Math.floor(5.0));
```

A. `5`

B. `5.0`

C. `5.00`

D. コンパイルエラー

**答え**：B. `5.0`

**解説**：

戻り値は `double`。たとえ整数でも型は `int` ではなく `double`。

→ `System.out.println()` では `5.0` と表示されます。

---

### ❓ 問題5：floor と round の違い

```java
System.out.println(Math.floor(-2.5));
System.out.println(Math.round(-2.5));
```

正しい出力は？

A. `-2.0` と `-2`

B. `-3.0` と `-2`

C. `-3.0` と `-3`

D. `-2.0` と `-3`

**答え**：B. `-3.0` と `-2`

**解説**：

- `Math.floor(-2.5)` → 小さい方向に切り捨て → `3.0`
- `Math.round(-2.5)` → 絶対値で四捨五入 → `2`

---

## ✅ よくある誤解ポイントまとめ

| 誤解 | 正しい理解 |
| --- | --- |
| `Math.floor()` の戻り値は int？ | → ❌ 常に `double` |
| 負の切り捨ては 0 に近づく？ | → ❌ 小さい方向（-2.3 → -3.0） |
| `floor` と `round` は同じ動作？ | → ❌ `floor` は切り捨て、`round` は四捨五入 |