# compareToIgnoreCase()

---

# ✅ `String.compareToIgnoreCase()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `compareToIgnoreCase(String str)` |
| 戻り値の型 | `int`（比較結果を示す整数） |
| 主な用途 | **文字列同士を「大文字・小文字を無視して」辞書順で比較する** |

---

# 🔹 シグネチャ（定義）

```java
public int compareToIgnoreCase(String str)
```

- **引数**：比較対象となる別の文字列
- **戻り値**：
    - **0**：両者が（大文字・小文字を無視して）一致
    - **負の値**：呼び出し元（this）が引数より「辞書順で前」
    - **正の値**：呼び出し元（this）が引数より「辞書順で後」

---

# 🔹 使用例（基本）

```java
String a = "apple";
String b = "Apple";

System.out.println(a.compareTo(b));           // 正の値
System.out.println(a.compareToIgnoreCase(b)); // 0
```

✅ **compareToIgnoreCase()は大文字小文字を無視**して比較するので、"apple" と "Apple" は**一致**とみなされます！

---

# 🔹 具体的な比較ルール

- 比較時に、両方の文字列を**仮想的に小文字（または大文字）に揃えて**比較する。
- 実際に文字列を変換するわけではなく、内部的に比較処理だけが行われる。
- 文字列の長さや順序は、通常の`compareTo()`と同じルールに従う。
- 途中まで一致していれば、残りの文字で比較される。

---

# 🔹 `compareTo()`との違いまとめ

| 比較項目 | compareTo() | compareToIgnoreCase() |
| --- | --- | --- |
| 大文字小文字区別するか？ | ✅ 区別する | ❌ 区別しない |
| "apple" vs "Apple" | 正の値（違うとみなす） | 0（同じとみなす） |
| 用途 | 厳密な辞書順ソート、完全一致比較 | 緩やかな比較（ユーザー入力比較など） |

---

# 🔹 よくある実務パターン

| シチュエーション | 使い方例 |
| --- | --- |
| ユーザーが入力した文字列の比較（大文字小文字問わず） | `"admin".compareToIgnoreCase(input)`で管理者認証などに使う |
| アルファベット順の一覧作成（大文字小文字混在対応） | コレクションソート時にカスタムComparatorで使うことがある |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 大文字小文字無視するか | ✅ 無視する |
| 0の意味 | （大文字小文字無視して）一致 |
| 負 or 正の値 | （無視した上で）辞書順で前後を示す |
| nullに対する動作 | 呼び出し元または引数がnullなら**NullPointerException**が発生する |

---

# ✨ 超シンプルまとめ

> compareToIgnoreCase()は「大小無視して文字列比較」する！
0なら一致、負なら前、正なら後！
ユーザー入力比較や緩やかな辞書順比較に向いている！
> 

✅ この感覚を押さえておけば、試験でも実務でもバッチリ使えます！

以下に、**「compareToIgnoreCase()専用 引っかけ問題集（大文字・小文字・長さ違いトリック対応・全8問）」** をご用意しました！

今回は、

**「大文字小文字の違いを無視できているか？」「途中一致＋長さ違いトリックを見抜けるか？」**

を意識して問題を作っています！

---

# 🔺 `compareToIgnoreCase()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：基本的な大文字小文字無視

```java
String a = "apple";
String b = "Apple";

System.out.println(a.compareToIgnoreCase(b));
```

### ✅ 問題 1の答え

**答え**：A. 0

**解説**：

- 大文字小文字を無視して完全一致とみなす！

---

### ❓ 問題 2：完全一致（小文字同士）

```java
String a = "hello";
String b = "hello";

System.out.println(a.compareToIgnoreCase(b));
```

### ✅ 問題 2の答え

**答え**：A. 0

**解説**：

- 当然ながら、小文字同士も一致！

---

### ❓ 問題 3：途中まで一致して最後が違う（大文字小文字無視）

```java
String a = "abcX";
String b = "abcy";

System.out.println(a.compareToIgnoreCase(b));

```

### ✅ 問題 3の答え

**答え**：C. 負の値

**解説**：

- "X"と"y"を比較 → 'x'は'y'よりUnicode値が小さいので負の値！

---

### ❓ 問題 4：短い方と長い方比較（完全一致）

```java
String a = "test";
String b = "Testcase";

System.out.println(a.compareToIgnoreCase(b));
```

### ✅ 問題 4の答え

**答え**：C. 負の値

**解説**：

- "test"は"testcase"の途中まで一致だが、長さで負になる！

---

### ❓ 問題 5：短い方と長い方比較（長さ優先判定）

```java
String a = "data";
String b = "database";

System.out.println(a.compareToIgnoreCase(b));

```

### ✅ 問題 5の答え

**答え**：C. 負の値

**解説**：

- 同じく短いほう（"data"）が前と判定される！

---

### ❓ 問題 6：1文字違い（大小無視）

```java
String a = "Admin";
String b = "ADMIT";

System.out.println(a.compareToIgnoreCase(b));
```

### ✅ 問題 6の答え

**答え**：C. 負の値

**解説**：

- "Admin"と"ADMIT"は途中"M"と"I"が違う → 'm'＜'i'なので負！

---

### ❓ 問題 7：先頭文字違い

```java
String a = "Banana";
String b = "apple";

System.out.println(a.compareToIgnoreCase(b));

```

### ✅ 問題 7の答え

**答え**：B. 正の値

**解説**：

- 'B'（66）と'a'（97）では 'B'が前だが、compareToIgnoreCaseでは小文字に揃えて比較 → 'b'（98）と'a'（97）なので'b'の方が大きい！

---

### ❓ 問題 8：nullとの比較

```java
String a = null;
String b = "sample";

System.out.println(a.compareToIgnoreCase(b));

```

### ✅ 問題 8の答え

**答え**：D. 実行時例外

**解説**：

- null対象にメソッドを呼び出すとNullPointerException！

---

# ✅ compareToIgnoreCase()引っかけ注意ポイントまとめ

| 観点 | 内容 |
| --- | --- |
| 大文字小文字は？ | ❌ 無視する |
| 長さ違いは？ | ✅ 違いが出たら長さで判定 |
| 途中一致でも最後まで比較するか？ | ✅ 最後の文字や長さでちゃんと比較する |
| nullには安全か？ | ❌ 安全ではない（NPE発生） |

---

# ✨ 超シンプルまとめ

> compareToIgnoreCase()は大小無視で比較！
途中一致でも長さや最後の文字で決まる！
nullには注意！（NPE対策必須）
> 

✅ この感覚を押さえれば、試験も実務でもバッチリ使いこなせます！