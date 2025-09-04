# lastIndexOf()

`String.lastIndexOf()` は、

**「文字列中で、指定した文字や部分文字列が最後に現れる位置（インデックス）を返すメソッド」**です。

`indexOf()` が「最初に出現する場所」なのに対して、

`lastIndexOf()` は**「最後に出現する場所」**を探します！

---

# ✅ `String.lastIndexOf()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `lastIndexOf()` |
| 戻り値の型 | `int`（インデックス番号、見つからなければ-1） |
| 主な用途 | 後ろから検索する（最後の出現位置を探す） |

---

# 🔹 シグネチャ（定義）

```java
// ① 文字 (char) を後ろから探す
public int lastIndexOf(int ch)

// ② 部分文字列 (String) を後ろから探す
public int lastIndexOf(String str)

// ③ 開始位置 (fromIndex) を指定して、文字を後ろから探す
public int lastIndexOf(int ch, int fromIndex)

// ④ 開始位置 (fromIndex) を指定して、部分文字列を後ろから探す
public int lastIndexOf(String str, int fromIndex)
```

---

# 🔹 使用例（基本）

```java
String s = "abracadabra";

System.out.println(s.lastIndexOf('a'));     // → 10
System.out.println(s.lastIndexOf("cad"));   // → 4
System.out.println(s.lastIndexOf('a', 5));  // → 5
System.out.println(s.lastIndexOf('z'));     // → -1k
```

✅ **インデックスは左から0スタート**。

✅ 最後に出現する場所のインデックスを返す。

✅ 見つからなければ `-1` を返す（例外にはならない！）。

---

# 🔸 indexOf()との違いまとめ

| 項目 | indexOf() | lastIndexOf() |
| --- | --- | --- |
| 探索方向 | 左から右へ | 右から左へ |
| 最初に見つけたものを返すか？ | はい | いいえ、最後に見つけたものを返す |
| fromIndex指定の効果 | 指定位置以降（右向き）を探す | 指定位置から（左向き）を探す |

---

# 🔹 よくある用途例

- **ファイルパスからファイル名を抽出する**
    
    → 最後のスラッシュ `/` の位置を探す
    
- **拡張子を取り出す**
    
    → 最後のピリオド `.` の位置を探す
    
- **区切り文字の最後の出現箇所を検出**

例：

```java
String path = "/usr/local/bin/file.txt";
int pos = path.lastIndexOf('/');
String filename = path.substring(pos + 1);
System.out.println(filename); // → "file.txt"
```

✅ **ファイル名・拡張子処理で超よく使われます！**

---

# 🔹 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 大文字小文字の扱い | ✅ 区別する（完全一致が必要） |
| 見つからなかった場合 | ✅ -1を返す（例外にはならない） |
| fromIndexに注意 | ✅ 指定位置から左へ向かって検索する |
| nullを渡すとどうなるか？ | ❌ NullPointerExceptionが発生する（strがnullのとき） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 戻り値の型 | `int`（見つからなければ-1） |
| 探索方向 | 後ろ（右→左）へ |
| 文字と文字列どちらも指定可能 | `char`でも`String`でも探せる |
| 大文字小文字の区別あり | 完全一致のみ許容 |
| 例外発生可能性 | nullを渡すと例外が発生する |

---

# ✨ 超シンプルまとめ

> lastIndexOf()は「最後に出現する場所」を探す！
fromIndexでスタート位置を変えられる！
見つからないなら-1を返すだけ！
> 

✅ これさえ覚えておけば完璧です！

---

# 🔺 `String.lastIndexOf()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：基本の後方検索

```java
String s = "abracadabra";
System.out.println(s.lastIndexOf('a'));
```

この出力は？

A. `5`

B. `10`

C. `0`

D. `-1`

**答え**：B. `10`

**解説**：

- 最後の'a'はインデックス10にあるので、**10**を返す！

✅ "最後"に出現した場所を返すことに注意！

---

### ❓ 問題 2：部分文字列検索

```java
String s = "JavaProgrammingJava";
System.out.println(s.lastIndexOf("Java"));
```

この出力は？

A. `0`

B. `14`

C. `4`

D. `-1`

**答え**：B. `14`

**解説**：

- 先頭にも"Java"があるが、**後ろの"Java"**（インデックス14）を返す！

✅ 部分文字列も「最後の出現場所」を探す！

---

### ❓ 問題 3：fromIndex指定の注意

```java
String s = "hellohello";
System.out.println(s.lastIndexOf('l', 7));
```

この出力は？

A. `3`

B. `2`

C. `7`

D. `8`

**答え**：C. `7`

**解説**：

- fromIndex=7（インデックス7は'l'）。
- そこから左に向かって探すので、**7番目の'l'** を見つける！

✅ fromIndex含めて逆向きに検索！

---

### ❓ 問題 4：大文字小文字違い

```java
String s = "HELLOhello";

System.out.println(s.lastIndexOf("Hello"));
```

この出力は？

A. `0`

B. `5`

C. `-1`

D. `5`または`0`ランダム

**答え**：C. `-1`

**解説**：

- 大文字小文字を区別する！
- "Hello"（大文字小文字混在）は含まれていないので、**1**。

✅ 完全一致のみ！

---

### ❓ 問題 5：見つからなかったときの扱い

```java
String s = "programming";
System.out.println(s.lastIndexOf('z'));
```

