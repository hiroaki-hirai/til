# toUpperCase()

`String.toUpperCase()` は、

**「文字列中のすべてのアルファベットを大文字に変換するメソッド」**です！

`toLowerCase()`の「逆バージョン」ですね。

Java Silver/Gold試験でも、「大小無視比較」や「出力フォーマット」に使うパターンが問われることがあります！

---

# ✅ `String.toUpperCase()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `toUpperCase()` |
| 戻り値の型 | `String`（大文字に変換された新しい文字列を返す） |
| 主な用途 | アルファベットを大文字化する |

---

# 🔹 シグネチャ（定義）

```java
public String toUpperCase()
public String toUpperCase(Locale locale)
```

- 引数なし版
- `Locale`指定版（地域ごとの大文字変換ルールに従う版）

✅ ふつうは**引数なし版**を押さえておけば試験対策も実務も十分です！

---

# 🔹 使用例（基本）

```java
String s = "Hello World!";

System.out.println(s.toUpperCase()); 
// → "HELLO WORLD!"
```

---

# 🔹 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 元の文字列は変更される？ | ❌ 変更されない（新しいStringを返す） |
| 英数字以外（記号、漢字など）は？ | ✅ 変換対象外（そのまま） |
| 大文字小文字無視比較で使う？ | ✅ 可能（toUpperCaseしてからequalsなど） |
| Locale指定版 | 特殊な言語ルール（例：トルコ語など）に対応できる |

---

# 🔹 大文字小文字無視で比較する例（toUpperCase版）

```java
String s1 = "java";
String s2 = "JAVA";

if (s1.toUpperCase().equals(s2.toUpperCase())) {
    System.out.println("同じ内容です！");
}
```

✅ 大小を揃えて比較すれば、大文字小文字を無視した比較ができる！

---

# 🔹 Locale版との違い（参考）

Locale版（例えばトルコ語）では大文字小文字ルールが異なることがあります。

**通常の開発や試験では、引数なし版（システムデフォルト）でOK**です！

```java
String s = "istanbul";

System.out.println(s.toUpperCase(Locale.ROOT)); 
// → "ISTANBUL"
```

---

# 🔹 よくある実務パターン

| シチュエーション | 使い方例 |
| --- | --- |
| ログインID比較（大小無視） | `input.toUpperCase().equals(stored.toUpperCase())` |
| キーワード検索（大小無視） | `text.toUpperCase().contains(keyword.toUpperCase())` |
| 出力フォーマット（全部大文字で表示） | `message.toUpperCase()` |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 変換対象 | アルファベット（a～z）を大文字（A～Z）に変換する |
| 元の文字列 | 変更しない（immutable、新しいStringを返す） |
| 数字・記号・漢字 | 変わらない |
| Locale版 | 特殊な言語用、通常は無指定でOK |

---

# ✨ 超シンプルまとめ

> toUpperCase()は「アルファベットを大文字に揃える」ために使う！
大小無視比較や出力フォーマット整形に活躍！
元の文字列はそのまま（immutable）！
> 

✅ この感覚を押さえておけば試験も実務もバッチリです！

---

# 🔺 `String.toUpperCase()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：基本的な大文字化

```java
String s = "hello world";
System.out.println(s.toUpperCase());
```

この出力は？

A. `"HELLO WORLD"`

B. `"hello world"`

C. `"Hello World"`

D. コンパイルエラー

**答え**：A. `"HELLO WORLD"`

**解説**：

- アルファベット小文字がすべて大文字に変換される！

✅ 数字や記号は影響を受けない！

---

### ❓ 問題 2：元の文字列は変わるか？

```java
String s = "ChatGPT";
s.toUpperCase();
System.out.println(s);
```

この出力は？

A. `"CHATGPT"`

B. `"ChatGPT"`

C. `"chatgpt"`

D. コンパイルエラー

**答え**：B. `"ChatGPT"`

**解説**：

- `toUpperCase()`は**新しい文字列を返すだけ**。
- 元の`s`はそのまま！

✅ **Stringはimmutable（不変）**！

---

### ❓ 問題 3：記号・数字を含む場合

```java
String s = "abc123#";

System.out.println(s.toUpperCase());
```

この出力は？

A. `"ABC123#"`

B. `"ABC123"`

C. `"abc123#"`

D. コンパイルエラー

**答え**：A. `"ABC123#"`

**解説**：

- アルファベットだけが大文字に変換。
- 数字と記号 `#` はそのまま残る！

✅ 数字・記号・漢字には影響なし！

---

### ❓ 問題 4：大文字小文字無視で比較できるか？

```java
String a = "spring";
String b = "SPRING";

System.out.println(a.toUpperCase().equals(b.toUpperCase()));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 両方とも`toUpperCase()`してから比較しているので一致！

✅ **大小無視比較にはtoUpperCase()＋equals()が有効**！

---

### ❓ 問題 5：Locale混同注意（参考問題）

```java
String s = "istanbul";

