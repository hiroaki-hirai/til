# isBlank()

---

# ✅ `String.isBlank()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `isBlank()` |
| 戻り値の型 | `boolean` |
| 主な用途 | 文字列が「空」または「空白文字だけ」かどうかを判定 |
| 追加バージョン | **Java 11** から追加 |

---

# 🔹 シグネチャ（定義）

```java
public boolean isBlank()
```

- 引数なし
- 戻り値は `true` または `false`
    - `true` → 文字列が空 (`""`) または空白文字だけ（スペース、タブ、改行など）
    - `false` → 非空白の文字が1つでも含まれている

---

# 🔹 動作イメージ（基本例）

```java
String s1 = "";
System.out.println(s1.isBlank()); // → true

String s2 = "   ";
System.out.println(s2.isBlank()); // → true

String s3 = "\n\t ";
System.out.println(s3.isBlank()); // → true

String s4 = " a ";
System.out.println(s4.isBlank()); // → false
```

✅ **空白だけ**ならOK、**非空白があったらNG**という判定基準です！

---

# 🔸 `isEmpty()` との違い

| 項目 | isEmpty() | isBlank() |
| --- | --- | --- |
| 判定基準 | 長さが0だけ | 空か空白文字だけならtrue |
| 空白 `" "` を渡した場合 | `false`（1文字あるから） | `true`（空白だけだからOK） |
| Javaバージョン | Java 6から | Java 11から |

---

# 🔹 内部的なイメージ（処理内容）

実際には、以下のようなチェックがされています：

```java
for (int i = 0; i < value.length; i++) {
    if (!Character.isWhitespace(value[i])) {
        return false;
    }
}
return true;
```

- つまり、**文字列の各charについて、全部空白（whitespace）ならtrue**。

（ここでいう「空白」とは、スペース、タブ、改行、フォームフィード、キャリッジリターンなどを含みます。）

---

# 🔹 よくある用途例

- **フォーム入力のバリデーション**
    - 入力が空白だけならエラーにする
- **データクリーニング**
    - 空白だけの行を除外する
- **検索ボックスの無効チェック**
    - 空白だけの検索は実行しない

例：

```java
if (input == null || input.isBlank()) {
    System.out.println("入力が空または空白のみです");
}
```

✅ **nullチェック＋isBlank()** を組み合わせるのが実務の鉄則！

---

# 🔹 よくある注意ポイント

| 注意点 | 解説 |
| --- | --- |
| nullに対して呼ぶとどうなる？ | ❌ `NullPointerException` が発生する！（nullチェック必須） |
| 空白が混じっていてもtrueになる？ | ✅ 空白だけならtrueになる |
| 半角スペース、タブ、改行なども対象？ | ✅ 全部「空白文字」とみなされる |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 判定基準 | 空または空白文字だけならtrue |
| 空白 `" "` は？ | true |
| 改行 `"\n"` は？ | true |
| null安全？ | ❌ nullは事前チェックが必要（そのまま呼ぶと例外） |
| 実務用途 | フォームバリデーション、データクリーニングなど |

---

# ✨ 超シンプルまとめ

> isBlank()は「空、または空白だけならtrue」、
空白も含めて"何もない"とみなしたいときに使う！
> 

✅ これさえ覚えれば、完璧です！

---

# 🔺 `String.isBlank()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：空文字列

```java
String s = "";
System.out.println(s.isBlank());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 空文字列（長さ0）は**isBlank()でもtrue**になります！

---

### ❓ 問題 2：スペース1個の場合

```java
String s = " ";
System.out.println(s.isBlank());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- スペースも**空白文字**なので、isBlank()は**true**を返す！

✅ isEmpty()とは違う点に注意！

---

### ❓ 問題 3：改行やタブだけの場合

```java
String s = "\n\t ";
System.out.println(s.isBlank());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 改行 `\n`、タブ `\t`、スペースもすべて**空白文字**扱い！
- 文字は入っているが「空白だけ」なので**true**！

---

### ❓ 問題 4：非空白文字が含まれる場合

```java
String s = " abc ";
System.out.println(s.isBlank());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- 'a' など非空白文字が含まれているので、isBlank()は**false**！

---

### ❓ 問題 5：nullに対して呼び出す場合

```java
String s = null;
System.out.println(s.isBlank());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- null参照に対してメソッドを呼び出すと、**NullPointerException** が発生！

✅ **null安全ではない**ので注意！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 空白だけでもfalseになる？ | ❌ 空白だけならisBlank()はtrue |
| 改行やタブは空白扱いじゃない？ | ❌ 改行・タブも空白とみなされ、trueになる |
| nullならisBlank()もtrueになる？ | ❌ NullPointerExceptionが発生する（nullチェック必須） |
| isBlank()はisEmpty()と同じ？ | ❌ 空白だけならisEmpty()はfalse、isBlank()はtrue |

---

# ✨ 超まとめ

> isBlank()は「空または空白だけならtrue」、
nullには要注意（事前チェック必須）！
> 

✅ この感覚を持てれば完璧です！