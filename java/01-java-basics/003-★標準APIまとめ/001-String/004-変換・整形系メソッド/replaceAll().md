# replaceAll()

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `replaceAll(String regex, String replacement)` |
| 主な用途 | **正規表現にマッチする部分を置き換える** |

---

# 🔹 メソッドシグネチャ（定義）

```java
public String replaceAll(String regex, String replacement)
```

- **引数**：
    - `regex`：置き換え対象を指定する**正規表現パターン**（String型）
    - `replacement`：置き換える内容（String型）
- **戻り値**：
    - 置き換えた後の**新しいString**を返す（元のStringは不変）

---

# 🔹 簡単な使用例

```java
String s = "a-b-c";
String replaced = s.replaceAll("-", "_");

System.out.println(replaced);  // 出力 → "a_b_c"
```

✅ ハイフンをすべてアンダースコアに置き換え！

---

# 🔹 重要なポイントまとめ

| ポイント | 説明 |
| --- | --- |
| 正規表現が使える | ✅ 通常のreplace()と違い、**regexパターン**で指定できる |
| 元の文字列は変更される？ | ❌ されない（Stringはimmutableなので新しい文字列を返す） |
| 部分一致全体に適用される？ | ✅ 対象全てにマッチして置き換える |

---

# 🔹 よくある間違い・注意点

| 注意内容 | 理由 |
| --- | --- |
| 正規表現の特殊文字を使う場合 | `.`（任意文字）、`*`（繰り返し）など正規表現の意味になるので注意！ |
| シンプルな文字列置換だけしたい場合 | 正規表現不要なら`replace(CharSequence target, CharSequence replacement)`を使うほうが安全！ |

---

# ✅ 超シンプルまとめ

> replaceAll()は Stringクラスのメソッド！
正規表現ベースで置換する！
新しいStringを返す！（元は不変）
> 

✅ この感覚を押さえておけば、試験でも実務でもバッチリ対応できます！

以下に、**「replaceAll()専用 引っかけ問題集（正規表現ミス防止対策付き全8問）」** をご用意しました！

特に、

- **正規表現の扱い（`.`やなど）**
- **エスケープ忘れミス**
- **replace()との違い**
    
    を引っかけるポイントにしてあります！
    

---

# 🔺 `replaceAll()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：基本的な置換

```java
String s = "apple-banana-cherry";
System.out.println(s.replaceAll("-", ","));
```

### ✅ 問題 1の答え

**答え**：A. `"apple,banana,cherry"`

**解説**：

- `"-"`はそのままマッチして、すべてカンマに置換される。

---

### ❓ 問題 2：正規表現`.`（ドット）の意味

```java
String s = "abc.def";
System.out.println(s.replaceAll(".", "_"));
```

### ✅ 問題 2の答え

**答え**：C. `"______"`

**解説**：

- `.`は「任意の1文字」にマッチするので、全文字が置き換えられる！

---

### ❓ 問題 3：正しくピリオドを置き換えたい場合

次のうち「ピリオド `.` を1文字だけ置き換える」正しいコードは

### ✅ 問題 3の答え

**答え**：B.

```java
s.replaceAll("\\.", "_")
```

**解説**：

- `"."`は正規表現で特別な意味（任意文字）なので、ピリオドそのものを指定するには**エスケープが必要**！

---

### ❓ 問題 4：アスタリスクの正規表現の意味

```java
String s = "abc*def";
System.out.println(s.replaceAll("*", "_"));

```

### ✅ 問題 4の答え

**答え**：B. 実行時例外（PatternSyntaxException）

**解説**：

- `"*"`は正規表現では「直前の文字の0回以上繰り返し」という意味で、単体では使えない！

---

### ❓ 問題 5：空白文字（スペース）を除去する場合

```java
String s = "hello world java";
System.out.println(s.replaceAll(" ", ""));
```

### ✅ 問題 5の答え

**答え**：A. `"helloworldjava"`

**解説**：

- `" "`（半角スペース）を空文字`""`に置換する。

---

### ❓ 問題 6：タブ文字 `\t` を除去したい場合

正しいコードは？

### ✅ 問題 6の答え

**答え**：B.

```java
s.replaceAll("\\t", "")
```

**解説**：

- `\t`はエスケープが2段階必要（Java文字列中で `\\t` と書く）！

---

