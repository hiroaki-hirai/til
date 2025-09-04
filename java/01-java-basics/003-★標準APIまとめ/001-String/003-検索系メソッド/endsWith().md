# endsWith()

`String.endsWith()` は、

**「文字列が指定した接尾辞（suffix）で終わっているかどうかを判定するメソッド」** です。

- *startsWith()の「末尾版」**だと思えばOKです！

Java Silver/Gold試験でも、**startsWith()/contains()/endsWith()の違い**を押さえる問題がよく出ます。

---

# ✅ `String.endsWith()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `endsWith(String suffix)` |
| 戻り値の型 | `boolean`（指定接尾辞で終わっていればtrue） |
| 主な用途 | ファイル拡張子チェック、後方マッチング |

```java
public boolean endsWith(String suffix)
```

- **引数**：接尾辞となる文字列（`String suffix`）
- **戻り値**：
    - `true` → 指定文字列で**終わっている**
    - `false` → 終わっていない

---

# 🔹 使用例（基本）

```java
String s = "HelloWorld";

System.out.println(s.endsWith("World")); // → true
System.out.println(s.endsWith("Hello")); // → false
```

✅ **末尾（最後）が指定文字列と完全一致**しているかをチェックします！

---

# 🔹 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 大文字小文字の区別 | ✅ 区別する（完全一致のみ） |
| 空文字 `""` をsuffixに渡した場合 | ✅ 常にtrue（すべての文字列は空文字で終わるとみなす） |
| nullを渡した場合 | ❌ NullPointerExceptionが発生する |
| 正規表現が使えるか | ❌ 使えない（単純な文字列一致のみ） |

---

# 🔹 よくある用途例

- **ファイル拡張子判定**
    - 例：「.txt」で終わっているファイル名だけ抽出
- **特定のURLエンドパターン判定**
    - 例：URLが`.html`で終わっているか？
- **メッセージ、ログの終了判定**
    - 例："完了"で終わるログだけ検出

例：

```java
String fileName = "document.txt";
if (fileName.endsWith(".txt")) {
    System.out.println("テキストファイルです");
}
```

✅ **実務ではファイル拡張子チェックによく使われます！**

---

# 🔸 startsWith() との違い

| 比較項目 | startsWith() | endsWith() |
| --- | --- | --- |
| 判定範囲 | 先頭 | 末尾 |
| 大文字小文字 | 区別する | 区別する |
| 空文字を渡す | true | true |
| null渡す | NullPointerException発生 | 同じく発生 |

---

# 🔹 よくあるミス・注意点

| ミス内容 | 正しい理解 |
| --- | --- |
| 大文字小文字は無視される？ | ❌ 完全一致（大小区別する） |
| nullを渡すとfalseになる？ | ❌ NullPointerExceptionが発生する |
| 正規表現が使える？ | ❌ 単純な文字列比較のみ（正規表現は使えない） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 判定基準 | 末尾が指定suffixと完全一致ならtrue |
| 大文字小文字の扱い | 区別する（完全一致のみ許容） |
| 空文字を渡した場合 | true |
| nullを渡した場合 | NullPointerException |
| よく使うシーン | ファイル拡張子チェック、URL後方マッチなど |

---

# ✨ 超シンプルまとめ

> endsWith()は「末尾一致」を厳密に判定する！
大小区別あり、空文字は常にtrue、nullはNG！
> 

✅ この感覚を押さえておけば完璧です！

---

# 🔺 `String.endsWith()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：基本的な末尾一致チェック

```java
String s = "Programming";

System.out.println(s.endsWith("ming"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- "ming"は文字列の末尾に一致しているので `true`！

---

### ❓ 問題 2：大文字小文字違いに注意

```java
String s = "HelloWorld";

