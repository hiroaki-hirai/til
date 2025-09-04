# intern()

`String.intern()`は、

**「文字列を文字列プール（String Pool）に登録し、そのプール内の参照を返すメソッド」**です！

実務でも**メモリ最適化**や**==による比較を成立させるテクニック**として、また試験（特にJava Silver/Gold）でもよく問われる重要項目です！

---

# ✅ `String.intern()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `intern()` |
| 戻り値の型 | `String`（プール内にある場合その参照を、なければ登録してその参照を） |
| 主な用途 | **文字列プール（共有領域）**への登録と参照取得 |

---

# 🔹 シグネチャ（定義）

```java
public String intern()
```

- 引数なし。
- 返り値：文字列プールに存在するインスタンスの参照（なければ登録して返す）。

---

# 🔹 使用例（基本）

```java
String a = new String("hello");
String b = "hello";

System.out.println(a == b);             // false（参照が違う）
System.out.println(a.intern() == b);    // true（参照が同じ！）
```

✅ **intern()するとリテラルと同じ参照になる！**

---

# 🔹 文字列プールとは？

- Javaは**Stringリテラル**をヒープ上ではなく、**共有プール（String Pool）**に保存します。
- 同じリテラル文字列なら**1個だけ**作られて共有されます（メモリ節約のため）。
- つまり、
    
    `"hello" == "hello"` は**true** になることがあるのはこのため！
    

---

# 🔹 intern() の効果まとめ

| 項目 | intern()なし | intern()あり |
| --- | --- | --- |
| new String("text")を作った場合 | ヒープ上に新しいインスタンスができる | プール内のインスタンス参照を返す（共有される） |
| 参照比較(==)が成功するか | ❌ 基本的に失敗（別インスタンス） | ✅ 成功する（同じ参照） |
| メモリ効率 | 悪い（同じ内容でも別インスタンス） | 良い（共通インスタンスを使う） |

---

# 🔹 よくある応用パターン

- **文字列の大量生成（例：大量ログ）でメモリ節約したい時**
- **equals()を使わずに==で文字列比較したい時（パフォーマンス重視）**
- **キャッシュ用途（文字列キーを省メモリに管理したい場合）**

---

# 🔹 intern()の注意点

| 注意ポイント | 内容 |
| --- | --- |
| 使いすぎ注意 | メモリ節約にはなるが、**過剰に使うとプールが肥大化**して逆にメモリを圧迫する |
| 効果があるのはStringだけ | 他のオブジェクト型にはintern()のような仕組みはない（String専用） |
| equals()との違い | intern()は参照を合わせる、equals()は内容を比較する |

---

# 🔹 【まとめ図解】

```java
String a = new String("java");
String b = "java";

a == b         → false
a.intern() == b → true
```

✅ **intern()すればリテラルと同じ参照を使える！**

---

# ✅ 最後のまとめ

| 比較観点 | 内容 |
| --- | --- |
| メイン用途 | 文字列プールに登録して**参照を揃える**こと |
| 使うメリット | メモリ節約、==による高速比較が可能になる |
| よくある使いどころ | キャッシュ、ログ解析、大量の同内容文字列管理 |
| 使う際の注意 | 無闇に使うとプールが膨れて逆効果になることもある |

---

# ✨ 超シンプルまとめ

> intern()は「文字列をプールに登録して共通参照にする」メソッド！
==で比較できるようにするために使う！
メモリ節約とパフォーマンス向上に効果あり！
> 

✅ この感覚を押さえておけば、試験・実務両方で完璧に対応できます！

以下に、**「intern()専用 引っかけ問題集（リテラル/変数/new混合パターン全8問）」** をご用意しました！

特に、

**リテラルか？変数か？new Stringか？intern()しているか？**

を慎重に見極めないと引っかかるようになっています！

---

# 🔺 `intern()`専用 引っかけ問題集（全8問）

---

### ❓ 問題 1：リテラル同士の比較

```java
String a = "java";
String b = "java";

System.out.println(a == b);
```

**答え**：`true`

**解説**：

- リテラル同士は**文字列プール**共有、参照が同じ！

---

### ❓ 問題 2：new String()同士の比較

```java
String a = new String("java");
String b = new String("java");

System.out.println(a == b);
```

**答え**：`false`

**解説**：

- new String()は**別インスタンス**が作られるので参照は異なる！

---

### ❓ 問題 3：new String()＋intern()比較

```java
String a = new String("java").intern();
String b = "java";

System.out.println(a == b);
```

**答え**：`true`

**解説**：

- intern()でプールに登録、リテラルと参照一致！

---

### ❓ 問題 4：変数連結 vs リテラル比較

```java
String part = "ja";
String a = "java";
String b = part + "va";

System.out.println(a == b);
```

**答え**：`false`

**解説**：

- **変数**を使った連結はコンパイル時にリテラル化されない！
- 実行時連結なので別インスタンス。

---

### ❓ 問題 5：変数連結＋intern()比較

```java
String part = "ja";
String a = "java";
String b = (part + "va").intern();

System.out.println(a == b);
```

**答え**：`true`

**解説**：

- intern()をかけたのでプール参照に統一！

✅ **実行時連結でもintern()で合わせられる！**

---

### ❓ 問題 6：リテラルとnew String()＋intern()なし

