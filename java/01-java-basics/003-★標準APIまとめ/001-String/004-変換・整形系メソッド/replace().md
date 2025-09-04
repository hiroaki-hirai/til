# replace()

---

# ✅ `String.replace()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `replace(CharSequence target, CharSequence replacement)` |
| 戻り値の型 | `String`（置き換え後の新しい文字列） |
| 主な用途 | **単純な文字列の置換**（正規表現は使わない！） |

---

# 🔹 シグネチャ（定義）

```java
public String replace(CharSequence target, CharSequence replacement)
```

- **target**：置き換え対象（文字列や文字）
- **replacement**：置き換える内容

---

# 🔹 使用例（基本）

```java
String s = "apple-banana-cherry";
String result = s.replace("-", "_");

System.out.println(result);  // → "apple_banana_cherry"
```

✅ ハイフン（`-`）をアンダースコア（`_`）に**単純に置換**！

---

# 🔹 重要なポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 正規表現を解釈するか？ | ❌ しない！（あくまで「その文字列そのまま」を置換する） |
| 元の文字列は変更されるか？ | ❌ 変更されない（Stringはimmutable＝不変オブジェクト） |
| 全部の一致箇所を置き換えるか？ | ✅ はい、**全部**対象になる（最初だけとかではない） |

---

# 🔹 replace()とreplaceAll()の違い

| 比較項目 | replace() | replaceAll() |
| --- | --- | --- |
| 正規表現対応 | ❌ しない（単純な文字列マッチ） | ✅ する（regexでマッチする） |
| 用途 | 安全な文字列置換 | 正規表現パターンに従った置換 |
| 特徴 | 意図しない置換が起きにくい | 正規表現ミスに注意が必要 |

---

### 🔸 具体例：replaceとreplaceAllの違い

```java
String s = "a.b.c";

System.out.println(s.replace(".", "_"));    // → "a_b_c"
System.out.println(s.replaceAll(".", "_")); // → "___"
```

- `replace()`は文字列の`.`そのものを置換！
- `replaceAll()`は`.`が「任意1文字」と解釈されて全文字が置換される！

---

# 🔹 さらに細かいバリエーション

## 🔸 文字単位での置換（char版）

```java
public String replace(char oldChar, char newChar)
```

```java
String s = "hello";
String result = s.replace('l', 'x');

System.out.println(result);  // → "hexxo"
```

✅ **char型**版なら、1文字単位の置換も可能！

---

# 🔹 よくある実務パターン例

| シチュエーション | replace()の使い方例 |
| --- | --- |
| ハイフン→アンダースコア変換 | `s.replace("-", "_")` |
| 特定の単語をまとめて変換したい場合 | `s.replace("apple", "orange")` |
| ログ出力で改行コードを見やすい文字に | `log.replace("\n", "[改行]")` |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 正規表現を解釈しない | 安心して単純な置換に使える |
| 置換対象すべてを一括置換する | ✅ 1箇所だけ置換したい場合は他の方法が必要 |
| 元の文字列は変わる？ | ❌ 変わらない（新しいStringを返す） |
| 安全に使えるケース | ピリオド`.`やアスタリスク`*`など、特殊記号でも意図せず暴走しない！ |

---

# ✨ 超シンプルまとめ

> replace()は単純な文字列置換用！
正規表現を解釈しないから安心！
元の文字列はそのまま（immutable）！
> 

✅ この感覚を押さえておけば、試験でも実務でもバッチリ使いこなせます！

以下に、**「replace()専用 引っかけ問題集（replaceAllとの混同対策付き全8問）」** をご用意しました！

**「単純置換なのか？正規表現解釈が必要なのか？」「エスケープは必要か？」**

を正しく見抜けるかがカギになります！

---

# 🔺 `replace()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：単純な文字列置換

```java
String s = "apple-banana-cherry";
System.out.println(s.replace("-", "_"));
```

### ✅ 問題 1の答え

**答え**：A. `"apple_banana_cherry"`

**解説**：

