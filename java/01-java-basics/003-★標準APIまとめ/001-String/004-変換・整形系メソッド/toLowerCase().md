# toLowerCase()

`String.toLowerCase()` は、

**「文字列中のすべてのアルファベットを小文字に変換するメソッド」** です。

Java Silver/Gold試験でも、「大文字小文字を無視して比較するテクニック」として非常によく問われます！

---

# ✅ `String.toLowerCase()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `toLowerCase()` |
| 戻り値の型 | `String`（小文字に変換された新しい文字列を返す） |
| 主な用途 | 大文字小文字を無視した比較・検索など |

---

# 🔹 シグネチャ（定義）

```java
public String toLowerCase()
public String toLowerCase(Locale locale)
```

- 引数なし版
- `Locale`指定版（地域による大小変換ルールに従う）

✅ 通常は**引数なし版**を使えば十分です！

---

# 🔹 使用例（基本）

```java
String s = "Hello World!";

System.out.println(s.toLowerCase()); 
// → "hello world!"
```

✅ アルファベットだけが対象！

数字や記号、日本語などは**変わりません**。

---

# 🔹 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 元の文字列は変更される？ | ❌ 変更されない（**新しいStringを返す**） |
| 英数字以外（記号、漢字など）は？ | ✅ 変換対象外（そのまま） |
| 大文字小文字無視したい時の使い方 | `toLowerCase()`＋`equals()` や `contains()` |
| Localeを指定すると？ | 地域依存のルールで変換される（例えばトルコ語問題など） |

---

# 🔹 大文字小文字無視で比較する例

```java
String s1 = "Java";
String s2 = "JAVA";

if (s1.toLowerCase().equals(s2.toLowerCase())) {
    System.out.println("同じ内容です！");
}
```

✅ どちらも小文字に変換してから比較すれば、大文字小文字を無視できる！

---

# 🔹 Locale版との違い

通常版（引数なし）は、**システムのデフォルトロケール**（Locale）に従います。

ただし、国によっては小文字変換ルールが違うので、

正確を期したいなら明示的に`Locale.ROOT`を使うこともあります！

例：

```java
String s = "Istanbul";

System.out.println(s.toLowerCase(Locale.ROOT)); 
// → "istanbul"（正確な小文字化）
```

✅ ただし、**試験対策では基本、引数なし版だけ抑えておけば大丈夫**です！

---

# 🔹 よくある実務パターン

| シチュエーション | 使い方例 |
| --- | --- |
| ユーザー名比較（大文字小文字無視） | `input.toLowerCase().equals(stored.toLowerCase())` |
| 検索機能（大文字小文字無視でヒット） | `target.toLowerCase().contains(keyword.toLowerCase())` |
| ファイル拡張子チェック（大小無視） | `filename.toLowerCase().endsWith(".jpg")` |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 変換対象 | アルファベット（A～Z）を小文字（a～z）に変換 |
| 元の文字列 | 変更しない（新しいStringを返す） |
| 数字・記号・漢字 | 変換しない |
| Locale版の存在 | あり（国・言語による特殊ルールに対応できる） |

---

# ✨ 超シンプルまとめ

> toLowerCase()は「文字列を小文字に変換」して、大文字小文字無視比較や検索に使う！
元の文字列は不変、新しい文字列が返る！
> 

✅ この感覚を押さえておけば、試験も実務もバッチリ対応できます！

---

# 🔺 `String.toLowerCase()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：小文字変換の基本動作

```java
String s = "Hello World";
System.out.println(s.toLowerCase());
```

この出力は？

A. `"hello world"`

B. `"HELLO WORLD"`

C. `"Hello World"`

D. コンパイルエラー

**答え**：A. `"hello world"`

**解説**：

- アルファベットの大文字だけが小文字に変換される！

✅ 数字や記号には影響しない！

---

### ❓ 問題 2：元の文字列は変わるか？

```java
String s = "OpenAI";
s.toLowerCase();
System.out.println(s);
```

この出力は？

A. `"openai"`

B. `"OpenAI"`

C. `"OPENAI"`

D. コンパイルエラー

**答え**：B. `"OpenAI"`

**解説**：

- **toLowerCase()は新しい文字列を返すだけ**。
- 元の`s`には何も影響しない！

✅ **Stringは不変（immutable）**！

---

### ❓ 問題 3：記号・数字の扱い

```java
String s = "A1B2#C3";

System.out.println(s.toLowerCase());
```

この出力は？

A. `"a1b2#c3"`

B. `"a1b2c3"`

C. `"A1B2#C3"`

D. コンパイルエラー

**答え**：A. `"a1b2#c3"`

**解説**：

- アルファベットだけが小文字に！
- 数字や記号 `#` はそのまま残る！

✅ **数字・記号は変換されない**！

---

### ❓ 問題 4：大文字小文字無視比較

