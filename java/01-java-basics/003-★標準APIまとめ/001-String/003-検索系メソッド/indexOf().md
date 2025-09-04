# indexOf()

`String.indexOf()` は Java において、

**指定した文字や部分文字列が、対象文字列の中で最初に出現する位置（インデックス）を返すメソッド**です。

とても頻出かつ、**substring()や文字列検索処理と組み合わせる**ために、

試験・実務の両方で非常に重要なメソッドです！

---

# ✅ `String.indexOf()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `indexOf` |
| 戻り値の型 | `int`（見つかった位置、または-1） |
| 主な用途 | 文字列の中から「最初の位置」を検索する |

---

# 🔹 シグネチャ（定義）

```java
// ① 文字(char)を探す
public int indexOf(int ch)
// ② 部分文字列(String)を探す
public int indexOf(String str)
// ③ 開始位置を指定して文字(char)を探す
public int indexOf(int ch, int fromIndex)
// ④ 開始位置を指定して部分文字列(String)を探す
public int indexOf(String str, int fromIndex)
```

✅ 見つからなかったら **-1** を返す！これ重要！

---

# 🔸 注意点まとめ

| 項目 | 内容 |
| --- | --- |
| 文字が見つかった場合 | 最初に一致したインデックスを返す |
| 見つからなかった場合 | `-1` を返す（例外は出ない） |
| fromIndex指定 | 指定した位置以降から検索する |
| 大文字小文字は区別するか？ | ✅ 完全一致（`"a"` と `"A"`は違う） |

---

# 🔹 よくある用途例

- **特定の文字以降の切り出し**
- **ファイルパスからファイル名だけ抽出**
- **CSVデータのカンマ位置を見つける**
- **検索ボックスのヒット判定**

例：

```java
String path = "/usr/local/bin/file.txt";
int pos = path.lastIndexOf('/');
String filename = path.substring(pos + 1);
System.out.println(filename); // → "file.txt"
```

---

# 🔸 indexOf() と lastIndexOf() の違い

| メソッド名 | 何をするか |
| --- | --- |
| `indexOf()` | 最初に出現する位置を探す（左から右へ） |
| `lastIndexOf()` | 最後に出現する位置を探す（右から左へ） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 戻り値 | 最初にマッチしたインデックス（見つからなければ-1） |
| 大文字小文字 | 区別する（完全一致のみ） |
| 例外発生の可能性 | ❌ ない（見つからないときは-1で正常動作） |
| fromIndex指定可能 | ✅ できる（指定位置以降を検索） |

---

# ✨ 超シンプルまとめ

> indexOfは「見つかったらインデックスを返す、見つからなければ-1」
> 

✅ この感覚があれば間違えません！

---

# 🔺 `String.indexOf()` 引っかけ問題集（全5問）

---

## 【基本ミスパターン】

---

### ❓ 問題 1：単純な文字検索

```java
String s = "Programming";
System.out.println(s.indexOf('g'));
```

この出力は？

A. `3`

B. `5`

C. `6`

D. `8`

**答え**：A. `3`

**解説**：

- "Programming" → 'g' が最初に出現するのはインデックス3（0から数えて4文字目）。

✅ 最初の一致のみ返すことに注意！

---

## 【大文字小文字区別ミス】

---

### ❓ 問題 2：大文字小文字を間違える

```java
String s = "JavaProgramming";
System.out.println(s.indexOf('P'));
```

この出力は？

A. `-1`

B. `4`

C. `5`

D. `実行時例外`

**答え**：B. `4`

**解説**：

- 'P'は大文字でインデックス4に存在する。
- ✅ Javaの`indexOf()`は**大文字小文字を区別する**！

---

## 【部分文字列 vs 単一文字混同ミス】

---

### ❓ 問題 3：部分文字列の検索

```java
String s = "abracadabra";
System.out.println(s.indexOf("cad"));
```

この出力は？

A. `3`

B. `4`

C. `5`

D. `-1`

**答え**：B. `4`

**解説**：

- 'c'がインデックス4、そこから"cad"が始まる。
- ✅ 部分文字列検索は**開始位置を返す**！

---

## 【fromIndex指定ミス】

---

### ❓ 問題 4：検索開始位置指定ミス

```java
String s = "hellohello";
System.out.println(s.indexOf('e', 5));
```

この出力は？

A. `1`

B. `6`

C. `7`

D. `-1`

**答え**：C. `7`

**解説**：

- `fromIndex=5` → 6文字目から右方向に検索。
- 6→'h', 7→'e' → インデックス7で発見！

✅ fromIndex以降の検索だと、最初にあった'e'(インデックス1)は無視される！

---

## 【見つからないときの取り扱いミス】

---

### ❓ 問題 5：見つからなかった場合

```java
String s = "ChatGPT";
System.out.println(s.indexOf('z'));
```

この出力は？

A. `0`

B. `-1`

C. `1`

D. 実行時例外

**答え**：B. `-1`

**解説**：

- 'z' は文字列中に存在しない。
- ✅ `indexOf()` は**見つからなければ-1を返す**！例外は発生しない！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 大文字と小文字は無視される？ | ❌ 完全一致（大文字小文字を区別する） |
| fromIndexを指定しても全体検索される？ | ❌ 指定位置以降のみ検索される |
| 見つからなかったら例外が発生する？ | ❌ 例外ではなく `-1` を返す |
| 部分文字列検索でも1文字ずつチェック？ | ❌ 部分文字列全体が一致した開始位置を返す |

---

# ✨ 超まとめ

> indexOfは「見つかったらインデックス、見つからなかったら-1、大文字小文字は厳密一致」！
> 

これを意識すれば完璧です！

---

# 🔺 `indexOf()`＋`substring()` 連携応用パターン問題集