### ❓ 問題 7：replace()とreplaceAll()の違い

次のコードで違いが出る理由は？

```java
String s = "a.b.c";

System.out.println(s.replace(".", "_"));
System.out.println(s.replaceAll(".", "_"));
```

### ✅ 問題 7の答え

**答え**：

- `replace()`は**単純な文字列置換**（正規表現を使わない）
- `replaceAll()`は**正規表現ベース置換**なので`.`が「任意文字」扱いされるため、全文字が置き換わる！

✅ **replace()は安全、replaceAll()は正規表現解釈に注意！**

---

### ❓ 問題 8：複数スペースを1個にする場合

```java
String s = "hello    world     java";
System.out.println(s.replaceAll(" +", " "));
```

### ✅ 問題 8の答え

**答え**：A. `"hello world java"`

**解説**：

- `" +"` は「1個以上の空白文字」という意味なので、連続したスペースをまとめて1個のスペースに置換する！

---

# ✅ 難易度アップまとめ

| 罠ポイント | 正しい対策 |
| --- | --- |
| `.`（ドット）は任意1文字になる | `"\\."`でエスケープする |
| `*`（アスタリスク）は単体で使えない | 文法エラー（PatternSyntaxException発生） |
| タブ文字`\t`を指定するとき | `"\\t"`と2重エスケープする |
| replace()とreplaceAll()の使い分け | replace()は単純置換、replaceAll()は正規表現置換 |

---

# ✨ 超シンプルまとめ

> replaceAll()は正規表現ベース！
特殊記号はエスケープ必須！
単純置換だけならreplace()を使うのも手！
> 

✅ ここまで押さえておけば、試験でも実務でも安心して使えます！

以下に、**「replaceAll()＋正規表現活用 実務応用パターン集（電話番号・メール・住所整形版）」** をご用意しました！

すべて**実務現場で本当にあり得るケース**を想定しており、

**正規表現の組み立て＋replaceAll()の使い方**を一緒にトレーニングできる内容です！

---

# 🔺 `replaceAll()`＋正規表現活用 実務応用パターン集

---

## 📞 電話番号整形パターン

---

### ❓ ① ハイフンあり → ハイフンなしに整形する

```java
String phone = "090-1234-5678";
String cleaned = phone.replaceAll("-", "");

System.out.println(cleaned); // 出力 → "09012345678"

```

✅ **ハイフンだけを削除するシンプルなパターン**！

---

### ❓ ② 電話番号の間違った区切り記号（スペース）を正しいハイフンに変換する

```java
String phone = "090 1234 5678";
String corrected = phone.replaceAll(" ", "-");

System.out.println(corrected); // 出力 → "090-1234-5678"
```

✅ **スペースをハイフンに置き換え！**

---

### ❓ ③ 電話番号の「数字以外」を全部削除して数値だけにする

```java
String phone = "(090) 1234-5678";
String digits = phone.replaceAll("[^0-9]", "");

System.out.println(digits); // 出力 → "09012345678"
```

✅ **[^0-9]**は「数字以外」を意味！

---

## 📧 メールアドレス整形パターン

---

### ❓ ④ 全角英数字を半角に（仮定：事前に別処理でやっているとする）

→ 不要なスペースだけ除去する

```java
String mail = " test@example.com ";
String cleaned = mail.trim();

System.out.println(cleaned); // 出力 → "test@example.com"
```

✅ **trim()だけでOK！（replaceAll不要パターン）**

---

### ❓ ⑤ メールアドレスに紛れた全角スペース・タブを一括除去

```java
String mail = "test@ example .com";
String corrected = mail.replaceAll("\\s+", "");

System.out.println(corrected); // 出力 → "test@example.com"

```

✅ **\s+は空白・タブ・改行すべて含むホワイトスペース**！

---

## 🏠 住所整形パターン

---

### ❓ ⑥ 住所の連続スペースを1つにまとめる

```java
String address = "東京都   千代田区  一ツ橋 ";
String normalized = address.trim().replaceAll(" +", " ");

System.out.println(normalized); // 出力 → "東京都 千代田区 一ツ橋"
```

✅ **連続空白を1個にまとめる＋trimで前後空白除去**！

---

### ❓ ⑦ 「都道府県名」の末尾の全角スペースを除去する

