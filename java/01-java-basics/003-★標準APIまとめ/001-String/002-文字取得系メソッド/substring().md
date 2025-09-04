# substring()

`String.substring()` は Java において、

**文字列から指定範囲の部分文字列（サブ文字列）を取り出すためのメソッド**です。

Javaの文字列操作の中でも最重要レベルのメソッドの一つで、

**範囲指定の考え方（beginIndexを含み、endIndexを含まない）**が試験・実務の両方でとても重要です！

---

# ✅ `String.substring()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `substring(int beginIndex)` / `substring(int beginIndex, int endIndex)` |
| 戻り値の型 | `String`（部分文字列） |
| 例外 | `StringIndexOutOfBoundsException`（範囲外アクセス時） |

---

# 🔹 シグネチャ（定義）

```java
// 1引数版（beginIndex〜最後まで）
public String substring(int beginIndex)

// 2引数版（beginIndex〜endIndexの手前まで）
public String substring(int beginIndex, int endIndex)
```

- `beginIndex`：開始インデックス（**含む**）
- `endIndex`：（2引数版のみ）終了インデックス（**含まない**）

---

# 🔹 使用例（基本）

```java
String s = "JavaWorld";

// 1引数版
System.out.println(s.substring(4));        // → "World"

// 2引数版
System.out.println(s.substring(0, 4));      // → "Java"
System.out.println(s.substring(4, 8));      // → "Worl"
```

✅ **beginIndexは含む、endIndexは含まない**に注意！

---

# 🔸 substring() のルールと注意点

| ルール内容 | 説明 |
| --- | --- |
| `beginIndex` は0以上 | 0から数える（配列・charAt()と同じ感覚） |
| `endIndex` は最大で `length()` | 文字列の長さと同じでもOK（超えると例外） |
| `[beginIndex, endIndex)`形式 | beginは含み、endは含まない（閉開区間） |
| `beginIndex == endIndex` | 空文字列 `""` を返す |
| `beginIndex > endIndex` | `StringIndexOutOfBoundsException` 発生 |
| マイナス指定はNG | `beginIndex < 0` または `endIndex < 0` なら例外発生 |

---

# 🔹 よくある実務での用途

- **特定部分の取り出し**（たとえば、年月日を「年」「月」「日」に分ける）
- **ファイルパスからファイル名だけを抽出**
- **文字列の先頭・末尾をカットする**
- **トークン処理や手作業によるパース**

例：

```java
String path = "/usr/local/bin/file.txt";
String filename = path.substring(path.lastIndexOf('/') + 1);
System.out.println(filename); // → "file.txt"
```

---

# 🔸 substring() とサロゲートペアの注意

- `substring()` は**char配列インデックス**単位で動くため、
    
    **サロゲートペアの真ん中を指定すると壊れる**可能性があります。
    
- Unicode完全対応が必要なら、`codePoints()`ベースの処理を検討！

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 戻り値 | `String`型（元の文字列の一部） |
| 範囲 | `[beginIndex, endIndex)`（endIndexは含まない） |
| エラー | 範囲外なら `StringIndexOutOfBoundsException` 発生 |
| サロゲートペア対応 | char単位のため、注意が必要 |

---

# ✨ 超シンプルまとめ

> substringは「開始は含み、終了は含まない」区間を抜き出す
> 

✅ これさえ意識すれば、ほぼ間違えません！

---

# 🔺 `String.substring()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：範囲指定（基本）

```java
String s = "HelloWorld";
System.out.println(s.substring(0, 5));
```

この出力は？

A. `"Hello"`

B. `"HelloW"`

C. `"H"`

D. 実行時例外

**答え**：A. `"Hello"`

**解説**：

- `beginIndex=0`（含む）、`endIndex=5`（含まない）
- ⇒ インデックス0～4 ('H', 'e', 'l', 'l', 'o') を取り出す！

---

### ❓ 問題 2：空文字列を返すパターン

```java
String s = "Sample";
System.out.println(s.substring(3, 3));
```

この出力は？

A. `"p"`

B. `"ple"`

C. `""`（空文字列）

D. 実行時例外

**答え**：C. `""`（空文字列）

**解説**：

- `beginIndex` と `endIndex` が同じなら、**空文字列を返す**仕様です！
- ✅ 例外にはなりません。

---

### ❓ 問題 3：範囲外アクセス

```java
String s = "Java";
System.out.println(s.substring(2, 5));
```

このコードの結果は？

A. `"va"`

B. `"v"`

C. `"vaJ"`

D. 実行時例外

**答え**：D. 実行時例外

**解説**：

- `endIndex=5` は `length()=4` を超えている！
- ✅ `StringIndexOutOfBoundsException` が発生します。

---

### ❓ 問題 4：beginIndex > endIndex

```java
String s = "OpenAI";
System.out.println(s.substring(4, 2));
```

このコードの結果は？

A. `"en"`

B. `"p"`

