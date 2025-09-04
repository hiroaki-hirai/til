# strip()

---

# ✅ `String.strip()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| 実装されたバージョン | **Java 11** から新しく登場 |
| 戻り値の型 | `String`（前後の空白を取り除いた新しい文字列） |
| 主な用途 | **先頭と末尾の空白（Unicode空白含む）を除去する** |

---

# 🔹 シグネチャ（定義）

```java
public String strip()
```

- **引数なし**。
- 戻り値は、空白除去後の**新しいString**。

---

# 🔹 使用例（基本）

```java
String s = "　Hello World　"; // 全角スペース付き
String result = s.strip();

System.out.println(result); // → "Hello World"
```

✅ strip()は、**半角スペースも全角スペースも**しっかり除去できる！

---

# 🔹 trim()との違い

| 項目 | `trim()` | `strip()` |
| --- | --- | --- |
| 登場時期 | Java 1.0（最初期から存在） | Java 11（比較的新しい） |
| 除去できる空白の種類 | **ASCIIの空白（U+0020）だけ** | **すべてのUnicode空白文字を対象にする** |
| 全角スペース（U+3000）は？ | ❌ 削除できない | ✅ 削除できる |
| 使いどころ | 軽い処理、古いコードベース | より厳密な空白処理、国際化対応が必要な場面 |

---

### 🔸 例：trim()とstrip()の違いを体験

```java
String s = "　Hello　"; // 両端に全角スペース（U+3000）

System.out.println("[" + s.trim() + "]");
System.out.println("[" + s.strip() + "]");
```

出力：

```java
[　Hello　]  ← trim()では全角スペースが残る！
[Hello]      ← strip()では全角スペースも除去される！
```

✅ **strip()は国際化対応（i18n対応）向き！**

---

# 🔹 さらに派生メソッドもある

Java 11以降、strip()と一緒に次も追加されています！

| メソッド名 | 説明 |
| --- | --- |
| `stripLeading()` | 先頭の空白だけ除去する |
| `stripTrailing()` | 末尾の空白だけ除去する |

🔸 使用例

```java
String s = "　Hello World　";

System.out.println(s.stripLeading());  // "Hello World　"
System.out.println(s.stripTrailing()); // "　Hello World"
```

---

# 🔹 よくある実務パターン

| シチュエーション | 使い方例 |
| --- | --- |
| ユーザー入力の空白を厳密に除去したい | `input.strip()` |
| 多言語対応のテキスト入力を正規化したい | `text.strip()` |
| 先頭空白だけ除去（左寄せ整形など） | `text.stripLeading()` |
| 末尾空白だけ除去（ファイル保存時に余計な改行カットなど） | `text.stripTrailing()` |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 先頭・末尾の空白を除去 | ✅ すべてのUnicode空白に対応 |
| 中間の空白は？ | ❌ そのまま（中間の空白は対象外） |
| trim()と比べたメリット | ✅ 全角空白も除去できる、より国際化に強い |
| 元の文字列は変わる？ | ❌ 変わらない（新しいStringを返す） |
| Javaのバージョン注意点 | Java 11以降のみ使える！（それ以前はtrim()のみ） |

---

# ✨ 超シンプルまとめ

> strip()は「先頭と末尾の全種類の空白を除去する」！
trim()はASCII空白だけ、strip()はUnicode空白もカバー！
Java11以降専用！
> 

✅ この感覚を押さえれば、試験でも実務でも安心して使いこなせます！

以下に、**「strip()専用 引っかけ問題集（trimとの違いを問う応用問題付き 全8問）」** をご用意しました！

今回は、

**「stripとtrimの違い（特に全角空白対応）」「Javaバージョン注意」「中間空白の扱い」**

を正確に見抜けるようになることが目標です！

---

# 🔺 `strip()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：基本 strip() 動作

```java
String s = "   Hello World   ";
System.out.println("[" + s.strip() + "]");
```

### ✅ 問題 1の答え

**答え**：A. `[Hello World]`

**解説**：

- strip()は先頭と末尾の空白をすべて除去する！

---

### ❓ 問題 2：trim()との違い（全角スペース）

```java
String s = "　Hello World　"; // 全角スペース付き
System.out.println("[" + s.trim() + "]");
System.out.println("[" + s.strip() + "]");
```

### ✅ 問題 2の答え

**答え**：B. `[　Hello World　]`, `[Hello World]`

**解説**：

- trim()は全角スペースを除去できないが、strip()は除去できる！

---

### ❓ 問題 3：strip()が対象とする空白文字は？

A. 半角スペースのみ

B. 全角スペースのみ

C. タブ文字と改行のみ

D. すべてのUnicode空白文字

### ✅ 問題 3の答え

**答え**：D. すべてのUnicode空白文字

**解説**：

- strip()はUnicodeの空白文字すべてが対象！

---

### ❓ 問題 4：strip()が使えるJavaバージョンは？

A. Java 8以降

B. Java 9以降

C. Java 10以降

D. Java 11以降

### ✅ 問題 4の答え

**答え**：D. Java 11以降

**解説**：

- Java 11から追加された新メソッド！

---

### ❓ 問題 5：中間の空白はどうなるか？

```java
String s = "Hello    World";

System.out.println("[" + s.strip() + "]");
```

### ✅ 問題 5の答え

**答え**：C. `[Hello    World]`

**解説**：

- 中間の空白には**strip()もtrim()も一切影響しない**！

---

### ❓ 問題 6：stripLeading()の挙動

