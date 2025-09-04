# min()

`Math.min()` は Java における **2つの値のうち小さい方を返すメソッド** です。

`Math.max()` の対になる存在で、**数値の最小値を簡潔に取得する**際に使われます。試験でも `max()` とセットでよく出題されます。

---

## ✅ `Math.min()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `min()` |
| 用途 | **2つの値のうち小さい方を返す** |
| 戻り値の型 | 引数と同じ（オーバーロードで型別に対応） |

---

## 🔹 シグネチャ（オーバーロード）

```java
public static int min(int a, int b)
public static long min(long a, long b)
public static float min(float a, float b)
public static double min(double a, double b)
```

---

## 🔸 NaNの扱い（浮動小数点）

| 式 | 結果 | 説明 |
| --- | --- | --- |
| `Math.min(Double.NaN, 100)` | `100.0` | NaN ではない方が優先される |
| `Math.min(NaN, NaN)` | `NaN` | 両方 NaN のときのみ NaN が返される |

---

## 🔸 型の一致に注意（混合不可）

```java
System.out.println(Math.min(5, 10L)); // ❌ コンパイルエラー
```

➡ int と long は型が違うため、どの min() を呼べばよいか不明。

✅ 対処方法：

```java
System.out.println(Math.min((long)5, 10L)); // OK
```

---

## 🔁 `Math.max()` との比較

| メソッド | 意味 | 戻り値の型 |
| --- | --- | --- |
| `Math.min(a,b)` | 小さい方の値を返す | `a` or `b` の型 |
| `Math.max(a,b)` | 大きい方の値を返す | 同上 |

---

## ✅ よくある誤解と注意点

| 誤解内容 | 正しい理解 |
| --- | --- |
| `min()` の戻り値は常に `int`？ | ❌ 入力の型に応じて `int`, `long`, `float`, `double` が返る |
| `NaN` を含むと `NaN` が返る？ | ❌ もう片方が有効ならそちらを返す |
| 異なる型の引数は自動で処理される？ | ❌ 明示的にキャストしないとコンパイルエラーになる |
| 複数値に対応している？ | ❌ 引数は2つのみ。3つ以上はネストで処理する必要あり |

---

## ✍️ 応用：3つ以上の最小値を求めたい場合

```java
int min = Math.min(a, Math.min(b, c));
```

または Java 8 以降なら Stream を使って：

```java
int min = Stream.of(a, b, c).min(Integer::compare).get();
```

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 比較対象 | 2つの値（同じ型） |
| 対応型 | int, long, float, double（オーバーロードで対応） |
| NaNの扱い | 両方が NaN の場合を除き、NaN ではない方を返す |
| 型混合はエラー | 異なる型を混ぜるとコンパイルエラーになる |

---

## 🔺 `Math.min()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：型の不一致

```java
System.out.println(Math.min(3, 3L));
```

A. `3`

B. `3.0`

C. コンパイルエラー

D. 実行時エラー

**答え**：C. コンパイルエラー

**解説**：

`int` と `long` の組み合わせでは、どちらのオーバーロードを呼ぶか特定できずエラーになります。

✅ 解決策：

```java
System.out.println(Math.min((long)3, 3L));
```

---

### ❓ 問題 2：NaN の扱い

```java
System.out.println(Math.min(Double.NaN, 10));
```

A. `NaN`

B. `10.0`

C. `0.0`

D. コンパイルエラー

**答え**：B. `10.0`

**解説**：

`Math.min()` は **NaN ではない方を優先して返す**。

→ 両方 NaN なら NaN、片方だけなら「正常な方」を返す。

---

### ❓ 問題 3：戻り値の型は？

```java
var result = Math.min(2.5f, 3.0f);
System.out.println(((Object)result).getClass().getSimpleName());
```

A. `Float`

B. `Double`

C. `Integer`

D. `Long`

**答え**：A. `Float`

**解説**：

引数が `float` 型なら、戻り値も **`float` 型** になります。

→ `Math.min()` は引数の型によってオーバーロードが選ばれ、**戻り値も一致**

---

### ❓ 問題 4：負の比較

```java
System.out.println(Math.min(-7, -3));
```

A. `-3`

B. `-7`

C. `0`

D. `4`

**答え**：B. `-7`

**解説**：

「より小さい方」を返すので、**よりマイナスの大きい値**が選ばれる。

→ `-7 < -3` → `-7` が最小。

---

### ❓ 問題 5：3つ以上の値の比較

```java
int a = 7, b = 2, c = 5;
int result = Math.min(a, b, c);
System.out.println(result);
```

A. `2`

B. `コンパイルエラー`

C. `5`

D. `Math.min` は3つまでOK

**答え**：B. コンパイルエラー

**解説**：

`Math.min()` は **2つの引数しか受け取れない**。

→ 3つ以上の比較をするにはネストする必要がある：

```java
int result = Math.min(a, Math.min(b, c));
```

---

## ✅ よくある誤解まとめ

| 誤解内容 | 実際の仕様 |
| --- | --- |
| 異なる型の組み合わせもOK？ | ❌ 明示的にキャストしないとコンパイルエラー |
| `Math.min(NaN, 10)` は NaN？ | ❌ 正常な値 `10.0` を返す |
| `min()` の戻り値は常に `int`？ | ❌ 引数の型に応じて `int`, `float`, `double` 等 |
| `Math.min(a, b, c)` は使える？ | ❌ 引数は2つまで。3つ以上はネストするかStream使用 |