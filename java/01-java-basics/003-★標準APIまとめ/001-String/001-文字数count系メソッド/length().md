# length()

`String.length()` は Java において、**文字列（`String`）の長さ（文字数）を取得するためのメソッド**です。

Javaの文字列は**文字配列（char[]）として内部管理**されており、`length()` を使うことでその配列の要素数＝文字数を知ることができます。

---

## ✅ `String.length()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `length()` |
| 戻り値の型 | `int`（文字数） |
| 引数 | なし |
| 例外 | 発生しない（`null` の場合はNPEに注意） |

---

## 🔹 シグネチャ（定義）

```java
public int length()
```

- 戻り値：この `String` オブジェクトに含まれる**文字の数（`int`）**

---

## 🔹 使用例

```java
String s1 = "Java";
System.out.println(s1.length());  // → 4

String s2 = "";
System.out.println(s2.length());  // → 0

String s3 = "こんにちは";
System.out.println(s3.length());  // → 5（全て1文字）

String s4 = "a\nb\tc";
System.out.println(s4.length());  // → 5（制御文字も1文字としてカウント）
```

---

## 🔸 注意点①：**配列と違って `()` が必要！**

```java
String s = "Hello";
System.out.println(s.length);     // ❌ コンパイルエラー！
System.out.println(s.length());   // ✅ OK
```

✅ `配列.length` はフィールド、

✅ `String.length()` はメソッドです！

---

## 🔸 注意点②：**null参照は例外になる**

```java
String s = null;
System.out.println(s.length()); // ❌ NullPointerException 発生
```

✅ null チェックを忘れずに！

---

## 🔸 注意点③：**サロゲートペアは2文字カウント**

Javaでは、**基本多言語面（BMP）外のUnicode文字**（絵文字など）は **2文字扱い（サロゲートペア）**になります。

```java
String emoji = "𩸽"; // Unicode: U+29E3D
System.out.println(emoji.length()); // → 2
```

✅ **目に見える1文字でも `length()` は2と返す**ケースに注意！

---

## ✅ よくある誤解と注意点

| 誤解内容 | 実際の仕様 |
| --- | --- |
| `String.length` で呼び出せる？ | ❌ 配列と違い、`String.length()` はメソッド |
| 絵文字や特殊文字も1文字になる？ | ❌ サロゲートペア（例：絵文字）は2文字になることも |
| 空文字（""）の長さは `null`？ | ❌ `0`。ただし null とは異なる |
| `length()` は空白や改行を除く？ | ❌ 全ての文字（スペース、改行、タブなど）をカウント |

---

## ✅ まとめ

| 特徴 | 説明 |
| --- | --- |
| メソッド形式 | `string.length()` （配列ではなくメソッド！） |
| 戻り値の型 | `int`（文字数） |
| 空文字の長さ | `0` |
| Unicode絵文字など | サロゲートペアで `2` と数えることがある |
| null の場合 | `NullPointerException` に注意 |

---

## 🔺 `String.length()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：配列と文字列の length の違い

```java
String s = "Java";
int len = s.length;
System.out.println(len);
```

このコードの実行結果は？

A. `4`

B. コンパイルエラー

C. 実行時例外

D. `NullPointerException`

**答え**：B. コンパイルエラー

**解説**：

`String.length` は **フィールドではなくメソッド**。

正しくは `s.length()`。

→ 配列は `length`（フィールド）、文字列は `length()`（メソッド）！

---

### ❓ 問題 2：空文字の長さ

```java
String s = "";
System.out.println(s.length());
```

A. `-1`

B. `0`

C. `null`

D. コンパイルエラー

**答え**：B. `0`

**解説**：

空文字 `""` は**長さ0の有効な文字列**。

null とは異なる！

---

### ❓ 問題 3：null の場合

```java
String s = null;
System.out.println(s.length());
```

A. `0`

B. `null`

C. `例外が発生する`

D. `-1`

**答え**：C. 例外が発生する（`NullPointerException`）

**解説**：

null に対してメソッドを呼び出すと **`NullPointerException`** が発生します。

→ `null.length()` はコンパイルOKでも実行時に例外！

---

### ❓ 問題 4：サロゲートペア文字の長さ

```java
String emoji = "𩸽"; // U+29E3D（1文字に見える Unicode 文字）
System.out.println(emoji.length());
```

A. `1`

B. `2`

C. `3`

D. 実行時エラー

**答え**：B. `2`

**解説**：

Javaの `String.length()` は **`char` の個数**（UTF-16単位）を返す。

サロゲートペア（BMP外の文字）は2個の `char` で表現されるため、**長さは2** になります。

---

### ❓ 問題 5：改行やタブ文字を含む文字列

```java
String s = "a\nb\tc";
System.out.println(s.length());
```

A. `3`

B. `4`

C. `5`

D. `6`

**答え**：C. `5`

**解説**：

文字列 `"a\nb\tc"` の中身は次の5文字：

- `a`（1文字）
- `\n`（改行）→ 1文字
- `b`
- `\t`（タブ）→ 1文字
- `c`

✅ 改行やタブも **1文字としてカウントされる**！

---

## ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `String.length` で文字数を取得できる？ | ❌ `length()` メソッドで取得（配列と混同に注意） |
| `""` の長さは `null`？ | ❌ `0`。nullとは別物 |
| 絵文字・特殊文字は常に `1` 文字？ | ❌ サロゲートペアは `2`。実際の表示と `length()` は異なる |
| 改行・タブは長さに含まれない？ | ❌ 含まれる（`\n` や `\t` も1文字） |
| null に対して `length()` を呼ぶと `0`？ | ❌ `NullPointerException` 発生 |