```java
String a = "JaVa";
String b = "java";

System.out.println(a.equals(b));
System.out.println(a.toLowerCase().equals(b.toLowerCase()));
```

出力はどれ？

A. `true`, `true`

B. `false`, `true`

C. `false`, `false`

D. `true`, `false`

**答え**：B. `false`, `true`

**解説**：

- `equals()`は大小区別するので`false`。
- 両方小文字化してから比較すれば`true`！

✅ 大文字小文字を無視したいなら**toLowerCase()後に比較！**

---

### ❓ 問題 5：Localeを考慮した場合（参考問題）

```java
String s = "Istanbul";

System.out.println(s.toLowerCase(Locale.ROOT));
```

この出力は？

A. `"istanbul"`

B. `"Istanbul"`

C. `"İstanbul"`（トルコ語表記）
D. コンパイルエラー

**答え**：A. `"istanbul"`

**解説**：

- Locale.ROOTを使うと、**国や言語に依存しない標準的な小文字化**が行われる！

✅ 普通は引数なしでも問題ないが、**特殊な言語（トルコ語など）では注意が必要**！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 元の文字列が変わる？ | ❌ 変わらない（Stringはimmutable） |
| 数字や記号も小文字化される？ | ❌ 変わらない（アルファベットだけ） |
| toLowerCase()だけで大文字小文字無視比較できる？ | ❌ 比較には`equals()`などと組み合わせる必要がある |
| nullを対象にtoLowerCase()できる？ | ❌ nullならNullPointerExceptionが発生する |

---

# ✨ 超まとめ

> toLowerCase()は「アルファベットだけ小文字化」、
元の文字列はそのまま！
比較にはtoLowerCase()＋equals()を組み合わせる！
> 

✅ この感覚を押さえれば、試験も実務も安心です！

---

# 🔺 `toLowerCase()`実務パターン別課題集（検索・認証・ファイルチェック）

---

## 【検索機能編】

---

### ❓ 問題 1：キーワード検索（大文字小文字無視）

```java
String text = "Java Programming Language";

System.out.println(text.toLowerCase().contains("programming"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- もともと"Programming"（大文字P）だが、小文字化してからcontainsしているので、**true**！

✅ **検索対象と検索語の両方を小文字化する**のがコツ！

---

### ❓ 問題 2：検索語も小文字化するパターン

```java
String text = "Welcome to OpenAI";

String keyword = "OPENAI";

boolean found = text.toLowerCase().contains(keyword.toLowerCase());
System.out.println(found);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 両方小文字化すれば、大文字小文字を無視できる！

---

## 【認証機能（ログインIDチェック）編】

---

### ❓ 問題 3：ログインIDの大文字小文字無視比較

```java
String inputId = "User123";
String storedId = "user123";

boolean match = inputId.toLowerCase().equals(storedId.toLowerCase());
System.out.println(match);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 大文字小文字を無視して、正しく一致判定できる！

✅ ログイン認証では**必ず安全に大文字小文字無視比較**！

---

### ❓ 問題 4：不正なnull値を防ぐには？

```java
String inputId = null;
String storedId = "admin";

boolean match = inputId != null && inputId.toLowerCase().equals(storedId.toLowerCase());

System.out.println(match);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- nullチェックをしてからtoLowerCase()を呼び出しているので安全！
- nullの場合、`false`に落ちる。

✅ **nullチェック→toLowerCase()→equals()**の流れが実務鉄則！

---

## 【ファイルチェック編】

---

### ❓ 問題 5：拡張子が.jpgかどうか判定（大文字小文字無視）

```java
String filename = "Picture.JPG";

boolean isJpg = filename.toLowerCase().endsWith(".jpg");
System.out.println(isJpg);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 小文字化してからendsWithしているので、拡張子判定に成功！

✅ **拡張子チェックはtoLowerCase()＋endsWith()の鉄板パターン！**

---

### ❓ 問題 6：複数拡張子許可（.jpg, .png）

```java
String filename = "icon.PNG";

boolean isImage = filename.toLowerCase().endsWith(".jpg") || filename.toLowerCase().endsWith(".png");
System.out.println(isImage);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 小文字化後、`.png`にもマッチしている！

✅ **OR条件（||）を使えば複数拡張子に対応できる！**

---

# ✅ 実務パターン別まとめ

| シチュエーション | 使い方パターン |
| --- | --- |
| 検索機能（大文字小文字無視） | `text.toLowerCase().contains(keyword.toLowerCase())` |
| 認証機能（ID/パスワード比較） | `input.toLowerCase().equals(stored.toLowerCase())` |
| ファイル拡張子チェック | `filename.toLowerCase().endsWith(".jpg")` |

---

# ✨ 超シンプルまとめ

> toLowerCase()は「大小無視したいならまず小文字に揃える！」
検索も認証も拡張子チェックも基本この形で安全対応！
> 

✅ この感覚を持っていれば、試験も実務もバッチリ対応できます！