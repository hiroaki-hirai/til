# insert()

---

# ✅ 【StringBuilder.insertメソッドとは】

**StringBuilder.insertメソッド**は、

**指定した位置に、指定した内容を挿入する**ためのメソッドです。

- **追加ではない**！（末尾ではなく、途中に「挿入」）
- **元の文字列をシフトして押し広げる**イメージ
- **元のオブジェクトを直接変更**（Stringとは違い、可変）

---

# 📚 【基本構文】

```java
StringBuilder insert(int offset, データ型 value)
```

| パラメータ | 意味 |
| --- | --- |
| offset | 挿入する位置（0から始まるインデックス） |
| value | 挿入する内容（文字列、数値、文字、boolean、など） |

---

# 🎯 【例：基本使用例】

```java
StringBuilder sb = new StringBuilder("HelloWorld");
sb.insert(5, " ");  // インデックス5に空白を挿入
System.out.println(sb); // → "Hello World"
```

✅ インデックス5の位置（"W"の直前）に空白が挿入された！

---

# ✨ 【色々なデータ型が使える！】

```java
StringBuilder sb = new StringBuilder("Age:");
sb.insert(4, 20);
System.out.println(sb); // → "Age:20"

```

- 数値（20）もそのまま挿入できる（内部的にStringに変換される）
- 他にも、boolean型、char型、配列、CharSequenceなども挿入可能！

---

# 📢 【注意点】

| 注意ポイント | 内容 |
| --- | --- |
| offsetは0から始まる | 0なら先頭に挿入、`length()`と同じなら末尾に挿入 |
| 不正な位置を指定すると例外 | offsetが負数や長さ超過なら `StringIndexOutOfBoundsException` が発生 |
| StringBuilderは可変 | 毎回新しいオブジェクトは作られず、同じオブジェクトの中身が変わる |

---

# 📚 【よくあるパターン別例】

### パターン①：先頭に挿入

```java
StringBuilder sb = new StringBuilder("World");
sb.insert(0, "Hello ");
System.out.println(sb); // → "Hello World"
```

---

### パターン②：末尾に挿入（appendと同じ効果）

```java
StringBuilder sb = new StringBuilder("Hello");
sb.insert(sb.length(), " World");
System.out.println(sb); // → "Hello World"
```

（※appendを使ったほうが普通はスマートだけど、insertでもできる）

---

### パターン③：複数回挿入

```java
StringBuilder sb = new StringBuilder("ABC");
sb.insert(1, "1").insert(3, "2");
System.out.println(sb); // → "A1B2C"
```

- insertは**連続呼び出し（メソッドチェーン）**できる！

---

# 🛑 【エラー例：不正な位置】

```java
StringBuilder sb = new StringBuilder("Test");
sb.insert(10, "X");  // ❌ StringIndexOutOfBoundsException
```

- 長さを超える位置（10）を指定すると例外になるので注意！

---

# 🔥 【超まとめ】

| 項目 | 内容 |
| --- | --- |
| 何をする？ | 文字列の指定位置にデータを挿入する |
| どんなデータ？ | 文字列、数値、boolean、char、配列、CharSequenceなどOK |
| 特徴 | 元オブジェクトを書き換える（可変） |
| 注意点 | 位置ミスで例外発生に注意 |

それでは、**StringBuilder.insert() 応用問題集（位置ずれトリック・配列挿入・null挿入など）** を作成します！

---

# ✅ 【StringBuilder.insert() 応用問題集】

---

## 【問題1】位置ずれトリック①

次のコードの出力結果は？

```java
StringBuilder sb = new StringBuilder("ABCD");
sb.insert(1, "X");
sb.insert(3, "Y");
System.out.println(sb);
```

✅ **出力結果**

```java
AXBYCD
```

🔵 【解説】

- 1文字目（Bの前）にXを挿入 → "AXBCD"
- その後、**3文字目**（Cの前）にYを挿入 → "AXBYCD"
- insertすると、**後ろが押し出されるので注意**！

---

# ✅ 【問題2】位置ずれトリック②（連続insert）

```java
StringBuilder sb = new StringBuilder("12345");
sb.insert(2, "A"); // → 12A345
sb.insert(4, "B"); // → 12A3B45
sb.insert(6, "C"); // → 12A3B4C5
System.out.println(sb);
```

✅ **出力結果**

```java
12A3B4C5
```

🔵 【解説】

- 挿入するたびに「右にずれる」ので、次のinsert位置を間違えないように！
- insert後の文字列を常に意識するのがコツ。

---

# ✅ 【問題3】配列挿入

```java
char[] arr = {'X', 'Y', 'Z'};
StringBuilder sb = new StringBuilder("Hello");
sb.insert(5, arr);
System.out.println(sb);
```

🔵 【解説】

- 配列 `{'X', 'Y', 'Z'}` をまとめて挿入
- 配列全体を文字列として挿入される（"XYZ"）

---

# ✅ 【問題4】部分配列挿入

```java
char[] arr = {'A', 'B', 'C', 'D', 'E'};
StringBuilder sb = new StringBuilder("12345");
sb.insert(2, arr, 1, 3);
System.out.println(sb);
```

✅ **出力結果**

```java
12BCD345
```

🔵 【解説】

- `arr, 1, 3` → 配列arrの**index1から3文字分**を取る（B, C, D）
- それを2文字目（3の直前）に挿入
- **範囲指定insert**も可能！

---

# ✅ 【問題5】null挿入時の挙動①

```java
StringBuilder sb = new StringBuilder("Test");
sb.insert(2, (String) null);
System.out.println(sb);
```

✅ **出力結果**

```java
Tenullst
```

🔵 【解説】

- **nullを挿入すると"null"（文字列）になる**
- 例外は発生しない
- 挿入位置2なので、"Te"のあとに"null"が入る

---

# ✅ 【問題6】null挿入時の挙動②（オブジェクト型）

```java
Object obj = null;
StringBuilder sb = new StringBuilder("Java");
sb.insert(2, obj);
System.out.println(sb);
```

✅ **出力結果**

```java
Janulva
```

🔵 【解説】

- Object型でも同様に、nullを渡すと"null"が挿入される
- 2文字目（vの前）に"null"が挿入されている

---

# ✅ 【問題7】エラーになるか？位置ミストラップ

```java
StringBuilder sb = new StringBuilder("Hello");
sb.insert(10, "X");
System.out.println(sb);
```

✅ **結果**

**例外発生**

```java
Exception in thread "main" java.lang.StringIndexOutOfBoundsException: offset 10, length 5
```

🔵 【解説】

- StringBuilderの長さは5なのに、10番目に挿入しようとするとエラー
- `offset`は **0～length()まで** の範囲でなければならない！

---

# 🎯 【まとめ表：insert応用の注意点】

| パターン | 注意ポイント |
| --- | --- |
| 位置ずれ | 挿入するたびに右にシフトするので、次の挿入位置に注意 |
| 配列挿入 | char配列はそのまま文字列化して挿入される |
| 部分配列挿入 | 配列の一部分だけ指定して挿入できる |
| null挿入 | 例外にならず、"null"（文字列）として挿入される |
| 不正位置 | 長さを超える位置を指定すると例外（StringIndexOutOfBoundsException） |

---

# 📢 【ここまで押さえたあなたができること】

✅ StringBuilderのinsertを自在に操れる

✅ トリッキーな位置ずれやnull挿入にも対応できる

✅ Java標準APIの「仕様まで理解した使い方」ができる

レベル、かなり高いです！