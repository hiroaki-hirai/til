# split()

---

# ✅ `String.split()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `split(String regex)` / `split(String regex, int limit)` |
| 戻り値の型 | `String[]`（分割後の文字列配列） |
| 主な用途 | **指定した区切り文字や正規表現パターンで文字列を分割する** |

---

# 🔹 シグネチャ（定義）

```java
public String[] split(String regex)
public String[] split(String regex, int limit)
```

- **regex**：
    - 分割する基準となる正規表現パターン。
- **limit**（省略可能）：
    - 配列の最大要素数を制御するオプション。
    - 特別な意味を持つ値もある（後述）。

---

# 🔹 使用例（基本）

```java
String s = "apple,banana,cherry";
String[] fruits = s.split(",");

System.out.println(Arrays.toString(fruits)); 
// → [apple, banana, cherry]
```

✅ **カンマ区切りで分割され、配列になる！**

---

# 🔹 正規表現に注意！

- `split()`の**第一引数は正規表現**です！
- 普通の文字でも使えるが、正規表現で特別な意味を持つ文字（例：`.`  `+` `?` `|` `(` `)` `[` `]` `{` `}` `\` `^` `$`）は**エスケープが必要**！

### 例：ピリオドで分割する場合

```java
String s = "www.example.com";
String[] parts = s.split("\\.");

System.out.println(Arrays.toString(parts));
// → [www, example, com]
```

✅ `split("\\.")` と **2重エスケープ**が必要！（Javaの文字列＋正規表現）

---

# 🔹 limitパラメータの意味

| limit値 | 意味 |
| --- | --- |
| 0 | 制限なし（末尾の空文字列はすべて削除される） |
| 正の整数 | 最大でその数の要素まで分割（超過部分はまとめて最後の要素に格納） |
| 負の整数 | 制限なし＋末尾の空文字列も保持する |

---

### 🔸 limitなし（または0）の例

```java
String s = "a,b,,c";
String[] result = s.split(",");

System.out.println(Arrays.toString(result));
// → [a, b, "", c]
```

（空文字列もちゃんと保持される）

---

### 🔸 limit指定ありの例（正数）

```java
String s = "a,b,c,d";
String[] result = s.split(",", 3);

System.out.println(Arrays.toString(result));
// → [a, b, c,d]
```

✅ 最後の要素に残りすべてが入る！

---

# 🔹 よくある実務パターン

| シチュエーション | 使用例 |
| --- | --- |
| CSVデータの1行をカンマで区切る | `split(",")` |
| パスやURLをスラッシュで区切る | `split("/")` |
| スペースまたはタブなど空白文字で分割 | `split("\\s+")`（正規表現で空白1回以上） |
| 固定区切りではなく複数の区切り文字で分割したい | `split("[,;\\s]+")`（カンマ、セミコロン、空白） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 分割基準 | 正規表現（普通の文字も使えるが注意） |
| 戻り値 | 分割された`String[]`配列 |
| limitパラメータの役割 | 要素数制限・末尾空白扱い制御 |
| 正規表現文字に注意 | 特別な意味を持つ文字（. * + など）はエスケープ必須 |
| null扱い | 呼び出し元（this）がnullならNullPointerException発生 |

---

# ✨ 超シンプルまとめ

> split()は文字列を「正規表現」で分割して配列にする！
limitを使うと分割のコントロールもできる！
正規表現の特殊文字にはエスケープ必須！
> 

✅ この感覚を押さえれば、試験でも実務でもバッチリ使いこなせます！

以下に、**「split()専用 引っかけ問題集（正規表現・limitトリック対応・全8問）」** をご用意しました！

今回は、

**「正規表現のエスケープ」「limitの効果」「空白の扱い」**

など、実際に間違いやすいポイントに絞って演習します！

---

# 🔺 `split()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：カンマで分割する基本

```java
String s = "apple,banana,cherry";
String[] parts = s.split(",");

System.out.println(Arrays.toString(parts));
```

### ✅ 問題 1の答え

**答え**：A. `[apple, banana, cherry]`

---

### ❓ 問題 2：ピリオドで分割（エスケープ忘れ）

```java
String s = "a.b.c";
String[] parts = s.split(".");

System.out.println(Arrays.toString(parts));

```

### ✅ 問題 2の答え

**答え**：B. `["", "", "", ""]`

**解説**：

- `"."`は正規表現で**「任意の1文字」**なので、すべての文字で分割されてしまう！

---

### ❓ 問題 3：ピリオドで正しく分割

正しくピリオド`.`で分割するには、どう書き換える？

A. `split("\\.")`

B. `split(".")`

C. `split("[.]")`

D. AとC両方OK

### ✅ 問題 3の答え

**答え**：D. AとC両方OK

**解説**：

- `\\.`も`[.]`も「ピリオド1個」を意味する正規表現！

---

### ❓ 問題 4：空白1個でsplitしたとき

```java
String s = "hello  world java";
String[] parts = s.split(" ");

System.out.println(Arrays.toString(parts));
```

### ✅ 問題 4の答え

**答え**：B. `[hello, "", world, java]`

**解説**：

- split(" ")は正規表現で「空白1個」だけ対象なので、連続空白の間に**空文字**が入る！

---

### ❓ 問題 5：空白1個以上を正規表現でsplit

```java
String s = "hello  world   java";
String[] parts = s.split("\\s+");

System.out.println(Arrays.toString(parts));
```

### ✅ 問題 5の答え

**答え**：A. `[hello, world, java]`

**解説**：

- `"\\s+"`なら**空白1個以上**まとめて1区切りにする！

---

### ❓ 問題 6：limit=2指定時

