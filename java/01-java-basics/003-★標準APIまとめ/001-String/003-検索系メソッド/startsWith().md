# startsWith()

`String.startsWith(String prefix)` は、

**「文字列が指定した接頭辞（prefix）で始まっているかどうかを判定するメソッド」** です。

Java Silver/Gold試験でも、「contains()との違い」や「index指定ありなし」などで問われる重要メソッドです！

---

# ✅ `String.startsWith()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `startsWith()` |
| 戻り値の型 | `boolean`（始まっていればtrue） |
| 主な用途 | 接頭辞チェック（指定文字列で始まっているか？） |

---

# 🔹 シグネチャ（定義）

```java
public boolean startsWith(String prefix)

public boolean startsWith(String prefix, int toffset)
```

- **第1引数**：prefix（接頭辞、必須）
- **第2引数**（オプション）：toffset（開始位置）
    
    → そこから先がprefixで始まっているかを調べる
    

---

# 🔹 使用例（基本）

```java
String s = "JavaProgramming";

System.out.println(s.startsWith("Java")); // → true
System.out.println(s.startsWith("Pro"));  // → false
```

✅ 指定した**文字列の先頭**が一致しているかを見る！

---

# 🔹 使用例（toffset指定版）

```java
String s = "JavaProgramming";

System.out.println(s.startsWith("Pro", 4)); // → true
```

✅ `toffset=4`なら、インデックス4（'P'）からスタートして"Pro"と一致するかを見る。

---

# 🔸 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 大文字小文字を無視するか？ | ❌ 区別する（完全一致が必要） |
| 空文字 `""` をprefixに渡した場合 | ✅ 常にtrue（どんな文字列でも空文字で始まっているとみなす） |
| nullをprefixに渡した場合 | ❌ NullPointerExceptionが発生する |
| 正規表現は使えるか？ | ❌ 使えない（文字列完全一致のみ） |

---

# 🔹 contains() との違いまとめ

| 比較項目 | startsWith() | contains() |
| --- | --- | --- |
| 何を判定する？ | 先頭が指定文字列と一致するか | 文字列の中に指定文字列が含まれるか |
| 位置 | 先頭のみ | どこにあってもOK |
| 大文字小文字 | 区別する | 区別する |
| null渡し | NullPointerException発生 | 同じく発生 |

---

# 🔹 よくある用途例

- **ファイル拡張子の簡易判定**
    
    → "file.txt".startsWith("file") なら処理続行、など
    
- **コマンド入力のプレフィックス確認**
- **APIリクエストのURLルーティング**

例：

```java
String fileName = "report2025.pdf";
if (fileName.startsWith("report")) {
    System.out.println("レポートファイルです");
}
```

✅ 実務では**startsWith()だけでプレフィックス検査する**ことは非常によくあります！

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 判定基準 | 先頭がprefixと完全一致していればtrue |
| 大文字小文字 | 区別する |
| 空文字を渡したら？ | true（空文字は何にでも先頭一致とみなす） |
| nullを渡したら？ | NullPointerExceptionが発生する |
| 正規表現は使えるか？ | 使えない（単純な文字列一致のみ） |

---

# ✨ 超シンプルまとめ

> startsWith()は「先頭が一致しているかだけ」を厳密に判定する！
contains()とは範囲も意味も違う！
> 

✅ この感覚を押さえておけば完璧です！

---

# 🔺 `String.startsWith()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：基本的な先頭一致チェック

```java
String s = "Programming";
System.out.println(s.startsWith("Pro"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- "Pro"は文字列の先頭にあるので `true`！

---

### ❓ 問題 2：大文字小文字を間違えるパターン

```java
String s = "JavaProgramming";
System.out.println(s.startsWith("java"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- startsWith()は**大文字小文字を区別する**！
- "Java"と"java"は違うので `false`！

---

### ❓ 問題 3：空文字列を渡した場合

```java
String s = "OpenAI";
System.out.println(s.startsWith(""));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- **空文字列 `""` は何にでも一致する**とみなされるので `true`！

✅ contains()と同様に、空文字列には特別な扱いがある！

---

### ❓ 問題 4：toffset指定ミス

```java
String s = "HelloWorld";
System.out.println(s.startsWith("World", 5));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- toffset=5なら、インデックス5（'W'）から"World"が始まっているかを見る。
- → 'W'から始まる"World"なので `true`！

✅ toffsetを使う場合は「そこから先がprefixで始まるか」！

---

### ❓ 問題 5：nullを渡した場合

```java
String s = "TestString";
System.out.println(s.startsWith(null));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- nullを渡すと、**NullPointerException** が発生する！
- ✅ null安全ではないので注意！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| startsWith()は大文字小文字無視？ | ❌ 完全一致（大文字小文字を区別する） |
| startsWith("")はfalseになる？ | ❌ 空文字列なら常にtrue（何にでもマッチ） |
| toffsetは無視される？ | ❌ toffset以降の部分文字列に対して判定される |
| nullを渡してもfalseになる？ | ❌ NullPointerExceptionが発生する |
| 正規表現が使える？ | ❌ 使えない（単純な文字列一致のみ） |

---

# ✨ 超まとめ

> startsWith()は「先頭or指定位置からprefix一致」だけを見る！
空文字は常にtrue、大文字小文字を区別、null渡しは禁止！
> 

✅ この感覚を持っていれば完璧です！