System.out.println(s.toUpperCase(Locale.ROOT));
```

この出力は？

A. `"ISTANBUL"`

B. `"İSTANBUL"`（トルコ語の点付き大文字İ）

C. `"istanbul"`

D. コンパイルエラー

**答え**：A. `"ISTANBUL"`

**解説**：

- Locale.ROOTを指定すれば標準的な変換になる。
- トルコ語特殊ルールを無視して、普通に大文字化される！

✅ **通常開発や試験では、Locale指定は意識しなくてOK**！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 元の文字列が変わる？ | ❌ 変わらない（新しい文字列が作られるだけ） |
| 数字や記号も大文字化される？ | ❌ アルファベットだけ大文字化、数字・記号はそのまま |
| toUpperCase()だけで大文字小文字無視比較できる？ | ❌ 必ず`equals()`や`contains()`と組み合わせる |
| null対象にtoUpperCase()できる？ | ❌ nullならNullPointerExceptionが発生する |

---

# ✨ 超まとめ

> toUpperCase()は「アルファベットを大文字にする」だけ！
元の文字列は不変！
大小無視比較はtoUpperCase()＋equals()でやる！
> 

✅ この感覚を押さえれば、試験も実務も安心です！

---

# 🔺 `toUpperCase()`を使ったフィルタリング応用課題集（全6問）

---

## 【基本編】

---

### ❓ 問題 1：リストから"JAVA"を含む要素だけ抽出

```java
List<String> list = List.of("Java", "JavaScript", "python", "JAVA8");

list.stream()
    .filter(s -> s.toUpperCase().contains("JAVA"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"Java"` のみ

B. `"Java"`, `"JavaScript"`

C. `"Java"`, `"JavaScript"`, `"JAVA8"`

D. `"JavaScript"`, `"python"`

**答え**：C. `"Java"`, `"JavaScript"`, `"JAVA8"`

**解説**：

- 全部toUpperCase()して"JAVA"を含んでいるものを抽出！

✅ **大文字小文字無視のフィルタリングができている**！

---

### ❓ 問題 2："ADMIN"を完全一致で探す（大文字小文字無視）

```java
List<String> users = List.of("admin", "Admin", "administrator", "ADMIN");

users.stream()
    .filter(u -> u.toUpperCase().equals("ADMIN"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"admin"` のみ

B. `"admin"`, `"Admin"`, `"ADMIN"`

C. `"administrator"`

D. `"Admin"`, `"administrator"`

**答え**：B. `"admin"`, `"Admin"`, `"ADMIN"`

**解説**：

- toUpperCaseして"ADMIN"と完全一致するものだけ！

✅ **部分一致ではなく完全一致に注意！**

---

## 【拡張子フィルタリング編】

---

### ❓ 問題 3：拡張子が`.PDF`のファイルを抽出

```java
List<String> files = List.of("doc1.pdf", "doc2.PDF", "doc3.txt");

files.stream()
    .filter(f -> f.toUpperCase().endsWith(".PDF"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"doc1.pdf"`

B. `"doc2.PDF"`

C. `"doc1.pdf"`, `"doc2.PDF"`

D. `"doc3.txt"`

**答え**：C. `"doc1.pdf"`, `"doc2.PDF"`

**解説**：

- ファイル名をtoUpperCaseしてからendsWith(".PDF")！

✅ **拡張子チェックも大文字小文字無視ができる！**

---

## 【応用編】

---

### ❓ 問題 4：特定キーワードを含むファイルだけ抽出（AND条件）

```java
List<String> files = List.of("readme.txt", "README.MD", "INSTALL.TXT");

files.stream()
    .filter(f -> f.toUpperCase().contains("README") && f.toUpperCase().endsWith(".TXT"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"readme.txt"`

B. `"README.MD"`

C. `"INSTALL.TXT"`

D. 出力なし

**答え**：A. `"readme.txt"`

**解説**：

- "README"を含み、かつ".TXT"で終わるものだけ！

✅ 複合条件フィルタもtoUpperCaseを使って正しく書ける！

---

## 【ミスパターン対策】

---

### ❓ 問題 5：比較対象を小文字にしてしまった場合

```java
List<String> words = List.of("Dog", "DOG", "cat");

words.stream()
    .filter(w -> w.toUpperCase().equals("dog"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"Dog"`, `"DOG"`

B. 出力なし

C. `"cat"`

D. 実行時例外

**答え**：B. 出力なし

**解説**：

- toUpperCase()しているのに、比較対象が小文字"dog"なので一致しない！

✅ **比較対象も必ず大文字で揃える**べき！

---

## 【安全設計】

---

### ❓ 問題 6：null要素が混じる場合（安全フィルタリング）

```java
List<String> list = Arrays.asList("Java", null, "JAVA8");

list.stream()
    .filter(s -> s != null && s.toUpperCase().contains("JAVA"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"Java"`

B. `"JAVA8"`

C. `"Java"`, `"JAVA8"`

D. 実行時例外

**答え**：C. `"Java"`, `"JAVA8"`

**解説**：

- nullチェックをしてからtoUpperCaseしているので安全に動作！

✅ **nullチェック＋toUpperCase＋フィルタ**の流れが実務では鉄板！

---

# ✅ まとめ

| 応用パターン | ポイント |
| --- | --- |
| 大文字小文字無視検索 | toUpperCaseしてからcontains() |
| 大文字小文字無視完全一致 | toUpperCaseしてからequals() |
| 拡張子チェック | toUpperCaseしてからendsWith(".XXX") |
| null安全フィルタリング | `s != null && s.toUpperCase().xxx()`の形にする |

---

# ✨ 超シンプルまとめ

> toUpperCase()を使えば、大文字小文字を無視して
安全かつ柔軟にフィルタリングができる！
nullチェックも忘れずに！
> 

✅ この感覚を押さえておけば、実務でもテストでも間違いなく使いこなせます！