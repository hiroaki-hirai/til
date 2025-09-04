# append()

# ✅ **StringBuilder.appendメソッドとは**

**appendメソッド**は、

「**既存の文字列の末尾にデータを追加する**」メソッドです。

**連続して呼び出す（チェーンする）こともできる**ので、

効率よく文字列を組み立てるのに使われます。

---

# 📚 **基本情報まとめ**

| 項目 | 内容 |
| --- | --- |
| メソッドシグネチャ | `public StringBuilder append(型 value)` |
| 戻り値 | `this`（自分自身を返すので、**メソッドチェーン可能**） |
| 主な引数型 | String, char, int, double, boolean, char[], Object など |
| 例外 | なし（基本的に安全） |

---

# ✍️ **使用例（基本）**

```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World");
System.out.println(sb.toString());
```

出力：

```java
Hello World
```

---

# 🔥 **appendメソッドの特徴**

| 特徴 | 説明 |
| --- | --- |
| **オーバーロードが豊富** | int、double、char配列、Object などほぼ何でも追加できる |
| **メソッドチェーンできる** | append().append().append() と続けて書ける |
| **元のオブジェクトが変化する** | **破壊的変更（mutable）** |
| **戻り値を無視しても使える** | `sb.append("abc");`だけで完結する（戻り値使わなくてもOK） |

---

# 🌟 **メソッドチェーン例**

```java
StringBuilder sb = new StringBuilder();
sb.append("Name: ").append("John").append(", Age: ").append(25);
System.out.println(sb.toString());
```

出力：

```java
Name: John, Age: 25
```

👉 **ポイント：**

都度変数代入しなくても連続してappendできる！

---

# ⚠️ **試験・実務で注意するべき点**

| 注意点 | 説明 |
| --- | --- |
| **戻り型はStringBuilder** | Stringじゃないので、出力時はtoString()が必要 |
| **破壊的操作（mutable）** | 元のStringBuilderオブジェクトが書き換えられる |
| **Object型をappendした場合** | ObjectのtoString()が自動的に呼ばれる |
| **nullをappendすると"null"になる** | null参照をappendしても例外にはならず、文字列"null"が追加される |

---

# 🎯 【まとめ】

- **append()は「追加する」ための最重要メソッド**
- **オーバーロードがたくさんある**
- **チェーン式に連続追加できる**
- **破壊的に（元のオブジェクトを）変更する**
- **出力時にはtoString()が必要**

---

# 📦 【おまけ】頻出パターン（試験に出やすい）

```java
StringBuilder sb = new StringBuilder();
sb.append("abc");
sb.append(123);
sb.append(true);
System.out.println(sb); // abc123true
```

**ポイント：**

- 数値もbooleanも全部文字列化されて結合される
- 最後にtoString()しなくても、`System.out.println(sb);`なら暗黙的に呼ばれる

それでは「**StringBuilder.append()専用の引っかけ問題集**」を

試験・実務で**よく間違いやすいポイントだけ**に絞って作成しますね！

---

# ✍️ **StringBuilder.append() 専用 引っかけ問題集**

---

## 【問題1】

次のコードの出力結果は？

```java
StringBuilder sb = new StringBuilder("A");
sb.append("B").append("C");
System.out.println(sb);
```

問題1

---

**B（ABC）**

---

append()は戻り値がthisなので、チェーンできる。

---

## 【問題2】

次のコードの出力結果は？

```java
StringBuilder sb = new StringBuilder("Hello");
sb.append((String) null);
System.out.println(sb);
```

問題2

---

**B（Hello null）**

---

nullをappendすると「null」という文字列が追加される（例外にはならない）。

---

## 【問題3】

次のコードの出力結果は？

```java
StringBuilder sb = new StringBuilder();
sb.append(100).append(20);
System.out.println(sb);

```

問題3

---

**B（10020）**

---

数値をそのまま文字列化して連結される。計算はされない。

---

## 【問題4】

次のコードで、コンパイルエラーが発生する箇所はどこか？

```java
StringBuilder sb = new StringBuilder();
sb.append('A');
sb.append(new char[]{'B', 'C'});
sb.append(new int[]{1,2});
System.out.println(sb);
```

問題4

---

**D（append new int[]{1,2}）**

---

int[]型にはappendオーバーロードがない。char[]はOK。

---

## 【問題5】

次のコードの出力結果は？

```java
StringBuilder sb = new StringBuilder("X");
sb.append(new StringBuilder("Y"));
System.out.println(sb);

```

問題5

---

**C（XY）**

---

StringBuilderをappendすると、そのtoString()が呼ばれて連結される。

---

# 🎯 **まとめ**

| よくあるミス | 理由 |
| --- | --- |
| `null`をappendすると例外だと思う | 実際は"null"文字列が追加される |
| 数値appendで足し算されると思う | 実際は文字列化されて連結される |
| 配列全部appendできると思う | char[]だけOK、int[]はダメ |
| StringBuilder同士のappendの動き | 相手のtoString()結果が連結される |

## Q, toString()が必要な場合はどのケースか？

---

# 🎯 つまり

- **画面表示だけならtoString()は省略できる**（裏で自動実行される）
- **String型の変数に代入したり、Stringの引数が必要なメソッドに渡すなら明示的にtoString()が必要
→ StringBuilder型なので、toString()でString型へ変更する必要がある**

---

# ✍️ まとめ

| シチュエーション | 必要？ |
| --- | --- |
| `System.out.println(sb);` | **toString()明示しなくてOK** |
| `String s = sb;` （代入） | **エラー**（型が違う） |
| `String s = sb.toString();` | **必要！** 正式にString型に変換して代入する |