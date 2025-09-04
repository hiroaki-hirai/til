# codePointAt()

`String.codePointAt()` は Java において、

**指定した位置にある「Unicodeコードポイント（int型）」を取得するメソッド**です。

つまり、`charAt()` のように単純な「char（16bit）」を返すのではなく、

**サロゲートペアも正しくまとめて1文字（1コードポイント）として扱う** ために使われます！

---

# ✅ `String.codePointAt()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `codePointAt(int index)` |
| 戻り値の型 | `int`（Unicodeコードポイント） |
| 主な用途 | 指定インデックス位置のコードポイント取得 |

---

# 🔹 シグネチャ（定義）

```java
public int codePointAt(int index)
```

- `index`：対象とする文字位置（**char単位のインデックス**）
- 戻り値：指定位置にあるコードポイント（`int型`）

---

# 🔹 使用例（基本）

```java
String s = "Hello";
System.out.println(s.codePointAt(1)); // 'e' のコードポイント → 101
```

- `'H'` → Unicode U+0048 → 72
- `'e'` → Unicode U+0065 → 101

✅ 通常の文字（BMP内）なら、`charAt()` と大差なし。

---

# 🔸 ここが重要！サロゲートペア対応

例えば、絵文字などの**サロゲートペア**を含む文字列では違いが出ます！

```java
String emoji = "𩸽"; // U+29E3D（サロゲートペア）
System.out.println(emoji.length());       // → 2（char数）
System.out.println(emoji.codePointAt(0)); // → 171069（U+29E3D）
```

✅ `codePointAt(0)` は、

上位サロゲート＋下位サロゲートを**まとめて1つのコードポイントとして正しく取得**します！

---

# 🔸 charAt() との違いまとめ

| 項目 | `charAt()` | `codePointAt()` |
| --- | --- | --- |
| 戻り値の型 | `char`（16bit） | `int`（32bit相当） |
| サロゲートペアの対応 | ❌ 分割される（片方だけ） | ✅ 正しくまとめて取得 |
| BMP内の通常文字 | ほぼ同じ（差はない） | 同じ意味合い |
| BMP外（絵文字など） | 片側しか取れない | 正しい1コードポイントを取れる |

---

# 🔹 特殊な挙動（注意点）

| ケース | 挙動 |
| --- | --- |
| `index` が範囲外 | `StringIndexOutOfBoundsException` 発生 |
| サロゲートペアの真ん中(index指定ミス) | 上位または下位サロゲートのみの結果になる |

※絵文字の後ろのindexをうっかり指定すると、下位サロゲートだけ拾うので注意！

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| サロゲートペア対応 | ✅ 上位＋下位サロゲートをまとめた「真の1文字」を取得できる |
| 戻り値 | `int`（Unicodeコードポイント値） |
| エラー条件 | 範囲外アクセスで例外 (`StringIndexOutOfBoundsException`) |
| 通常文字（ASCIIなど） | `charAt()`とほぼ同じ結果 |

---

# ✨ 超シンプルまとめ

> charAt() は「UTF-16の1パーツ」を取り出す
> 
> 
> **codePointAt() は「人間が見る1文字（コードポイント）」を取り出す**
> 

✅ こう覚えるとばっちりです！

---

# 🔺 `String.codePointAt()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：通常文字列の場合

String s = "Java";
System.out.println(s.codePointAt(1));

このコードの出力は？

A. `'a'`

B. `a`

C. `97`

D. コンパイルエラー

**答え**：C. `97`

**解説**：

- `'a'` のUnicodeコードポイントは `97`
- `codePointAt()` は **int型のコードポイント値** を返します。
- `'a'` や `"a"` ではなく、数値 `97`！

---

### ❓ 問題 2：戻り値の型

```java
String s = "Hello";
var cp = s.codePointAt(0);
System.out.println(((Object)cp).getClass().getSimpleName());
```

この出力は？

A. `Character`

B. `String`

C. `Integer`

D. `Char`

**答え**：C. `Integer`

**解説**：

- `codePointAt()` の戻り値は **プリミティブint**。
- `Object`にキャストするとボクシングされて、`Integer`型になります。

---

### ❓ 問題 3：サロゲートペア文字列の場合

```java
String emoji = "𩸽"; // サロゲートペア
System.out.println(emoji.codePointAt(0));
```

この出力に最も近いものは？

A. 1

B. 171069

C. 55362

D. 実行時例外

**答え**：B. 171069

**解説**：

- `"𩸽"` は U+29E3D（16進数）= 171069（10進数）
- `codePointAt(0)` は **上位＋下位サロゲートを正しくまとめて** 1つのコードポイントを返します。

---

### ❓ 問題 4：範囲外アクセス

このコードの結果は？

A. `'i'`

B. 105

C. 0

D. 実行時例外

**答え**：D. 実行時例外

**解説**：

- `"Hi"` の有効インデックスは 0, 1
- `index=2` は範囲外アクセス！
    
    → 実行時に `StringIndexOutOfBoundsException` が発生します。
    

---

### ❓ 問題 5：サロゲートペアの下位側インデックスを指定したら？

```java
String emoji = "𩸽"; // サロゲートペア
System.out.println(emoji.codePointAt(1));
```

この出力は？

A. サロゲートペア全体の値

B. 上位サロゲートだけの値

C. 下位サロゲートだけの値

D. 実行時例外

**答え**：C. 下位サロゲートだけの値

**解説**：

- `codePointAt(index)` は、**指定したインデックスが上位サロゲートの場合に限り**
    
    上位＋下位を合わせた正しいコードポイントを返します。
    
- **下位サロゲート単体**を指定すると、そのchar（16bit）だけを読み取ってしまい、正しくないコードポイントになります。

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `codePointAt()` は常に1文字？ | ❌ サロゲートペアは0番目だけ正しく取得できる |
| `codePointAt()` は `char` を返す？ | ❌ `int`型のコードポイント（32bit相当）を返す |
| 範囲外でもnullが返る？ | ❌ 即 `StringIndexOutOfBoundsException` が発生 |
| 下位サロゲートでも正しく取得できる？ | ❌ 上位サロゲート位置だけが正しく1コードポイントになる |