---

## 【基本：1回だけ出現するパターン】

---

### ❓ 問題 1：ファイル名だけを取り出す

```java
String path = "/home/user/file.txt";
String fileName = path.substring(path.indexOf('/') + 1);
System.out.println(fileName);
```

この出力は？

A. `"home/user/file.txt"`

B. `"user/file.txt"`

C. `"file.txt"`

D. 実行時例外

**答え**：A. `"home/user/file.txt"`

**解説**：

- `indexOf('/')` → 最初の `/` はインデックス0
- `+1` → インデックス1以降をsubstring
    
    → `"home/user/file.txt"`
    

---

## 【応用：最後のスラッシュ以降を取りたい】

---

### ❓ 問題 2：最後のスラッシュの後ろを取得する

```java
String path = "/home/user/file.txt";
String fileName = path.substring(path.lastIndexOf('/') + 1);
System.out.println(fileName);
```

この出力は？

A. `"home/user/file.txt"`

B. `"file.txt"`

C. `"user/file.txt"`

D. 実行時例外

**答え**：B. `"file.txt"`

**解説**：

- `lastIndexOf('/')` → 最後の `/` のインデックス
- その次のインデックスからsubstring！

✅ `indexOf()` と `lastIndexOf()`の使い分けが大事！

---

## 【見つからない場合の処理ミス】

---

### ❓ 問題 3：区切り文字が存在しない場合

```java
String s = "filename";
int pos = s.indexOf('/');
String result = (pos != -1) ? s.substring(pos + 1) : s;
System.out.println(result);
```

この出力は？

A. 空文字列 `""`

B. `"filename"`

C. 実行時例外

D. `/`を含む文字列

**答え**：B. `"filename"`

**解説**：

- `/` が見つからない場合、`indexOf()` は `1` を返す。
- 三項演算子で安全に処理しているため、`substring`せずそのまま返している！

---

## 【範囲指定ミス】

---

### ❓ 問題 4：インデックス指定間違い

```java
String s = "abcdefg";
System.out.println(s.substring(s.indexOf('c'), s.indexOf('g')));
```

この出力は？

A. `"cdefg"`

B. `"cdef"`

C. `"cde"`

D. 実行時例外

**答え**：B. `"cdef"`

**解説**：

- `indexOf('c') = 2`
- `indexOf('g') = 6`
- substring(2,6) → インデックス2～5（'c','d','e','f')

✅ endIndexは「含まない」ことに注意！

---

## 【複数区切り文字の応用】

---

### ❓ 問題 5：カンマで区切られた最初の値を取り出す

```java
String csv = "apple,banana,orange";
System.out.println(csv.substring(0, csv.indexOf(',')));
```

この出力は？

A. `"apple"`

B. `"banana"`

C. `"orange"`

D. `"apple,"`

**答え**：A. `"apple"`

**解説**：

- `indexOf(',')` → 最初のカンマの位置
- substring(0, カンマの位置) → `"apple"`

---

## 【全体応用セット】

---

### ❓ 問題 6：メールアドレスからドメイン名だけを取り出す

```java
String email = "user@example.com";
String domain = email.substring(email.indexOf('@') + 1);
System.out.println(domain);
```

この出力は？

A. `"@example.com"`

B. `"user"`

C. `"example.com"`

D. `"@user"`

**答え**：C. `"example.com"`

**解説**：

- `indexOf('@')` で '@' の位置を探す
- `+1` して '@'を飛ばして後ろをsubstring！

---

# ✅ よくあるミスパターンまとめ

| ミス内容 | 正しい理解 |
| --- | --- |
| indexOf() が見つからなかった場合にsubstringする | ❌ すぐsubstringせず、-1チェックするべき |
| endIndexを含めてsubstringしようとする | ❌ endIndexは「含まない」 |
| lastIndexOf()を使うべきところでindexOf()を使う | ❌ 最後の区切り位置取得はlastIndexOf()が必要 |
| 区切り文字がない場合の例外を想定していない | ❌ indexOf()が-1の場合のガード処理を入れる |

public int indexOf(int ch)
indexOfメソッドのcharを探すシグニチャの場合、仮引数の型がchar型ではなくint型なのはなぜですか？

---

# ✅ 答え：

> Javaでは、charは内部的に数値（Unicodeコードポイント）だからです。
そして、char型の範囲（0～65535）を超えた文字（サロゲートペアなど）にも対応できるようにするため、
より広い型である int を使っているのです！
> 

---

# 🔵 詳しく整理して説明すると…

| 項目 | 内容 |
| --- | --- |
| Javaの`char`型 | 16bit符号なし整数（範囲：0〜65535） |
| Javaの`int`型 | 32bit符号あり整数（範囲：-2³¹〜2³¹-1） |
| Unicodeコードポイント | 0〜10FFFFまで存在する（つまり65536以上も存在する） |

---

## 🔹 なぜ `int ch` なのか？

- Javaでは**char型は16bit**しかないため、
    
    BMP（基本多言語面、U+0000～U+FFFF）内の文字しか直接表せません。
    
- でも、Unicodeの世界では**U+10000以上のコードポイント**（たとえば絵文字など）がたくさんあります！
- そのため、サロゲートペアを含むような**広いコードポイントを扱える**ように、
    
    `indexOf(int ch)` では**int型**で引数を受け取るように設計されているのです。
    

---

# 🔸 実際のイメージ

たとえば：

```java
String s = "𩸽"; // U+29E3D (サロゲートペア)

int codePoint = 0x29E3D; // 171069
System.out.println(s.indexOf(codePoint)); // 0
```

✅ indexOf(int ch) は「16bit以内の文字」だけでなく、

**32bitのコードポイント（Unicode拡張文字）も正しく扱える**

仕様になっています！