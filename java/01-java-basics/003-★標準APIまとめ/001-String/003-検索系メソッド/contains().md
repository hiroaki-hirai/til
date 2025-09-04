# contains()

`String.contains()` は、

**「文字列の中に、指定した部分文字列が含まれているかどうかを判定するメソッド」**です。

文字列の検索にとてもよく使われ、**Java Silver/Gold試験でも高頻度で出題される**重要メソッドです！

---

# ✅ `String.contains()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `contains()` |
| 戻り値の型 | `boolean`（含まれていればtrue） |
| 主な用途 | 部分文字列の存在チェック |
| 追加バージョン | Java 1.5から |

---

# 🔹 シグネチャ（定義）

```java
public boolean contains(CharSequence s)
```

- 引数：`CharSequence` 型（たとえば `String` など）
- 戻り値：
    - `true` → 指定した文字列を含んでいる
    - `false` → 含んでいない

---

# 🔹 使用例（基本）

```java
String s = "JavaProgramming";

System.out.println(s.contains("Program")); // → true
System.out.println(s.contains("gram"));    // → true
System.out.println(s.contains("Python"));  // → false
```

✅ **完全一致でなくても**、部分的に含まれていれば`true`になります！

---

# 🔸 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 大文字小文字の扱い | ✅ 区別する（`"Java"` と `"java"` は違う） |
| nullを渡した場合 | ❌ `NullPointerException` が発生する |
| 空文字列 `""` を渡した場合 | ✅ 常にtrue（空文字はどこにでも"含まれている"とみなす） |
| ワイルドカード指定 | ❌ サポートしない（完全な部分文字列だけ探す） |

---

# 🔹 contains() と indexOf() の関係

実は、`contains()`の内部実装は**indexOf()を使っています**！

つまり、簡単に言うと：

```java
public boolean contains(CharSequence s) {
    return indexOf(s.toString()) >= 0;
}
```

✅ **indexOf()で見つかったらtrue、見つからなかったらfalse**

というシンプルなロジックです！

---

# 🔹 よくある用途例

- **キーワード検索**
    
    → 文章に特定の単語が含まれているか探す
    
- **パターンフィルター**
    
    → リストから"〇〇を含む"データだけ抽出
    
- **簡単なバリデーション**
    
    → URLに"https"が入っているか確認、など
    

例：

```java
if (url.contains("https")) {
    System.out.println("安全なURLです");
}
```

---

# 🔹 よくあるミス・注意点

| ミス内容 | 正しい理解 |
| --- | --- |
| 大文字小文字を無視して判定できる？ | ❌ 大文字小文字は区別される |
| nullを渡したらどうなる？ | ❌ NullPointerException（必ずnullチェックすること） |
| contains()で正規表現を使える？ | ❌ できない（正規表現はmatches()などを使う） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 判定基準 | 部分文字列が含まれていればtrue |
| 大文字小文字 | 区別する |
| nullを渡したとき | NullPointerExceptionが発生 |
| 空文字を渡したとき | 常にtrue（空文字は全ての文字列に含まれるとみなす） |

---

# ✨ 超シンプルまとめ

> contains()は「部分文字列が入ってるか？」を素早く判定するメソッド！
大文字小文字を区別し、nullには要注意！
> 

✅ これを押さえておけば、試験も実務も安心です！

---

# 🔺 `String.contains()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：基本的な部分一致判定