System.out.println(s.endsWith("world"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- "World"と"world"は違う！
- ✅ **endsWith()は大文字小文字を区別する**！

---

### ❓ 問題 3：空文字列をsuffixに渡した場合

```java
String s = "ChatGPT";

System.out.println(s.endsWith(""));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 空文字列 `""` は、**すべての文字列が空文字で終わる**とみなされる。
- ✅ 常に`true`になる！

---

### ❓ 問題 4：部分一致と勘違いパターン

```java
String s = "development";

System.out.println(s.endsWith("elop"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- "elop"は途中にあるけど、**末尾ではない**！
- ✅ endsWithは**「末尾が一致しているか」だけ**を判定する。

---

### ❓ 問題 5：nullを渡した場合

```java
String s = "TestString";

System.out.println(s.endsWith(null));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- nullを渡すと**NullPointerException**が発生！
- ✅ null安全ではないので注意が必要！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 大文字小文字を無視して比較できる？ | ❌ 完全一致（大文字小文字を区別する） |
| 空文字を渡すとfalseになる？ | ❌ 空文字は常にtrue（どの文字列も空文字で終わる） |
| 部分一致でもtrueになる？ | ❌ 完全に**末尾一致**していなければfalse |
| nullを渡すとfalseになる？ | ❌ NullPointerExceptionが発生する |
| 正規表現が使える？ | ❌ 正規表現ではなく単純な文字列比較だけ |

---

# ✨ 超まとめ

> endsWith()は「末尾が完全一致」しているかだけをチェック！
大小区別あり、空文字はtrue、nullはNG！
> 

✅ この感覚を押さえておけば、試験でも実務でも間違えません！

---

# 🔺 `String.endsWith()` 応用演習セット（拡張子チェック・パターンマッチング）

---

## 【拡張子チェック編】

---

### ❓ 問題 1：.txtファイルか判定

```java
String filename = "document.txt";

System.out.println(filename.endsWith(".txt"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- ".txt"で終わっているので**true**。

✅ 実務では「拡張子確認」で非常によく使われる！

---

### ❓ 問題 2：大文字小文字の違いに注意

```java
String filename = "document.TXT";

System.out.println(filename.endsWith(".txt"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- `.TXT`（大文字）と`.txt`（小文字）は違うとみなされ、**false**。

✅ 大小区別ありに注意！

---

## 【リストフィルタリング編】

---

### ❓ 問題 3：リストから.jpgファイルだけ抽出

```java
List<String> files = List.of("photo.jpg", "image.png", "picture.jpg");

files.stream()
    .filter(f -> f.endsWith(".jpg"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"photo.jpg"` のみ

B. `"photo.jpg"`, `"picture.jpg"`

C. `"image.png"`

D. 出力なし

**答え**：B. `"photo.jpg"`, `"picture.jpg"`

**解説**：

- `.jpg`で終わっているファイルだけを抽出している！

✅ stream＋endsWithはリストフィルタリングの黄金パターン！

---

### ❓ 問題 4：複数拡張子を許可するフィルタリング

```java
List<String> files = List.of("data.csv", "info.txt", "report.doc");

files.stream()
    .filter(f -> f.endsWith(".csv") || f.endsWith(".txt"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"data.csv"`, `"info.txt"`

B. `"report.doc"`

C. `"info.txt"` のみ

D. 出力なし

**答え**：A. `"data.csv"`, `"info.txt"`

**解説**：

- `.csv`または`.txt`のどちらかで終わるものを抽出！

✅ 複数条件を組み合わせるときは`||`でつなぐ！

---

## 【パターンマッチング応用編】

---

### ❓ 問題 5：特定のURL末尾判定

```java
String url = "https://example.com/index.html";

System.out.println(url.endsWith(".html"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- ".html"で終わっているので**true**！

✅ サイト判定・ルーティング判定でもよく使われる！

---

### ❓ 問題 6：拡張子を大文字小文字無視して判定する場合

```java
String filename = "image.JPG";

boolean result = filename.toLowerCase().endsWith(".jpg");
System.out.println(result);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- `toLowerCase()`で小文字化してからendsWith判定している！
- 大文字小文字を無視した比較ができる！

✅ 実務では「小文字化してからendsWith()」が鉄則！

---

# ✅ 応用まとめ

| 応用パターン | ポイント |
| --- | --- |
| ファイル拡張子チェック | `.endsWith(".拡張子")` で判定できる |
| 大文字小文字無視して判定 | `.toLowerCase().endsWith(".xxx")` を使う |
| 複数条件で絞り込み | `.endsWith(".a") |
| URL判定やルーティングにも応用可 | `.endsWith("/index.html")` などで判定できる |

---

# ✨ 超シンプルまとめ

> endsWith()は「末尾パターンを簡単にチェックする武器」！
実務でも試験でも頻出、大文字小文字・null渡しに注意！
> 

✅ これを押さえれば、開発でもテストでも安心です！