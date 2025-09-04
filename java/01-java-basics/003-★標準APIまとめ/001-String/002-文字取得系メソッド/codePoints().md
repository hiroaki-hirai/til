# codePoints()

`String.codePoints()` は Java において、

**文字列のすべての Unicode コードポイントを順番に流すための `IntStream` を返すメソッド**です。

つまり、**サロゲートペアも正しく1文字として扱いながら、全体を順に処理できる**強力なAPIです！

---

# ✅ `String.codePoints()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `codePoints()` |
| 戻り値の型 | `IntStream`（java.util.stream.IntStream） |
| 主な用途 | 文字列の各コードポイントをストリームで処理する |

---

# 🔹 シグネチャ（定義）

```java
public IntStream codePoints()
```

- 引数なし
- 戻り値は `IntStream`
    - 各要素は、文字列内のコードポイント（`int`）を表す

---

# 🔹 使用例（基本）

```java
String s = "Java";
s.codePoints()
 .forEach(cp -> System.out.println(cp));
```

出力：

```java
74   // 'J'
97   // 'a'
118  // 'v'
97   // 'a'
```

✅ 各文字の Unicode コードポイント（10進数）が順に出力されます！

---

# 🔹 サロゲートペア対応（重要）

たとえば、絵文字を含む文字列：

```java
String emoji = "A𩸽B"; // 'A' + '𩸽'（サロゲートペア） + 'B'

emoji.codePoints()
     .forEach(cp -> System.out.println(cp));
```

出力：

```java
65      // 'A'
171069  // '𩸽'（U+29E3D）
66      // 'B'
```

✅ サロゲートペアも1つのコードポイントとして正しく流れてくる！

---

# 🔹 よくある用途例

- **絵文字や特殊文字を正しく処理したいとき**
- **文字列から特定のコードポイントをフィルター/マッピングしたいとき**
- **Unicode範囲ごとの処理（絵文字だけ、漢字だけなど）**
- **文字列長（見た目の文字数）を数えたいとき**

---

# 🔹 よく使うパターン（応用）

### コードポイントを文字列に戻す

```java
String s = "A𩸽B";
String result = s.codePoints()
                 .mapToObj(cp -> new String(Character.toChars(cp)))
                 .collect(Collectors.joining());
System.out.println(result); // → "A𩸽B"
```

✅ コードポイント列をもとに再構成！

---

# 🔸 `String.codePoints()` vs `chars()`

| 比較項目 | `codePoints()` | `chars()` |
| --- | --- | --- |
| ストリームの型 | `IntStream`（コードポイント単位） | `IntStream`（char単位） |
| サロゲートペア対応 | ✅ 正しくまとめて処理する | ❌ 上下サロゲート別々に処理する |
| 用途 | Unicodeフル対応 | 単純なASCII・単一文字向け |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| サロゲートペア対応 | ✅（上位＋下位まとめて1コードポイント） |
| 戻り値 | `IntStream`（要素はint型） |
| 典型用途 | 絵文字対応、Unicodeベース文字処理 |
| `chars()`との違い | `chars()`はchar単位、`codePoints()`は文字単位 |

---

# ✨ 超シンプルまとめ

> codePoints() は「人間が見る意味での文字」を順番に流す
> 

✅ こう覚えれば間違えません！

---

# 🔺 `String.codePoints()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：戻り値の型

```java
String s = "Java";
var stream = s.codePoints();
System.out.println(stream.getClass().getSimpleName());
```

出力は？

A. `Stream<Integer>`

B. `IntStream`

C. `List<Integer>`

D. `Stream<String>`

**答え**：B. `IntStream`

**解説**：

✅ `codePoints()` の戻り値は **`IntStream`（プリミティブintベースのストリーム）**。

`Stream<Integer>` ではない！

---

### ❓ 問題 2：通常文字列の場合の各要素

```java
String s = "ABC";
s.codePoints().forEach(cp -> System.out.print(cp + " "));
```

出力に最も近いものは？

A. `65 66 67`

B. `A B C`

C. `1 2 3`

D. コンパイルエラー

**答え**：A. `65 66 67`

**解説**：

- 'A' = 65
- 'B' = 66
- 'C' = 67
    
    ✅ コードポイント（数値）として出力される！
    

---

### ❓ 問題 3：サロゲートペアを含む文字列

```java
String emoji = "A𩸽B"; // 'A' + '𩸽'(サロゲートペア) + 'B'
System.out.println(emoji.codePoints().count());
```

この出力は？

A. `3`

B. `4`

C. `5`

D. 実行時例外

**答え**：A. `3`

**解説**：

- 'A'（1）
- '𩸽'（サロゲートペアだけど1コードポイント）
- 'B'（1）

✅ 合計3コードポイント！

---

### ❓ 問題 4：codePoints()とchars()の違い

次のうち、**正しくサロゲートペアを1文字扱いする**メソッドはどれ？

A. `String.chars()`

B. `String.codePoints()`

C. `String.getBytes()`

D. `String.split("")`

**答え**：B. `String.codePoints()`

**解説**：

- `chars()` はUTF-16の**char単位**で流す → サロゲートペアが分離される
- `codePoints()` は**人間が見る文字（コードポイント単位）**で流す！

---

### ❓ 問題 5：Streamの各要素を文字列化する場合

```java
String s = "AB";
String result = s.codePoints()
                 .mapToObj(cp -> (char) cp)
                 .collect(Collectors.joining());
System.out.println(result);
```

このコードの結果は？

A. `"AB"`

B. `"6566"`

C. コンパイルエラー

D. 実行時例外

**答え**：C. コンパイルエラー

**解説**：

- `mapToObj(cp -> (char)cp)` は `char` を返すが、`Collectors.joining()` は**String型**のStreamを期待する。
- そのままだと型が合わずコンパイルエラー！

✅ 正しいのは：

```java
.mapToObj(cp -> new String(Character.toChars(cp)))
```

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `codePoints()` は Stream<Integer> を返す？ | ❌ IntStream（プリミティブintストリーム） |
| `codePoints()` は文字列で流す？ | ❌ int（数値）で流す |
| `chars()` もサロゲートペア対応？ | ❌ chars()はUTF-16のchar単位（サロゲート分離する） |
| サロゲートペアは2文字カウント？ | ❌ codePoints()なら正しく1文字カウントされる |