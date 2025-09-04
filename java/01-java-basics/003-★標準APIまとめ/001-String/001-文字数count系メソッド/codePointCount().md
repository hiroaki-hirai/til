# codePointCount()

`String.codePointCount()` は Java において、**指定した範囲に含まれる「Unicodeコードポイント数（≒見た目の文字数）」を数えるためのメソッド**です。

特に、**絵文字や拡張漢字など、サロゲートペアを含む文字列に対して正しい文字数を数えたいときに必須**です！

---

## ✅ `String.codePointCount()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `codePointCount(int beginIndex, int endIndex)` |
| 戻り値の型 | `int` |
| 特徴 | **サロゲートペアを1文字としてカウント** |

---

## 🔹 シグネチャ（定義）

```java
public int codePointCount(int beginIndex, int endIndex)
```

- `beginIndex`：範囲の開始インデックス（0以上）
- `endIndex`：範囲の終了インデックス（この位置の手前までカウント）
- 戻り値：指定範囲内の**コードポイント（実質的な文字数）**

---

## 🔹 使用例（基本）

```java
String s = "Java";
System.out.println(s.codePointCount(0, s.length())); // → 4
```

✅ 通常のアルファベットなら、`length()` と同じ結果になります。

---

## 🔸 サロゲートペア対応例（重要）

```java
String emoji = "𩸽"; // 1つの絵文字、内部的には2つのcharで構成
System.out.println(emoji.length());                 // → 2
System.out.println(emoji.codePointCount(0, emoji.length())); // → 1
```

✅ `length()` は**char数（2）**を返す
✅ `codePointCount()` は**見た目の1文字数（1）**を返す！

---

## 🔸 注意点：範囲指定は `[beginIndex, endIndex)` 形式

- **beginIndex は含む**
- **endIndex は含まない**

例：

```java
String s = "ABCDE";
System.out.println(s.codePointCount(1, 4)); // "BCD" → 3
```

- **範囲指定は char単位**（サロゲートペアは2char必要）
- **カウントは code point単位**（サロゲートペアは1code point扱い）

```java
String s = "A𩸽B"; // 3文字に見えるが実際は char数4
System.out.println(s.codePointCount(0, 3));
```

→ 範囲は、0,1,2 の "A𩸽” となる

| インデックス（char単位） | 内容 |
| --- | --- |
| 0 | 'A' |
| 1 | サロゲートペアの上位サロゲート（𩸽の前半） |
| 2 | サロゲートペアの下位サロゲート（𩸽の後半） |
| 3 | 'B' |

---

## 🔹 エラーになるケース

| 例 | 内容 | 結果 |
| --- | --- | --- |
| `beginIndex < 0` | 開始位置が負数 | `StringIndexOutOfBoundsException` |
| `endIndex > length()` | 終了位置が文字列長を超える | 例外発生 |
| `beginIndex > endIndex` | 開始位置が終了位置より大きい | 例外発生 |

---

## ✅ よくある誤解と注意点

| 誤解内容 | 実際の仕様 |
| --- | --- |
| `length()` と `codePointCount()` は同じ？ | ❌ サロゲートペア（絵文字など）があると違いが出る |
| `endIndex` を含めてカウントする？ | ❌ endIndex は含まない（範囲は `[begin, end)`） |
| サロゲートペアは2文字？ | ✅ `length()`上は2だが、`codePointCount()`では1 |
| 範囲外指定しても大丈夫？ | ❌ 即 `StringIndexOutOfBoundsException` が発生 |

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 主な用途 | 「見た目の文字数」や「実際のコードポイント数」を数える |
| サロゲートペア対応 | ✅ 絵文字や拡張文字も正しく1文字とカウント |
| 戻り値型 | `int`型 |
| 範囲指定方法 | `[beginIndex, endIndex)`（endを含まない） |
| エラー条件 | インデックスが不正（負数、オーバー、逆順） |

---

## 🔺 `length()` vs `codePointCount()` 比較問題集（全5問）

---

### ❓ 問題 1：通常文字列の違い

```java
String s = "Java";
System.out.println(s.length());
System.out.println(s.codePointCount(0, s.length()));
```

この2つの出力は？

A. length → 4, codePointCount → 3

B. length → 4, codePointCount → 4

C. length → 3, codePointCount → 4

D. 実行時例外

**答え**：B. length → 4, codePointCount → 4

**解説**：

通常のアルファベットやひらがなは、**1文字＝1char**。

→ `length()` と `codePointCount()` の結果は一致します。

---

### ❓ 問題 2：サロゲートペア文字列

```java
String emoji = "𩸽"; // Unicode U+29E3D（絵文字）
System.out.println(emoji.length());
System.out.println(emoji.codePointCount(0, emoji.length()));
```

この出力は？

A. length → 1, codePointCount → 1

B. length → 2, codePointCount → 1

C. length → 1, codePointCount → 2

D. 実行時例外

**答え**：B. length → 2, codePointCount → 1

**解説**：

- `length()` は char単位 → サロゲートペアなので2
- `codePointCount()` は Unicode単位 → 見た目の1文字だけ

---

### ❓ 問題 3：部分範囲のカウント

```java
String s = "A𩸽B"; // 3文字に見えるが実際は char数4
System.out.println(s.codePointCount(0, 3));
```

このコードの結果は？

A. 2

B. 3

C. 4

D. 実行時例外

**答え**：A. 2

**解説**：

範囲 `[0, 3)` は char 配列の 0〜2番目を対象。

その中に

- A（1char）
- 𩸽（2char＝サロゲートペア）
が含まれており、合計2コードポイント。

---

### ❓ 問題 4：範囲指定ミス

```java
String s = "Hello";
System.out.println(s.codePointCount(3, 2));
```

この結果は？

A. 0

B. -1

C. 実行時例外

D. 例外は発生しない

**答え**：C. 実行時例外

**解説**：

`beginIndex > endIndex` の場合、

→ `StringIndexOutOfBoundsException` が発生！

---

### ❓ 問題 5：全体カウントの書き方

次のうち、「`String s` の**すべてのコードポイント数**を正しく数える方法」として正しいものはどれ？

A. `s.codePointCount(0, s.length())`

B. `s.codePointCount(0, s.length - 1)`

C. `s.codePointCount(0, s.length() - 1)`

D. `s.length()`

**答え**：A. `s.codePointCount(0, s.length())`

**解説**：

- `length()` は char数
- `codePointCount(0, s.length())` がコードポイント数全体を数える正しい方法

---

## ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `length()` と `codePointCount()` は常に同じ？ | ❌ サロゲートペアを含むと異なる |
| `endIndex` は含まれる？ | ❌ `[beginIndex, endIndex)`（endIndexは含まない） |
| 部分指定が逆でも問題ない？ | ❌ beginIndex > endIndex は例外になる |
| `length()` で実際の文字数が数えられる？ | ❌ サロゲートペアを考慮しないので不正確な場合あり |