```java
String s = "apple,banana,cherry";
String[] parts = s.split(",", 2);

System.out.println(Arrays.toString(parts));
```

### ✅ 問題 6の答え

**答え**：B. `[apple, banana,cherry]`

**解説**：

- limit=2だと、最初の1回だけ分割して、残りはまとめて保持！

---

### ❓ 問題 7：limit=0指定時

```java
String s = "a,b,,c";
String[] parts = s.split(",", 0);

System.out.println(Arrays.toString(parts));
```

### ✅ 問題 7の答え

**答え**：A. `[a, b, "", c]`

**解説**：

- limit=0は**制限なし＋末尾の空白は削除される**（ただし途中の空白は残る）！

---

### ❓ 問題 8：limit負数指定時

```java
String s = "a,b,,c,";
String[] parts = s.split(",", -1);

System.out.println(Arrays.toString(parts));
```

### ✅ 問題 8の答え

**答え**：A. `[a, b, "", c, ""]`

**解説**：

- limit<0なら**末尾の空文字列も保持する**！

---

# ✅ split()引っかけ注意ポイントまとめ

| 罠ポイント | 正しい理解 |
| --- | --- |
| 正規表現で特殊文字を使う場合 | `\\.`のように2重エスケープが必要 |
| 空白区切り | `" "`は空白1個のみ、`\s+`なら連続空白まとめて区切れる |
| limitの効果 | 0→制限なし（末尾空白削除）、負数→制限なし＋末尾空白保持 |
| nullには安全か？ | ❌ 呼び出し元（this）がnullならNPE発生 |

---

# ✨ 超シンプルまとめ

> split()は「正規表現」で分割！
limitとエスケープに注意！
空白系や特殊文字の時は特に慎重に！
> 

✅ この感覚を押さえれば、試験も実務でもバッチリ使いこなせます！

以下に、**「split()＋Stream活用（分割→フィルタ→加工→収集）応用演習セット」** をご用意しました！

ここでは、**split()だけでは終わらず、その後にStreamでフィルタ・加工・集計**するパターンを実務寄りに演習します！

---

# 🔺 split()＋Stream活用 応用演習（分割→フィルタ→加工→収集）

---

## 【問題編】

---

### ❓ 問題 1：カンマ区切りの数値文字列から偶数だけ取り出す

```java
String s = "1,2,3,4,5,6";
```

上記の文字列から、**偶数だけを取り出してリスト化**してください。

✅ 問題 1の答え

```java
List<Integer> evens = Arrays.stream(s.split(","))
    .map(Integer::parseInt)
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evens); 
// → [2, 4, 6]
```

✅ **split→数値変換→偶数だけfilter→リスト収集！**

---

### ❓ 問題 2：スペース区切りの単語から5文字以上の単語だけ大文字に変換して収集

```java
String s = "apple banana cat dog elephant";
```

上記から、**5文字以上の単語を大文字に変換してリスト化**してください。

✅ 問題 2の答え

```java
List<String> upperWords = Arrays.stream(s.split(" "))
    .filter(word -> word.length() >= 5)
    .map(String::toUpperCase)
    .collect(Collectors.toList());

System.out.println(upperWords);
// → [APPLE, BANANA, ELEPHANT]
```

✅ **split→長さでfilter→大文字変換→リスト収集！**

---

### ❓ 問題 3：セミコロン区切りの単語から先頭が"a"の単語だけ小文字に統一して収集

```java
String s = "Apple;Banana;Avocado;cherry;Apricot";
```

先頭が"a"または"A"の単語だけ**小文字変換してリスト化**してください。

✅ 問題 3の答え

```java
List<String> aWords = Arrays.stream(s.split(";"))
    .filter(word -> word.toLowerCase().startsWith("a"))
    .map(String::toLowerCase)
    .collect(Collectors.toList());

System.out.println(aWords);
// → [apple, avocado, apricot]
```

✅ **split→小文字変換して先頭"a"判定→小文字化→リスト収集！**

---

### ❓ 問題 4：改行区切りの住所データから"東京都"を含むものだけ取り出す

```java
String s = "大阪市\n東京都新宿区\n名古屋市\n東京都渋谷区";
```

"東京都"を含む住所だけリスト化してください。

✅ 問題 4の答え

```java
List<String> tokyoAddresses = Arrays.stream(s.split("\n"))
    .filter(addr -> addr.contains("東京都"))
    .collect(Collectors.toList());

System.out.println(tokyoAddresses);
// → [東京都新宿区, 東京都渋谷区]
```

✅ **split→contains("東京都")でfilter→リスト収集！**

---

### ❓ 問題 5：カンマ区切りの文字列を重複なしでソートしてリスト化

```java
String s = "banana,apple,cherry,apple,banana";

```

重複を排除し、辞書順にソートしてリスト化してください。

✅ 問題 5の答え

```java
List<String> sortedUnique = Arrays.stream(s.split(","))
    .distinct()
    .sorted()
    .collect(Collectors.toList());

System.out.println(sortedUnique);
// → [apple, banana, cherry]
```

✅ **split→重複排除→ソート→リスト収集！**

---

# ✅ まとめ表

| ステップ順 | 説明 |
| --- | --- |
| ① split() | 区切り文字で分割して配列化 |
| ② Arrays.stream() | 配列をStreamに変換 |
| ③ 加工・フィルタリング | `filter()` / `map()` などで加工 |
| ④ collect() | リスト化 (`Collectors.toList()`やSet化なども可) |

---

# ✨ 超シンプルまとめ

> split()で分割したら、Streamで自在にフィルタ・加工・収集できる！
実務ではこのパターンが非常に多い！
> 

✅ この感覚を押さえれば、試験でも実務でも即戦力で使えます！