```java
String s = "JavaProgramming";
System.out.println(s.contains("Program"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- "Program" という部分文字列は含まれているので `true`！

---

### ❓ 問題 2：大文字小文字を間違えるパターン

```java
String s = "JavaProgramming";
System.out.println(s.contains("program"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- "program"（小文字）は"JavaProgramming"には含まれていない。
- ✅ **contains()は大文字小文字を区別する**！

---

### ❓ 問題 3：空文字列を渡した場合

```java
String s = "Java";
System.out.println(s.contains(""));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- **空文字列 `""` はすべての文字列に含まれている**とみなされるので `true`。

✅ よく引っかかるポイント！

---

### ❓ 問題 4：nullを渡した場合

```java
String s = "Java";
System.out.println(s.contains(null));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- nullを渡すと `NullPointerException` が発生！
- ✅ **事前にnullチェックが必要**！

---

### ❓ 問題 5：正規表現の間違いパターン

```java
String s = "Java123";
System.out.println(s.contains("\\d+"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- `contains()`は**単なる部分文字列の検索**をするだけ。
- 正規表現（`\d+`）は**そのまま文字列として扱われる**ので、見つからず `false`。

✅ 正規表現を使いたいなら、`matches()`や`Pattern`を使うべき！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 大文字小文字は無視される？ | ❌ 完全一致（大文字小文字を区別する） |
| 空文字列を渡したらfalseになる？ | ❌ 空文字は常にtrue（どの文字列にも含まれているとみなす） |
| nullを渡してもfalseになる？ | ❌ NullPointerExceptionが発生する |
| 正規表現が使える？ | ❌ contains()は正規表現を解釈しない（文字列一致のみ） |

---

# ✨ 超まとめ

> contains()は「部分文字列が含まれるか」だけを見る！
大文字小文字は区別、空文字はtrue、null渡しは禁止！
> 

✅ この感覚を持てば、試験も実務も安心です！

---

# 🔺 `contains()` 実務応用パターン問題集（リストフィルタリングなど）

---

## 【リストフィルタリング：基本】

---

### ❓ 問題 1：リストから "Java" を含む要素だけ抽出

```java
List<String> list = List.of("Java", "Python", "JavaScript", "C++");

list.stream()
    .filter(s -> s.contains("Java"))
    .forEach(System.out::println);
```

出力される要素は？

A. `Java` のみ

B. `Java`, `JavaScript`

C. `Java`, `JavaScript`, `C++`

D. すべて

**答え**：B. `Java`, `JavaScript`

**解説**：

- "Java"という部分文字列を含むものが対象。
- "Java"も"JavaScript"も含まれる！

---

## 【リストフィルタリング：大文字小文字注意】

---

### ❓ 問題 2：大文字小文字を無視して "python" を含む要素を抽出

```java
List<String> list = List.of("Java", "Python", "javaScript", "PYTHON");

list.stream()
    .filter(s -> s.toLowerCase().contains("python"))
    .forEach(System.out::println);
```

出力される要素は？

A. `Python` のみ

B. `PYTHON` のみ

C. `Python`, `PYTHON`

D. 出力なし

**答え**：C. `Python`, `PYTHON`

**解説**：

- `toLowerCase()`で全部小文字に変換してから`contains()`している！

✅ **大文字小文字無視したいなら、事前に変換して判定する**！

---

## 【リストフィルタリング：空文字注意】

---

### ❓ 問題 3：空文字列を含むかチェック

```java
List<String> list = List.of("apple", "", "banana");

long count = list.stream()
    .filter(s -> s.contains(""))
    .count();

System.out.println(count);
```

の出力は？

A. `1`

B. `2`

C. `3`

D. `実行時例外`

**答え**：C. `3`

**解説**：

- `contains("")`は**常にtrue**！
- リストの全要素（3件）すべてがマッチする！

---

## 【リストフィルタリング：null対応】

---

### ❓ 問題 4：nullが混じるリストを安全に判定

```java
List<String> list = Arrays.asList("dog", null, "cat");

list.stream()
    .filter(s -> s != null && s.contains("a"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"dog"`

B. `"cat"`

C. `"dog"`, `"cat"`

D. 実行時例外

**答え**：B. `"cat"`

**解説**：

- nullチェック (`s != null`) をしてからcontains()を呼んでいるので安全！
- "cat"だけ"a"を含む！

---

## 【リストフィルタリング：先頭一致風フィルタリング】

---

### ❓ 問題 5："J"から始まる要素を取りたいがcontainsを使った場合

```java
List<String> list = List.of("Java", "JavaScript", "Python", "C++");

list.stream()
    .filter(s -> s.contains("J"))
    .forEach(System.out::println);
```

出力される要素は？

A. `"Java"`, `"JavaScript"`

B. `"Java"`, `"JavaScript"`, `"C++"`

C. `"Java"` のみ

D. 出力なし

**答え**：A. `"Java"`, `"JavaScript"`

**解説**：

- `contains("J")`は「どこかに'J'が含まれていればOK」なので、
- "Java"と"JavaScript"が対象。

✅ **先頭一致ならstartsWith()を使うべきだが、ここではcontains()なので「含まれていれば」OK**！

---

## 【リストフィルタリング：複合条件】

---

### ❓ 問題 6："Java"を含み、かつ長さが8以上の要素だけ取得

```java
List<String> list = List.of("Java", "JavaScript", "Python", "C++");

list.stream()
    .filter(s -> s.contains("Java") && s.length() >= 8)
    .forEach(System.out::println);
```

出力される要素は？

A. `"JavaScript"`

B. `"Java"`

C. `"Java"`, `"JavaScript"`

D. 出力なし

**答え**：A. `"JavaScript"`

**解説**：

- "Java"を含み、かつ長さ8以上 → "JavaScript"のみ条件に合う！

---

# ✅ まとめ

| ポイント | 説明 |
| --- | --- |
| 大文字小文字を無視したい場合 | `toLowerCase()`や`toUpperCase()`で揃えてから判定 |
| 空文字`""`はすべてにマッチする | リスト全体にマッチするので注意 |
| nullが混じるリストでは要nullチェック | (`s != null && s.contains(...))` |
| startsWith/endsWithとの違いに注意 | containsは「部分一致」、先頭・末尾限定ではない |

---

# ✨ 超シンプルまとめ

> contains()をリストフィルタリングで使うときは、
空文字・null・大文字小文字に要注意！
> 

✅ これを押さえておけば、実務でも完璧に使いこなせます！