- replace()は単純な文字列置換！

---

### ❓ 問題 2：ピリオド`.`を置換

```java
String s = "a.b.c";
System.out.println(s.replace(".", "_"));
```

### ✅ 問題 2の答え

**答え**：A. `"a_b_c"`

**解説**：

- replace()は正規表現を解釈しないので、`.`は単なる文字扱い。

---

### ❓ 問題 3：replaceAll()でピリオドを置換しようとした場合

```java
String s = "a.b.c";
System.out.println(s.replaceAll(".", "_"));
```

### ✅ 問題 3の答え

**答え**：B. `"___"`

**解説**：

- replaceAll()は正規表現を解釈するため、`.`は「任意1文字」にマッチして全文字が置換される！

---

### ❓ 問題 4：アスタリスクの置換（replace使用）

```java
String s = "a*b*c";
System.out.println(s.replace("*", "-"));
```

### ✅ 問題 4の答え

**答え**：A. `"a-b-c"`

**解説**：

- replace()は単純にをに置換！

---

### ❓ 問題 5：アスタリスクの置換（replaceAll使用）

```java
String s = "a*b*c";
System.out.println(s.replaceAll("*", "-"));
```

### ✅ 問題 5の答え

**答え**：C. 実行時例外（PatternSyntaxException）

**解説**：

- replaceAll()は正規表現解釈のため、単独のは正規表現エラーになる！

---

### ❓ 問題 6：空文字列に対するreplace()

```java
String s = "";
System.out.println(s.replace("a", "b"));
```

### ✅ 問題 6の答え

**答え**：A. `""`（空文字列）

**解説**：

- 空文字列に対して置換しても、置換対象がないのでそのまま空文字列が返る。

---

### ❓ 問題 7：文字列が一致しない場合の置換

```java
String s = "openai";
System.out.println(s.replace("x", "y"));
```

### ✅ 問題 7の答え

**答え**：A. `"openai"`

**解説**：

- "x"が存在しないため、置換されずそのまま！

---

### ❓ 問題 8：nullに対してreplace()を呼んだら？

```java
String s = null;
System.out.println(s.replace("a", "b"));

```

### ✅ 問題 8の答え

**答え**：C. 実行時例外（NullPointerException）

**解説**：

- nullに対してメソッド呼び出しをするとNullPointerExceptionが発生する！

---

# ✅ replace()とreplaceAll() 最重要まとめ

| 比較観点 | replace() | replaceAll() |
| --- | --- | --- |
| 正規表現を使う？ | ❌ 使わない（単純な文字列マッチ） | ✅ 使う（regex解釈される） |
| 特殊文字（`.`や`*`）に注意？ | ❌ そのまま文字扱い | ✅ 意味を持つのでエスケープ必須 |
| 安全に単純置換だけしたい場合 | ✅ replace()を使うべき | ❌ replaceAll()はミスを誘発しやすい（要注意） |
| メモリ効率 | どちらもStringはimmutableなので変わらない | 同じ |

---

# ✨ 超シンプルまとめ

> replace()は単純な文字列置換用！
replaceAll()は正規表現ベースなので注意！
特に.や*に油断しない！
> 

✅ この感覚を押さえれば、試験も実務も安心して使えます！

以下に、**「replace() / replaceAll() / replaceFirst() クロス比較演習（応用版・全8問）」** をご用意しました！

ここでは、

**「どのメソッドが正しい結果を出すか？」「なぜ意図通りに動かないのか？」**

を考え抜く練習になります！

---

# 🔺 `replace()` / `replaceAll()` / `replaceFirst()` クロス比較演習（応用版）

---

## 【問題編】

---

### ❓ 問題 1：基本比較（ピリオドを置換）

```java
String s = "a.b.c";

System.out.println(s.replace(".", "_"));
System.out.println(s.replaceAll(".", "_"));
System.out.println(s.replaceFirst(".", "_"));
```

### ✅ 問題 1の答え

**答え**：A. `"a_b_c"`, `"___"`, `"_b.c"`