C. 空文字列 `""`

D. 実行時例外

**答え**：D. 実行時例外

**解説**：

- `beginIndex > endIndex` はエラー！
- ✅ `StringIndexOutOfBoundsException` が発生します。

---

### ❓ 問題 5：サロゲートペアを跨ぐケース

```java
String emoji = "A𩸽B"; // 'A' + '𩸽'(サロゲートペア) + 'B'
System.out.println(emoji.substring(1, 2));
```

この出力は？

A. `"𩸽"`

B. 上位サロゲートのみの断片文字

C. `"B"`

D. 実行時例外

**答え**：B. 上位サロゲートのみの断片文字

**解説**：

- `substring(1,2)`はcharインデックス単位で動作！
- サロゲートペアの上位サロゲートだけを取り出すので、**壊れた文字**（不正な表示）になります。
- ✅ サロゲートペアの真ん中を切ると、見た目や意味がおかしくなる！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| beginIndexとendIndexが同じなら例外？ | ❌ 空文字列 `""` を返すだけ。例外は発生しない。 |
| endIndexは含まれる？ | ❌ 含まない（閉開区間 `[begin, end)`） |
| endIndexはlengthより大きくてもOK？ | ❌ 超えると即例外 `StringIndexOutOfBoundsException` |
| サロゲートペアの真ん中を切っても問題ない？ | ❌ 上位サロゲートだけ取り出すと壊れた文字になる |

---

# 🔺 `String.substring()` 応用問題集（前方一致・後方一致・部分切り出し）

---

## 【前方一致編】

---

### ❓ 問題 1：前方5文字を取得

```java
String s = "ProgrammingLanguage";
System.out.println(s.substring(0, 5));
```

この出力は？

A. `"Progr"`

B. `"Progra"`

C. `"Progrm"`

D. 実行時例外

**答え**：A. `"Progr"`

**解説**：

- `beginIndex=0`、`endIndex=5`
- インデックス0,1,2,3,4（5文字）→ `"Progr"`

---

### ❓ 問題 2：指定文字列の先頭が "Pro" か判定

```java
String s = "Programming";
boolean result = s.substring(0, 3).equals("Pro");
System.out.println(result);
```

この結果は？

A. `true`

B. `false`

C. コンパイルエラー

D. 実行時例外

**答え**：A. `true`

**解説**：

- `substring(0,3)` → `"Pro"`
- `"Pro".equals("Pro")` → `true`

✅ 前方一致の判定に substring を使う方法！

---

## 【後方一致編】

---

### ❓ 問題 3：後ろ3文字を取得

```java
String s = "Developer";
System.out.println(s.substring(s.length() - 3));
```

この出力は？

A. `"per"`

B. `"ope"`

C. `"e"`

D. 実行時例外

**答え**：A. `"per"`

**解説**：

- `s.length()=9`
- `substring(6)` → インデックス6,7,8 → `"per"`

✅ 末尾文字列を取得するときは **`length() - n`** を使う！

---

### ❓ 問題 4：末尾が "per" か判定

```java
String s = "Developer";
boolean result = s.substring(s.length() - 3).equals("per");
System.out.println(result);
```

この結果は？

A. `true`

B. `false`

C. コンパイルエラー

D. 実行時例外

**答え**：A. `true`

**解説**：

- `substring(length()-3)` → `"per"`
- 比較対象 `"per"` と一致！

✅ 後方一致判定にも substring は使える！

---

## 【部分切り出し編】

---

### ❓ 問題 5：特定範囲の部分抽出

```java
String s = "abcdefghijklmnopqrstuvwxyz";
System.out.println(s.substring(5, 10));
```

この出力は？

A. `"efghi"`

B. `"fghij"`

C. `"eghij"`

D. 実行時例外

**答え**：A. `"fghij"`

**解説**：

- インデックス5→'f'からインデックス9→'j'まで！
- `[5, 10)` → 'f', 'g', 'h', 'i', 'j'

---

### ❓ 問題 6：文字列からドメイン名だけ抜き出す

```java
String email = "user@example.com";
System.out.println(email.substring(email.indexOf('@') + 1));
```

この出力は？

A. `"example.com"`

B. `"@example.com"`

C. `"ser@example.com"`

D. 実行時例外

**答え**：A. `"example.com"`

**解説**：

- `indexOf('@')` → @の位置
- `+1` することで@の**次の位置**からsubstringする！

✅ 「特定文字の後ろから抽出」も substringの得意技！

---

# ✅ まとめ：substring 応用パターン

| 応用パターン | 使い方のポイント |
| --- | --- |
| 前方一致判定 | `substring(0, 長さ).equals(比較対象)` |
| 後方一致判定 | `substring(length()-長さ).equals(比較対象)` |
| 末尾取得 | `substring(length()-n)` |
| 中間部分取り出し | `substring(begin, end)` |
| 特定文字の後ろから取り出し | `substring(indexOf(文字)+1)` |