```java
String address = "東京都　千代田区一ツ橋";  // 「東京都」の後ろに全角スペースが混じってる

String cleaned = address.replaceAll("　", "");

System.out.println(cleaned); // 出力 → "東京都千代田区一ツ橋"
```

✅ **全角スペース（U+3000）は"　"なので、普通にreplaceAll("　", "")する！**

---

## 🔥 応用：さらに複雑なパターン

---

### ❓ ⑧ 郵便番号形式（NNN-NNNN）に強制整形

ユーザーが「1234567」と入力した場合に「123-4567」にする。

```java
String zip = "1234567";
String formatted = zip.replaceAll("(\\d{3})(\\d{4})", "$1-$2");

System.out.println(formatted); // 出力 → "123-4567"
```

✅ **正規表現のキャプチャグループ$1, $2を使ったパターン置換**！

---

# ✅ 総まとめ

| シチュエーション | replaceAll()使用パターン |
| --- | --- |
| 電話番号整形（ハイフン除去など） | `replaceAll("-", "")`, `replaceAll("[^0-9]", "")` |
| メールアドレス空白除去 | `replaceAll("\\s+", "")` |
| 住所整形（連続スペースまとめ） | `replaceAll(" +", " ")` |
| 郵便番号フォーマット強制（3-4桁） | `replaceAll("(\\d{3})(\\d{4})", "$1-$2")` |

---

# ✨ 超シンプルまとめ

> replaceAll()は単純置換だけでなく、正規表現を活かすと実務整形が超強力になる！
キャプチャグループ（$1, $2）や\s+（空白類）も積極活用しよう！
> 

✅ この感覚を押さえれば、試験も実務もバッチリです！

# Q,

String.replaceAllメソッドは、新たな変数を用意しそこえ置換後の文字列の参照を代入する目的で使用する。
元の文字列は置換されない（不変オブジェクトより）。
なお、replaceAllメソッドは、戻り値を返すのみなので副作用なし。
上記の認識は合っていますか？

はい、Hiroakiさんの認識は**非常に正確です**。

内容を確認・補足しながら整理します。

---

## ✅ `String.replaceAll()` に関するHiroakiさんの認識の検証

### ① **「新たな変数に置換後の参照を代入する目的で使用する」**

👉 **正しいです。**

- `String.replaceAll()` は **元の文字列を変更するのではなく、置換後の新しい文字列を返す**メソッドです。
- `String` は不変（immutable）なので、**変更＝新しいオブジェクトの生成**です。

例：

```java
String original = "abc123";
String modified = original.replaceAll("[0-9]", "X");

System.out.println(original); // abc123（変化なし）
System.out.println(modified); // abcXXX（新しいオブジェクト）
```

---

### ② **「元の文字列は置換されない（不変オブジェクトより）」**

👉 **完全に正しいです。**

- `String` クラスは `final` クラスであり、その内部も `final char[]` または `final byte[]`。
- したがって、**一度生成された文字列の内容は変更できません。**

---

### ③ **「replaceAllメソッドは戻り値を返すのみなので副作用なし」**

👉 **これも正しいです。**

- `replaceAll()` は **純粋関数的（pure function）な設計**で、
    
    入力に対して出力を返すだけで、**元の状態に影響（副作用）を与えません**。
    

---

## ✅ 補足：`replace()` との違い

| メソッド | 説明 | 引数 | 正規表現？ |
| --- | --- | --- | --- |
| `replace()` | 単純な文字列置換 | `char` や `CharSequence` | ❌ |
| `replaceAll()` | 正規表現に一致した部分を置換 | `regex`, `replacement` | ✅ |

```java
"abc123".replace("1", "X");        // → "abcX23"
"abc123".replaceAll("[0-9]", "X"); // → "abcXXX"
```

---

## ✅ 結論（確認）

| 項目 | 判定 | 備考 |
| --- | --- | --- |
| 新しい参照を代入するために使う | ✅ | 破壊的変更ではなく「変換」 |
| 元の文字列は変更されない | ✅ | `String` はイミュータブル |
| 副作用はない | ✅ | 戻り値を返すだけで状態を持たない |

---

ご指摘の通り、`replaceAll()` は「安全に、元の文字列を壊さず使う」ことができる**副作用のない変換手段**です。

正規表現のマッチやグループ置換についても必要があれば解説できますので、気軽にお知らせください！