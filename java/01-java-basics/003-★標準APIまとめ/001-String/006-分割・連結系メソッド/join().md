# join()

---

# ✅ `String.join()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `join(CharSequence delimiter, CharSequence... elements)` など |
| 戻り値の型 | `String`（結合された文字列） |
| 主な用途 | **複数の文字列を、指定した区切り文字で結合する** |

---

# 🔹 シグネチャ（定義）

主な2種類のオーバーロードがあります。

```java
public static String join(CharSequence delimiter, CharSequence... elements)
public static String join(CharSequence delimiter, Iterable<? extends CharSequence> elements)
```

- **delimiter**：
    - 要素同士をつなぐための区切り文字列。
- **elements**：
    - 結合したい複数の文字列（配列またはイテラブル）。

---

# 🔹 使用例（基本）

### ① 可変長引数版（配列・複数文字列）

```java
String result = String.join("-", "apple", "banana", "cherry");

System.out.println(result);
// → "apple-banana-cherry"
```

✅ `"apple"`, `"banana"`, `"cherry"`が **"-"** で結合される！

---

### ② Iterable版（リストなど）

```java
List<String> fruits = List.of("apple", "banana", "cherry");
String result = String.join(" / ", fruits);

System.out.println(result);
// → "apple / banana / cherry"
```

✅ リストからでも直接結合できる！

---

# 🔹 よくある用途・パターン

| シチュエーション | 使用例 |
| --- | --- |
| CSV出力 | `String.join(",", columns)` |
| URLパス結合 | `String.join("/", "api", "v1", "users")` |
| ファイルパス作成（Windowsなら"\"） | `String.join("\\", "C:", "Users", "Admin")` |
| リスト表示（ユーザー名一覧など） | `String.join("、", usernames)` |

---

# 🔹 注意点まとめ

| 項目 | 内容 |
| --- | --- |
| null要素がある場合 | NullPointerExceptionになる（elementsにnullが入っていると例外） |
| null文字列を防ぐには？ | 事前にfilterするか、Optional扱いする必要がある |
| delimiterはnull可？ | ❌ delimiterがnullならNullPointerExceptionになる |
| 元のリストは変わる？ | ❌ 元データは変更されない（immutable設計） |

---

# 🔹 split()との対比

| 機能 | split() | join() |
| --- | --- | --- |
| 何をする？ | 文字列を区切り文字で分割して配列にする | 配列やリストを区切り文字で結合して1つの文字列にする |
| 典型的な使い方 | "a,b,c"→["a","b","c"] | ["a","b","c"]→"a,b,c" |

---

# ✅ まとめ

| ポイント | 内容 |
| --- | --- |
| 何をする？ | 複数の文字列を、指定区切り文字で連結する |
| 対象 | 配列、リスト（Iterable） |
| null注意事項 | delimiterも要素もnullだと例外発生 |
| splitとの関係 | 逆操作（splitは分割、joinは結合） |
| よく使うシーン | CSV生成、パス生成、リスト表示、ログ出力など多用途！ |

---

# ✨ 超シンプルまとめ

> join()は「区切り文字をはさんで文字列をまとめる」！
配列やリストから簡単にきれいな文字列を作れる！
null混入には要注意！
> 

✅ この感覚を押さえれば、試験も実務もバッチリ使いこなせます！

以下に、**「join()専用 引っかけ問題集（nullトリック・区切り文字設定ミス防止 全8問）」** をご用意しました！

今回は、

**「null混入の扱い」「区切り文字指定のミス」「配列とリストの違い」**

など、実際にミスしやすいポイントを徹底トレーニングします！

---

# 🔺 `join()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：基本的なjoin動作

```java
String result = String.join("-", "a", "b", "c");
System.out.println(result);
```

### ✅ 問題 1の答え

**答え**：A. `a-b-c`

---

### ❓ 問題 2：区切り文字が空文字

```java
String result = String.join("", "a", "b", "c");
System.out.println(result);
```

### ✅ 問題 2の答え

**答え**：A. `abc`

**解説**：

- 区切り文字が空文字なら、単純に連結される！

---

### ❓ 問題 3：null区切り文字を渡したら？

```java
String result = String.join(null, "a", "b", "c");
System.out.println(result);
```

### ✅ 問題 3の答え

**答え**：D. 実行時例外（NullPointerException）

**解説**：

- delimiterがnullだと即NPE！

---

### ❓ 問題 4：要素にnullが含まれている場合（可変長引数版）

```java
String result = String.join("-", "a", null, "c");
System.out.println(result);
```

### ✅ 問題 4の答え

**答え**：C. 実行時例外（NullPointerException）

**解説**：

- 要素にnullがあるとNPEになる！

---

### ❓ 問題 5：リスト版joinで要素がnullの場合

```java
List<String> list = Arrays.asList("a", null, "c");
String result = String.join("-", list);
System.out.println(result);
```

### ✅ 問題 5の答え

**答え**：C. 実行時例外（NullPointerException）

**解説**：

- リスト版でも要素にnullがあるとダメ！

---

### ❓ 問題 6：空のリストをjoinすると？

```java
List<String> empty = List.of();
String result = String.join("-", empty);
System.out.println(result);
```

### ✅ 問題 6の答え

**答え**：B. `""`（空文字）

**解説**：

- 空のリストをjoinすると空文字列！

---

### ❓ 問題 7：join対象が1個だけなら？

```java
String result = String.join("-", "onlyone");
System.out.println(result);
```

### ✅ 問題 7の答え

**答え**：A. `onlyone`

**解説**：

- 区切り文字を挟む相手がいないので、単純にその要素だけ！

---

### ❓ 問題 8：nullを回避した安全なjoin方法は？

次のうち、安全にnullを排除してjoinする方法は？

```java
list.stream().filter(Objects::nonNull).collect(Collectors.joining("-"));
```

**解説**：

- null排除するならfilter必須！

---

# ✅ join()引っかけ注意ポイントまとめ

| 罠ポイント | 正しい理解 |
| --- | --- |
| delimiterがnull | ❌ NPE（必ず区切り文字は設定する） |
| 要素にnullがある | ❌ NPE（streamでfilterして除去するのが安全） |
| 空リストをjoinすると？ | ✅ 空文字列を返す |
| 要素1個だけの場合 | ✅ 区切り文字は使われず、その1個がそのまま返る |

---

# ✨ 超シンプルまとめ

> join()はnullに非常に厳しい！
delimiterも要素もnull不可！
安全運用ではfilter(Objects::nonNull)必須！
> 

✅ この感覚を押さえれば、試験も実務でも安心してjoin()を活用できます！