# charAt()

`String.charAt()` は Java において、**指定した位置の1文字（char型）を取得するメソッド**です。

文字列の中から **「n番目の文字を取り出したい」** という時に使います。

ただし、**サロゲートペア文字（絵文字など）では注意が必要**です！

---

## ✅ `String.charAt()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `charAt(int index)` |
| 戻り値の型 | `char` |
| 引数 | 取り出したい位置（0から始まるインデックス） |
| 例外 | `StringIndexOutOfBoundsException`（範囲外アクセス時） |

---

## 🔹 シグネチャ（定義）

```java
public char charAt(int index)
```

- `index`：0始まりの文字位置（`0 <= index < s.length()`）
- 戻り値：その位置にある `char` 型の文字（16bit）

---

## 🔹 使用例（基本）

```java
String s = "Java";
System.out.println(s.charAt(0)); // → 'J'
System.out.println(s.charAt(2)); // → 'v'
```

✅ 配列っぽいイメージで「何番目か」の文字が取り出せます。

---

## 🔸 注意点①：**インデックスは0からスタート**

```java
String s = "Hello";
System.out.println(s.charAt(4)); // → 'o'
System.out.println(s.charAt(5)); // ❌ 例外発生（範囲外）
```

✅ 最大インデックスは `length() - 1`！

---

## 🔸 注意点②：**サロゲートペア（絵文字など）は分かれている**

たとえば：

```java
String emoji = "𩸽"; // U+29E3D（サロゲートペア）
System.out.println(emoji.length());    // → 2
System.out.println(emoji.charAt(0));   // → ? (上位サロゲート)
System.out.println(emoji.charAt(1));   // → ? (下位サロゲート)
```

✅ `charAt()` で取り出すと、サロゲートペアは**バラバラに読まれる**！

---

## 🔸 サロゲートペアを正しく扱いたい場合は？

👉 `String.codePointAt(index)` や

👉 `String.codePoints()` を使うのが安全です！

例：

```java
int codePoint = emoji.codePointAt(0);
System.out.println(Character.toChars(codePoint)); // 正しく1文字として表示
```

---

## ✅ よくある誤解と注意点

| 誤解内容 | 実際の仕様 |
| --- | --- |
| `charAt()` は「見た目の1文字」を取る？ | ❌ サロゲートペアの場合、片側だけ（16bit char単位） |
| 文字列外のインデックスも安全？ | ❌ 範囲外なら `StringIndexOutOfBoundsException` が発生 |
| `charAt()` は `String` を返す？ | ❌ `char` 型を返す（文字列ではない） |
| `charAt()` でUnicode全対応できる？ | ❌ codePoint系メソッドで扱うべき |

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| インデックス指定 | 0始まり。最大は `length() - 1` |
| 戻り値 | `char`（16bit、サロゲートペアは分離される） |
| 範囲外アクセス | `StringIndexOutOfBoundsException` |
| Unicode対応が必要な場合 | `codePointAt()` や `codePoints()` を使う |

---

## 🔺 `String.charAt()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：インデックス指定の範囲

```java
String s = "Java";
System.out.println(s.charAt(4));
```

このコードの結果は？

A. `'a'`

B. コンパイルエラー

C. 実行時例外

D. `'v'`

**答え**：C. 実行時例外

**解説**：

`charAt(4)` は範囲外アクセス（最大は `length()-1 = 3`）。

✅ 実行時に `StringIndexOutOfBoundsException` が発生します。

---

### ❓ 問題 2：戻り値の型

```java
String s = "Hello";
var c = s.charAt(1);
System.out.println(((Object)c).getClass().getSimpleName());
```

A. `Character`

B. `String`

C. `Char`

D. プリミティブ型

**答え**：D. プリミティブ型

**解説**：

`charAt()` の戻り値は **プリミティブ型の `char`**。

クラスではなく、基本型！

---

### ❓ 問題 3：サロゲートペア文字列

```java
String emoji = "𩸽"; // サロゲートペア（U+29E3D）
System.out.println(emoji.length());
System.out.println(emoji.charAt(0));
System.out.println(emoji.charAt(1));
```

この出力内容について正しい記述はどれ？

A. `length()` は 1、charAt(0)は'𩸽'

B. `length()` は 2、charAt(0)とcharAt(1)はそれぞれ別のchar

C. `length()` は 2、charAt(0)とcharAt(1)は同じ文字

D. コンパイルエラー

**答え**：B. `length()` は 2、charAt(0)とcharAt(1)はそれぞれ別のchar

**解説**：

- `𩸽` は UTF-16上で2charに分かれて格納
- `charAt(0)` は上位サロゲート、`charAt(1)` は下位サロゲート
- つまり1つの絵文字が2つのcharに分かれている！

---

### ❓ 問題 4：charAt()とStringの違い

```java
String s = "abc";
char c = s.charAt(1);
System.out.println(c + "d");
```

この出力は？

A. `bd`

B. `bd` （改行付き）

C. `98d`

D. コンパイルエラー

**答え**：C. `98d`

**解説**：

- `char` + `String` ではなく
- `char` + `String` の場合、`char` が **int（文字コード）に自動変換されて加算**されます！

→ 'b'（ASCII 98）+ "d" → 98 + "d" → `"98d"`

✅ 正しく文字として連結したいなら、こうする：

```java
System.out.println("" + c + "d"); // → "bd"
```

---

### ❓ 問題 5：範囲指定の注意

```java
String s = "sample";
System.out.println(s.charAt(-1));
```

A. `'s'`

B. `'e'`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

負のインデックス（-1）は許可されない！

→ 実行時に `StringIndexOutOfBoundsException` が発生。

---

## ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `charAt()` は String を返す？ | ❌ `char` 型（プリミティブ型）を返す |
| `charAt()` でサロゲートペアもまとめて取得？ | ❌ 上下サロゲートに分かれる（片側ずつ） |
| 範囲外アクセスでもnullが返る？ | ❌ 即 `StringIndexOutOfBoundsException` が発生 |
| `char` + `String` は文字連結？ | ❌ `char` が数値に変換されて加算される |