**解説**：

- replace()は単純置換
- replaceAll()は`.`が任意文字にマッチする
- replaceFirst()は最初の1文字だけ置換

---

### ❓ 問題 2：replace()とreplaceAll()の違い（正規表現）

```java
String s = "1+2=3";

System.out.println(s.replace("+", "-"));
System.out.println(s.replaceAll("+", "-"));
```

replaceAll()はどうなるか？

### ✅ 問題 2の答え

**答え**：B. 例外が発生する（PatternSyntaxException）

**解説**：

- `+`は正規表現で「直前文字の1回以上繰り返し」を意味するので、単独ではエラー！

---

### ❓ 問題 3：replaceFirst()の挙動

```java
String s = "apple apple apple";

System.out.println(s.replaceFirst("apple", "orange"));
```

### ✅ 問題 3の答え

**答え**：B. `"orange apple apple"`

**解説**：

- replaceFirst()は最初にマッチしたものだけを置換！

---

### ❓ 問題 4：replaceAll()とreplaceFirst()で対象が複数の場合

```java
String s = "hello hello world";

System.out.println(s.replaceAll("hello", "hi"));
System.out.println(s.replaceFirst("hello", "hi"));

```

### ✅ 問題 4の答え

**答え**：A. `"hi hi world"`, `"hi hello world"`

**解説**：

- replaceAll()はすべて置換
- replaceFirst()は最初だけ！

---

### ❓ 問題 5：ドット`.`を正しくreplaceAllしたい

```java
String s = "file.name.extension";

System.out.println(s.replaceAll(".", "-"));

```

意図通りにピリオドだけを置換したい。正しい書き直しは？

### ✅ 問題 5の答え

**答え**：A.

```java
s.replaceAll("\\.", "-")
```

**解説**：

- `.`は正規表現では任意文字→エスケープ必須！

---

### ❓ 問題 6：replace()とreplaceAll()で「複数空白」を1個にまとめるならどちら？

```java
String s = "hello    world   java";
```

空白を1個にまとめたい。正しい選択は？

### ✅ 問題 6の答え

**答え**：B. replaceAll()

**解説**：

- 連続空白（" +"）を1個空白にまとめるには正規表現が必要！

---

### ❓ 問題 7：replaceFirst()で改行を除去できるか？

```java
String s = "line1\nline2\nline3";

System.out.println(s.replaceFirst("\n", ""));
```

### ✅ 問題 7の答え

**答え**：B. `"line1line2\nline3"`

**解説**：

- replaceFirst()は最初の`\n`（改行）だけ削除される！

---

### ❓ 問題 8：replace()対象がない場合

```java
String s = "openai";

System.out.println(s.replace("x", "y"));
System.out.println(s.replaceFirst("x", "y"));
```

### ✅ 問題 8の答え

**答え**：A. `"openai"`, `"openai"`

**解説**：

- 対象がない場合は、元の文字列がそのまま返る（例外にはならない）！

---

# ✅ replace() / replaceAll() / replaceFirst() まとめ

| 比較項目 | replace() | replaceAll() | replaceFirst() |
| --- | --- | --- | --- |
| 正規表現を解釈する？ | ❌ しない | ✅ する | ✅ する |
| すべて置換するか？ | ✅ 全部 | ✅ 全部 | ❌ 最初の1つだけ |
| 任意文字や特殊記号注意？ | ❌ 気にしなくてよい | ✅ 気にする（エスケープ必要） | ✅ 気にする（エスケープ必要） |
| 安全な単純置換には？ | ✅ replace()が最適 | ❌ replaceAll()は正規表現理解必須 | ❌ replaceFirst()も正規表現理解必須 |

---

# ✨ 超シンプルまとめ

> replace()＝安全な単純置換！
replaceAll()＝全部正規表現ベース！
replaceFirst()＝最初だけ正規表現ベース！
特殊文字（. * + ? など）にはエスケープ注意！
> 

✅ ここまで押さえておけば、試験・実務どちらもプロレベルで使い分けられます！