```java
String s = "   Hello World   ";

System.out.println("[" + s.stripLeading() + "]");
```

### ✅ 問題 6の答え

**答え**：B. `[Hello World   ]`

**解説**：

- stripLeading()は先頭だけ除去、末尾の空白は残る！

---

### ❓ 問題 7：stripTrailing()の挙動

```java
String s = "   Hello World   ";

System.out.println("[" + s.stripTrailing() + "]");
```

### ✅ 問題 7の答え

**答え**：C. `[   Hello World]`

**解説**：

- stripTrailing()は末尾だけ除去、先頭の空白は残る！

---

### ❓ 問題 8：nullに対してstrip()を呼んだら？

```java
String s = null;
System.out.println(s.strip());
```

### ✅ 問題 8の答え

**答え**：C. 実行時例外（NullPointerException）

**解説**：

- nullに対してメソッドを呼ぶとNPE発生！

---

# ✅ strip()とtrim()の最重要比較まとめ

| 比較項目 | trim() | strip() |
| --- | --- | --- |
| 登場バージョン | Java 1.0 | Java 11 |
| 対応空白範囲 | ASCIIスペースのみ | Unicode空白全対応 |
| 全角スペース除去できる？ | ❌ できない | ✅ できる |
| 中間空白に影響する？ | ❌ しない | ❌ しない |
| null対象にそのまま呼べる？ | ❌ 呼べない（NullPointerException） | ❌ 呼べない（NullPointerException） |

---

# ✨ 超シンプルまとめ

> strip()はUnicode全対応！
trim()はASCIIだけ！
中間の空白はstripしても変わらない！
Java11以降専用なのでバージョンに注意！
> 

✅ この感覚を押さえれば、試験でも実務でもバッチリ対応できます！

以下に、**「strip()＋replaceAll()応用 実務パターン集（住所・氏名クリーニング版）」** をご用意しました！

実務現場でよく遭遇する、

**「ユーザー入力の整形」「空白・特殊文字の除去・正規化」**

をテーマにしています！

---

# 🔺 `strip()`＋`replaceAll()`応用 実務パターン集（住所・氏名クリーニング）

---

## 🏠 住所クリーニングパターン

---

### ❓ ① 住所の前後空白除去（strip）

```java
String address = "　東京都 千代田区 一ツ橋　";
String cleaned = address.strip();

System.out.println(cleaned);  // → "東京都 千代田区 一ツ橋"
```

✅ **先頭・末尾の全角・半角空白を一括除去！**

---

### ❓ ② 住所の連続スペースを1つにまとめる（replaceAll）

```java
String address = "東京都   千代田区    一ツ橋";
String normalized = address.replaceAll(" +", " ");

System.out.println(normalized);  // → "東京都 千代田区 一ツ橋"
```

✅ **連続したスペース（半角空白）を1個にまとめる！**

---

### ❓ ③ 住所中の全角スペースも削除して正規化

```java
String address = "東京都　千代田区　一ツ橋"; // 全角スペース(U+3000)入り

String cleaned = address.replaceAll("　", "").replaceAll(" +", " ");

System.out.println(cleaned);  // → "東京都千代田区一ツ橋"
```

✅ **全角スペースを消してから半角スペース整形！**

---

### ❓ ④ 住所の改行を除去する（replaceAll）

```java
String address = "東京都千代田区\n一ツ橋";
String flattened = address.replaceAll("\\s+", " ");

System.out.println(flattened);  // → "東京都千代田区 一ツ橋"
```

✅ **\s+は空白・タブ・改行すべてにマッチする！**

---

## 👤 氏名クリーニングパターン

---

### ❓ ⑤ 氏名の前後空白除去（strip）

```java
String name = "　山田 太郎　";
String cleaned = name.strip();

System.out.println(cleaned);  // → "山田 太郎"
```

✅ **氏名もstrip()でまず前後空白を正規化！**

---

### ❓ ⑥ 氏名の中の余計なスペースまとめ（replaceAll）

```java
String name = "山田    太郎";
String normalized = name.replaceAll(" +", " ");

System.out.println(normalized);  // → "山田 太郎"
```

✅ **連続空白は1個にまとめる！（氏名入力あるある）**

---

### ❓ ⑦ 氏名中の半角スペースを全角スペースに統一する

```java
String name = "山田 太郎";

String unified = name.replaceAll(" ", "　");

System.out.println(unified);  // → "山田　太郎"
```

✅ **日本語入力では全角スペースを使いたい場面もある！（申請書類など）**

---

### ❓ ⑧ 氏名に紛れ込んだ制御文字を除去する

```java
String name = "山田\t太郎\n";
String cleaned = name.replaceAll("\\s+", "");

System.out.println(cleaned);  // → "山田太郎"
```

✅ **\s+を使えばタブや改行も一括除去できる！**

---

# ✅ strip()＋replaceAll() 活用ポイントまとめ

| 目的 | 適したメソッド |
| --- | --- |
| 先頭・末尾の空白除去 | `strip()` |
| 半角スペースの連続まとめ | `replaceAll(" +", " ")` |
| 全角スペースの除去 | `replaceAll("　", "")` |
| 改行・タブ・制御文字の除去 | `replaceAll("\\s+", "")` |
| 半角スペースを全角スペースに統一 | `replaceAll(" ", "　")` |

---

# ✨ 超シンプルまとめ

> strip()でまず「前後の空白を正規化」して、
replaceAll()で「中間の空白・特殊文字を正規化」する！
> 

✅ これが実務における**最強のデータクレンジング基本パターン**です！