```java
String a = "java";
String b = new String("java");

System.out.println(a == b);
```

**答え**：`false`

**解説**：

- new String()は**常に新規インスタンス**。
- intern()してないので参照は違う！

---

### ❓ 問題 7：同じnew String()でも参照違う？

```java
String a = new String("java");
String b = new String("java");

System.out.println(a.intern() == b.intern());
```

**答え**：`true`

**解説**：

- intern()すると両方ともプールの"java"参照を指す！

✅ **複数のnew String()でもintern()で参照統一できる！**

---

### ❓ 問題 8：intern()せずequals()で比較

```java
String a = new String("java");
String b = "java";

System.out.println(a.equals(b));
```

**答え**：`true`

**解説**：

- equals()は**内容比較**なので参照が違ってもtrue！

✅ intern()は不要、equals()なら内容だけを見る！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| new String()なら中身も違う？ | ❌ 中身は同じ。参照が違うだけ！ |
| 変数連結してもリテラル扱い？ | ❌ 変数絡むと実行時連結。リテラル化されない！ |
| intern()すると内容が変わる？ | ❌ 内容は変わらない。参照だけプール共有のものに変わる！ |
| equals()は参照比較する？ | ❌ equals()は内容比較だけ。参照違ってもtrueになることがある！ |

---

# ✨ 超シンプルまとめ

> new String()は必ず別参照！
リテラル同士は同じ参照！
変数連結は別インスタンス、intern()すればリテラルと一致！
==は参照比較、equals()は内容比較！
> 

✅ ここまで押さえれば、試験でも実務でも完璧に対応できます！

**リテラル/変数/new/intern()の組み合わせを見抜き、正しい比較（== / equals）を選べるか？**

にフォーカスした**プロレベルの応用問題**になっています！

---

# 🔺 `== / equals() / intern()` ミックス超応用問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：リテラル＋new String()比較

```java
String a = "openai";
String b = new String("openai");

System.out.println(a == b);
System.out.println(a.equals(b));
```

### ✅ 問題 1の答え

出力：

- `false`
- `true`

**解説**：

- new String()は参照が違う（==はfalse）。
- 内容は同じなのでequals()はtrue。

---

### ❓ 問題 2：変数連結とリテラル比較

```java
String x = "open";
String a = "openai";
String b = x + "ai";

System.out.println(a == b);
System.out.println(a.equals(b));

```

### ✅ 問題 2の答え

出力：

- `false`
- `true`

**解説**：

- 変数を使った連結は実行時連結なので==はfalse。
- 中身は同じなのでequals()はtrue。

---

### ❓ 問題 3：変数連結＋intern()

```java
String x = "open";
String a = "openai";
String b = (x + "ai").intern();

System.out.println(a == b);
System.out.println(a.equals(b));

```

### ✅ 問題 3の答え

出力：

- `true`
- `true`

**解説**：

- intern()でプールに戻したため==もtrue。

---

### ❓ 問題 4：new String()同士のintern比較

```java
String a = new String("chat");
String b = new String("chat");

System.out.println(a.intern() == b.intern());
```

### ✅ 問題 4の答え

出力：

- `true`

**解説**：

- 両方intern()でプールに戻しているので==もtrue。

---

### ❓ 問題 5：連結リテラル vs new String()比較（internなし）

```java
String a = "ch" + "at";
String b = new String("chat");

System.out.println(a == b);
System.out.println(a.equals(b));
```

### ✅ 問題 5の答え

出力：

- `false`
- `true`

**解説**：

- new String()は参照違うが、中身は同じ。

---

### ❓ 問題 6：連結リテラル vs new String()比較（internあり）

```java
String a = "ch" + "at";
String b = new String("chat").intern();

System.out.println(a == b);
System.out.println(a.equals(b));
```

### ✅ 問題 6の答え

出力：

- `true`
- `true`

**解説**：

- intern()でリテラルと揃えたため参照も一致！

---

### ❓ 問題 7：変数で連結後、internを忘れた場合

```java
String part = "open";
String a = "openai";
String b = part + "ai";

System.out.println(a == b.intern());
```

### ✅ 問題 7の答え

出力：

- `true`

**解説**：

- b.intern()でプールの"openai"参照になる！

---

### ❓ 問題 8：equals()だけならintern()は必要か？

```java
String a = new String("java");
String b = "java";

System.out.println(a.equals(b));
System.out.println(a.intern().equals(b));
```

### ✅ 問題 8の答え

出力：

- `true`
- `true`

**解説**：

- equals()は**中身比較**なのでintern()しなくても結果は同じ。
- intern()してもequals()結果は変わらない。

---

# ✅ 難易度アップまとめ

| 比較パターン | ポイント |
| --- | --- |
| リテラル vs new String() | ==はfalse、equals()はtrue |
| 変数連結 vs リテラル | ==はfalse、intern()すれば==true |
| new String()同士 | ==はfalse、intern()してから==比較すればtrue |
| equals()とintern() | equals()だけならintern()不要（中身だけを見る） |

---

# ✨ 超シンプルまとめ

> new String()は参照違う！
変数連結も参照違う！
intern()すればプールに合わせられる！
equals()は内容だけ比較、intern()不要！
> 

✅ この感覚を押さえておけば、試験でも実務でもプロフェッショナルレベルで対応できます！