この出力は？

A. `0`

B. `-1`

C. `実行時例外`

D. `コンパイルエラー`

**答え**：B. `-1`

**解説**：

- 'z'は存在しないので、**1**を返す！

✅ **見つからないだけなら例外にはならない**（ここ重要！）。

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 最初に見つかったものを返す？ | ❌ 最後に見つかったものを返す |
| 大文字小文字は無視される？ | ❌ 完全一致が必要（大小区別あり） |
| 見つからなければ例外が発生する？ | ❌ 発生しない。単に-1を返すだけ |
| fromIndexは右向き探索？ | ❌ fromIndexから左向きに探索 |

---

# ✨ 超まとめ

> lastIndexOf()は「最後に出現する場所」を探す！
大小区別あり、見つからなければ-1、fromIndexは左向き探索！
> 

✅ この感覚を押さえておけば、試験でも実務でも安心です！

---

# 🔺 `lastIndexOf()`を使ったファイルパス/拡張子抽出演習セット

---

## 【ファイル名抽出演習】

---

### ❓ 問題 1：パスからファイル名を抽出

```java
String path = "/usr/local/bin/file.txt";

int pos = path.lastIndexOf('/');
String filename = path.substring(pos + 1);

System.out.println(filename);
```

この出力は？

A. `/file.txt`

B. `bin/file.txt`

C. `file.txt`

D. `local/bin/file.txt`

**答え**：C. `file.txt`

**解説**：

- 最後の`/`の位置（`/bin/`の直後）から切り出している！

✅ **lastIndexOf('/') + 1**でファイル名の開始位置を決める！

---

### ❓ 問題 2：拡張子を取り出す

```java
String path = "/usr/local/bin/file.txt";

int pos = path.lastIndexOf('.');
String ext = path.substring(pos + 1);

System.out.println(ext);
```

この出力は？

A. `.txt`

B. `txt`

C. `file.txt`

D. `/usr/local/bin/file.txt`

**答え**：B. `txt`

**解説**：

- 最後の`.`の位置から、その次（+1）をスタートして切り出している！

✅ 拡張子だけ取り出したいなら**lastIndexOf('.') + 1からsubstring**！

---

## 【複合演習】

---

### ❓ 問題 3：パスから拡張子なしのファイル名だけを取り出す

```java
String path = "/usr/local/bin/file.txt";

int start = path.lastIndexOf('/') + 1;
int end = path.lastIndexOf('.');

String name = path.substring(start, end);

System.out.println(name);
```

この出力は？

A. `file.txt`

B. `file`

C. `.txt`

D. `/usr/local/bin`

**答え**：B. `file`

**解説**：

- `/`と`.`の間をsubstringで抜き出している！

✅ 「拡張子なしファイル名」は**スラッシュ後～ピリオド直前**！

---

## 【境界ミス防止演習】

---

### ❓ 問題 4：ピリオドがないファイルの場合（例外発生するか？）

```java
String path = "/usr/local/bin/file";

int pos = path.lastIndexOf('.');
System.out.println(pos);
```

この出力は？

A. `-1`

B. `0`

C. `ファイル名の長さ-1`

D. 実行時例外

**答え**：A. `-1`

**解説**：

- ピリオドが存在しない場合、`lastIndexOf('.')`は**-1**を返す！

✅ **見つからなければ-1を返すだけ！例外にはならない！**

---

### ❓ 問題 5：拡張子を安全に取り出す方法（null防止策）

```java
String path = "/usr/local/bin/file";

int pos = path.lastIndexOf('.');
String ext = (pos != -1) ? path.substring(pos + 1) : "";

System.out.println(ext);
```

この出力は？

A. 空文字 `""`

B. `file`

C. `bin`

D. 実行時例外

**答え**：A. 空文字 `""`

**解説**：

- `pos==-1`（ピリオドなし）なので、空文字を返す！

✅ **存在チェック（pos!=-1）を入れるのが実務では鉄則**！

---

## 【URL応用編】

---

### ❓ 問題 6：URLのファイル名だけ取り出す

```java
String url = "https://example.com/files/report2025.pdf";

int pos = url.lastIndexOf('/');
String fileName = url.substring(pos + 1);

System.out.println(fileName);
```

この出力は？

A. `files/report2025.pdf`

B. `report2025.pdf`

C. `example.com`

D. `https`

**答え**：B. `report2025.pdf`

**解説**：

- 最後のスラッシュ `/`の直後をsubstringしているので、ファイル名だけ取れる！

✅ ファイルダウンロード処理などでよく使う！

---

# ✅ 応用まとめ

| パターン | ポイント |
| --- | --- |
| ファイル名だけ取り出す | `lastIndexOf('/') + 1 から substring` |
| 拡張子だけ取り出す | `lastIndexOf('.') + 1 から substring` |
| ファイル名（拡張子なし）取り出す | `lastIndexOf('/') + 1 ～ lastIndexOf('.') まで substring` |
| ピリオドがなければ-1を返す | 必ず-1チェックして安全に処理 |

---

# ✨ 超シンプルまとめ

> ファイル名も拡張子も「最後の/」や「最後の.」の位置を探してsubstringするだけ！
lastIndexOf()＋substringの黄金パターンを使いこなそう！
> 

✅ これを押さえれば、実務でも試験でも大活躍できます！