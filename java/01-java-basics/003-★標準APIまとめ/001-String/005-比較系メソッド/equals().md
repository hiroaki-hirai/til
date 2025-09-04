# equals()

`String.equals()`は、

**「2つの文字列が**完全に一致**するか（大文字小文字も区別して）比較するメソッド」**です！

Java Silver/Gold試験でも、「==との違い」や「equalsIgnoreCaseとの違い」と合わせてよく問われる超重要ポイントですね！

---

# ✅ `String.equals()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `equals(Object anObject)` |
| 戻り値の型 | `boolean`（等しければtrue、異なればfalse） |
| 主な用途 | **大文字小文字を区別して完全一致かどうかを比較**する |

---

# 🔹 シグネチャ（定義）

```java
public boolean equals(Object anObject)
```

- **引数**：比較対象（型はObject。基本はStringを渡す）
- **戻り値**：
    - `true` → 完全に一致（文字数も内容も同じ）
    - `false` → どちらかが違う

---

# 🔹 使用例（基本）

```java
String a = "OpenAI";
String b = "OpenAI";
String c = "openai";

System.out.println(a.equals(b)); // → true
System.out.println(a.equals(c)); // → false
```

✅ **大小区別あり！**

- "OpenAI"と"OpenAI" → `true`
- "OpenAI"と"openai" → `false`

---

# 🔹 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 大文字小文字は区別するか？ | ✅ 区別する |
| 完全一致だけ判定できるか？ | ✅ 文字数も内容も同じでなければfalse |
| 部分一致できるか？ | ❌ できない（部分一致はcontainsなどを使う） |
| nullを渡すと？ | ✅ 正しくfalseを返す（NullPointerExceptionにはならない） |

---

# 🔹 `==`との違い（超重要！）

| 比較項目 | `==` | `equals()` |
| --- | --- | --- |
| 比較対象 | 参照（同じオブジェクトか？）を比較する | 内容（文字列の中身）を比較する |
| 使う場面 | インスタンスが同一かどうかを確認したい場合だけ | 文字列の中身が同じかどうかを確認したい場合 |

---

## 🔸 例：「==」と「equals()」の違い

```java
String s1 = new String("hello");
String s2 = new String("hello");

System.out.println(s1 == s2);       // → false（参照が違う）
System.out.println(s1.equals(s2));  // → true（内容は同じ）
```

✅ **equals()は「中身比較」なのでtrue！**

---

# 🔹 nullに対して安全か？

```java
String s = "test";

System.out.println(s.equals(null)); // → false（例外なし）
```

✅ **equals()は引数がnullでも安全にfalseを返す！**

（逆に、null.equals(s)はNullPointerExceptionになるので注意！）

---

# 🔹 equalsIgnoreCase()との違いまとめ

| 比較項目 | equals() | equalsIgnoreCase() |
| --- | --- | --- |
| 大文字小文字区別 | ✅ 区別する | ❌ 区別しない（無視する） |
| 完全一致判定か？ | ✅ 完全一致のみ | ✅ 完全一致のみ（大小無視で） |
| 部分一致できるか？ | ❌ 不可 | ❌ 不可 |

---

# ✅ 実務での使用例

| シチュエーション | 使い方例 |
| --- | --- |
| 確実に一致しているか（大小含めて）判定したい | `a.equals(b)` |
| DB登録名と入力名の厳密照合 | `input.equals(dbValue)` |
| フラグ（ON/OFFなど）厳密比較 | `status.equals("ON")` |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 大文字小文字を区別する | ✅ 必ず区別する |
| 完全一致のみ比較できる | ✅ はい（部分一致は不可） |
| null引数に対して安全か？ | ✅ 引数がnullならfalseを返す、例外は出ない |
| 使いどころ | 中身が完全に一致しているか厳密にチェックしたいとき |

---

# ✨ 超シンプルまとめ

> equals()は「中身が完全一致しているか」を大文字小文字を区別して判定！
==とは違う！部分一致もできない！
null引数にも安全！
> 

✅ この感覚を押さえれば、試験でも実務でもバッチリ対応できます！

---

# 🔺 `String.equals()` 専用引っかけ問題集（全6問）

---

### ❓ 問題 1：基本的な完全一致比較

```java
String a = "Java";
String b = "Java";

System.out.println(a.equals(b));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- **中身が完全一致している**ので、equals()は`true`！

✅ 参照（==）じゃなく、**内容**を見る！

---

### ❓ 問題 2：大文字小文字の違いに注意

```java
String a = "Java";
String b = "java";

System.out.println(a.equals(b));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- equals()は**大文字小文字を区別する**ので"J"と"j"は違う！

✅ equals()は必ず大小区別する！

---

### ❓ 問題 3：==との違い（参照比較）

```java
String a = new String("Hello");
String b = new String("Hello");

System.out.println(a == b);
System.out.println(a.equals(b));
```

この出力は？

A. `true`, `true`

B. `false`, `true`

C. `true`, `false`

D. `false`, `false`

**答え**：B. `false`, `true`

**解説**：

- `==`は**参照比較**だから`false`（違うnewインスタンス）。
- `equals()`は**内容比較**だから`true`！

✅ **==とequals()は違う！**

---

### ❓ 問題 4：部分一致はできるか？

```java
String s = "Programming";

System.out.println(s.equals("Program"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- equals()は**完全一致のみ**！
- 部分一致したいならcontains()を使う！

✅ equals()は部分一致できない！

---

### ❓ 問題 5：null引数の場合

```java
String s = "example";

System.out.println(s.equals(null));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- equals()にnullを渡すと、単に`false`を返すだけ。
- **NullPointerExceptionにはならない！**

✅ 安全に使える！

---

### ❓ 問題 6：呼び出し側がnullの場合

```java
String s = null;

System.out.println(s.equals("test"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- 呼び出し元がnullだと**NullPointerExceptionが発生**！

✅ **null.equals(～)** の形はNG！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| equals()は部分一致できる？ | ❌ 完全一致しか判定できない |
| 大文字小文字を無視して比較できる？ | ❌ 区別する（無視しない） |
| null引数でも例外が出る？ | ❌ 引数がnullならfalseを返すだけ |
| 呼び出し側がnullなら大丈夫？ | ❌ 呼び出し元がnullだと例外（NullPointerException） |

---

# ✨ 超シンプルまとめ

> equals()は「内容が完全一致しているか」を大文字小文字を区別して判定！
==とは違う！部分一致できない！呼び出し元nullは例外！
> 

✅ この感覚を押さえておけば試験でも実務でも安心です！

以下に、**試験でも実務でも超重要な「==とequals()を意図的に混ぜた引っかけ演習問題集（全6問）」** をご用意しました！

特に、

- **参照比較 vs 内容比較**
- **リテラルとnew String()の違い**
- **文字列プールの仕組み**
を意識して引っかけを仕込んでいます！

---

# 🔺 `==`と`equals()` 引っかけ演習問題集（全6問）

---

### ❓ 問題 1：リテラル同士の比較

```java
String a = "hello";
String b = "hello";

System.out.println(a == b);
System.out.println(a.equals(b));
```

この出力は？

A. `true`, `true`

B. `false`, `true`

C. `true`, `false`

D. `false`, `false`

**答え**：A. `true`, `true`

**解説**：

- リテラル同士は**文字列プール**に入り、参照も同じ！
- 中身も同じなのでequals()も`true`。

✅ **リテラルなら==でもtrueになることがある！**

---

### ❓ 問題 2：new String()同士の比較

```java
String a = new String("java");
String b = new String("java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

この出力は？

A. `true`, `true`

B. `false`, `true`

C. `true`, `false`

D. `false`, `false`

**答え**：B. `false`, `true`

**解説**：

- newを使うと**毎回違うインスタンス**なので参照は違う（==はfalse）。
- 中身は同じなのでequals()は`true`。

✅ **new String()は必ず別インスタンス！**

---

### ❓ 問題 3：リテラルとnew String()比較

```java
String a = "OpenAI";
String b = new String("OpenAI");

System.out.println(a == b);
System.out.println(a.equals(b));
```

この出力は？

A. `true`, `true`

B. `false`, `true`

C. `true`, `false`

D. `false`, `false`

**答え**：B. `false`, `true`

**解説**：

- 参照は違うので==はfalse。
- 内容は同じなのでequals()はtrue。

✅ **リテラルとnew String()は参照が異なる！**

---

### ❓ 問題 4：内容が違うとき

```java
String a = "Java";
String b = "JAVA";

System.out.println(a == b);
System.out.println(a.equals(b));
```

この出力は？

A. `true`, `true`

B. `false`, `true`

C. `false`, `false`

D. `true`, `false`

**答え**：C. `false`, `false`

**解説**：

- 文字列リテラルでも**内容が違えば別物**（プールに入ってても違う！）
- 大文字小文字が違うのでequals()もfalse。

✅ **equals()は大小区別する！**

---

### ❓ 問題 5：intern()を使った場合

```java
String a = new String("test").intern();
String b = "test";

System.out.println(a == b);
System.out.println(a.equals(b));

```

この出力は？

A. `true`, `true`

B. `false`, `true`

C. `true`, `false`

D. `false`, `false`

**答え**：A. `true`, `true`

**解説**：

- intern()すると、文字列プール内のインスタンスを使う。
- リテラルと同じ参照になるので==もtrue！

✅ **intern()でリテラルと参照を揃えられる！**

---

### ❓ 問題 6：nullとの比較

```java
String s = null;
String t = "value";

System.out.println(t.equals(s));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- equals()にnullを渡すのは安全。
- 内容が違うのでfalseを返すだけ。

✅ **呼び出し元（左側）がnullだとダメ、引数（右側）がnullならfalseで済む！**

---

# ✅ まとめ表（超重要比較まとめ）

| 比較項目 | == | equals() |
| --- | --- | --- |
| 何を比較する？ | 参照（アドレス）を比較する | 内容（文字列中身）を比較する |
| リテラル同士 | trueになることが多い | true（内容同じなら） |
| new String()同士 | false（毎回別インスタンス） | true（内容同じなら） |
| 大文字小文字無視できる？ | できない | できない（equalsIgnoreCaseは別） |
| null対象の安全性 | null == ～ はOK | s.equals(null)はOK |
| 呼び出し元がnullの場合 | OK（null == ～） | NG（NullPointerException） |

---

# ✨ 超シンプルまとめ

> ==は参照比較、equals()は内容比較！
new String()は別インスタンス、intern()でプールに戻せる！
equals()は大小区別するし完全一致だけ判定！
> 

✅ この感覚を押さえておけば、試験でも実務でも完全攻略できます！

ここでは、**Java Silver/Gold試験でも非常によく問われる「== / equals() / equalsIgnoreCase()クロス演習問題集（全8問）」** をご用意しました！

**参照比較・内容比較・大文字小文字無視比較の違い**を正確に区別できるようになるのが目標です！

---

# 🔺 `== / equals() / equalsIgnoreCase()` クロス演習問題集（全8問）

---

### ❓ 問題 1：リテラル同士の比較

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));
```

出力結果は？

A. `true`, `true`, `true`

B. `false`, `true`, `true`

C. `true`, `false`, `true`

D. `false`, `false`, `false`

**答え**：A. `true`, `true`, `true`

**解説**：

- リテラル同士なので==はtrue（文字列プール内で同一参照）。
- 内容も一致。
- equalsIgnoreCase()も一致。

---

### ❓ 問題 2：new String()同士の比較

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));
```

**答え**：B. `false`, `true`, `true`

**解説**：

- newは別インスタンスなので==はfalse。
- equals()/equalsIgnoreCase()は内容比較なのでtrue。

---

### ❓ 問題 3：大文字小文字違いの比較

```java
String a = "Java";
String b = "JAVA";

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));
```

**答え**：D. `false`, `false`, `true`

**解説**：

- ==はfalse（リテラルでも内容違えば別）。
- equals()は大文字小文字区別するのでfalse。
- equalsIgnoreCase()は無視するのでtrue！

---

### ❓ 問題 4：リテラルとnew String()＋大文字違い

```java
String a = "Hello";
String b = new String("HELLO");

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));
```

**答え**：D. `false`, `false`, `true`

**解説**：

- newなので参照は違う（==はfalse）。
- equals()は大文字小文字区別するのでfalse。
- equalsIgnoreCase()ならtrue。

---

### ❓ 問題 5：異なる文字列同士の比較

```java
String a = "Spring";
String b = "Summer";

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));
```

**答え**：D. `false`, `false`, `false`

**解説**：

- 内容が全く違うので全部false。

✅ equalsIgnoreCase()でも内容が違えばfalse！

---

### ❓ 問題 6：intern()を使った場合

```java
String a = new String("cloud").intern();
String b = "cloud";

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));
```

**答え**：A. `true`, `true`, `true`

**解説**：

- intern()でプールに戻したので==もtrue。
- 内容も一致。

---

### ❓ 問題 7：nullを引数にする場合（呼び出し元は非null）

```java
String a = "test";

System.out.println(a.equals(null));
System.out.println(a.equalsIgnoreCase(null));
```

**答え**：D. `false`, `false`

**解説**：

- equals()もequalsIgnoreCase()も、**引数がnullならfalse**を返す（例外は出ない）。

---

### ❓ 問題 8：null呼び出し元の場合（呼び出し元がnull）

```java
String a = null;
String b = "test";

System.out.println(a == b);
System.out.println(a.equals(b));
```

**答え**：B. `false`, 実行時例外（NullPointerException）

**解説**：

- ==はnull比較できるのでfalse。
- equals()は呼び出し元がnullなので例外発生！

✅ 呼び出し元がnullかどうかは要注意！

---

# ✅ 超まとめ表

| 比較パターン | == | equals() | equalsIgnoreCase() |
| --- | --- | --- | --- |
| 参照（同じオブジェクト）比較するか？ | ✅ はい | ❌ 内容だけ比較 | ❌ 内容だけ比較 |
| 内容（文字列の中身）を比較するか？ | ❌ 参照だけ | ✅ 内容を比較 | ✅ 内容を比較（大文字小文字無視） |
| 大文字小文字を無視するか？ | ❌ 区別する | ❌ 区別する | ✅ 区別しない |
| null引数を許容するか？ | ✅ 比較できる（null == ～OK） | ✅ 引数nullならfalse | ✅ 引数nullならfalse |
| 呼び出し元がnullの場合 | ✅ OK（null == ～OK） | ❌ NullPointerException発生 | ❌ NullPointerException発生 |

---

# ✨ 超シンプルまとめ

> ==は参照比較、equalsは内容比較（区別あり）、equalsIgnoreCaseは内容比較（区別なし）！
nullの扱いに注意、intern()で==がtrueになるケースもあり！
> 

✅ この感覚を押さえておけば、試験でも実務でも完全攻略できます！

---

# 🔺 `== / equals() / equalsIgnoreCase()` 超トリッキー演習問題集（全8問）

---

### ❓ 問題 1：new String()＋リテラル比較

```java
String a = "java";
String b = new String("java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

**答え**：`false`, `true`

**解説**：

- new String()は**常に別インスタンス**なので==はfalse。
- 中身は同じなのでequals()はtrue。

---

### ❓ 問題 2：文字列連結（＋演算子）を使った場合

```java
String a = "hello";
String b = "he" + "llo";

System.out.println(a == b);

```

**答え**：`true`

**解説**：

- 定数同士の＋演算子は**コンパイル時に連結される**！
- 結果的にリテラル同士になり、同じ文字列プール参照になる！

---

### ❓ 問題 3：文字列連結（変数使用）を使った場合

```java
String part = "he";
String a = "hello";
String b = part + "llo";

System.out.println(a == b);
```

**答え**：`false`

**解説**：

- 変数を使うと**実行時連結**になるため、プールされず異なる参照になる！

✅ 定数＋定数ならtrue、変数絡むとfalse！

---

### ❓ 問題 4：文字列連結＋intern()

```java
String part = "he";
String a = "hello";
String b = (part + "llo").intern();

System.out.println(a == b);

```

**答え**：`true`

**解説**：

- intern()で**プールのhelloと一致させる**ので==はtrue！

✅ intern()を使えば変数連結でも参照を合わせられる！

---

### ❓ 問題 5：equalsIgnoreCase()で内容違い比較

```java
String a = "Java";
String b = "JavaScript";

System.out.println(a.equalsIgnoreCase(b));

```

**答え**：`false`

**解説**：

- equalsIgnoreCase()は**完全一致のみ**判定する。
- "Java"と"JavaScript"は違うのでfalse！

✅ 部分一致はできない！

---

### ❓ 問題 6：呼び出し元がnullの場合

```java
String a = null;
String b = "test";

System.out.println(a == b);
System.out.println(a.equals(b));
```

**答え**：`false`, 実行時例外（NullPointerException）

**解説**：

- ==はnull比較OK、false。
- equals()は呼び出し元がnullなので例外発生！

---

### ❓ 問題 7：引数がnullの場合

```java
String a = "test";

System.out.println(a.equals(null));
System.out.println(a.equalsIgnoreCase(null));
```

**答え**：`false`, `false`

**解説**：

- equals()もequalsIgnoreCase()も**引数がnullなら安全にfalseを返す**！

---

### ❓ 問題 8：意図的に騙すnew＋intern比較

```java
String a = new String("data").intern();
String b = "data";

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a.equalsIgnoreCase(b));

```

**答え**：`true`, `true`, `true`

**解説**：

- intern()でプール参照になっているので==もtrue。
- 内容も同じ、大小無視比較でも同じ！

✅ new String()でもintern()をかければリテラルと一致できる！

---

# ✅ 難問まとめ表

| 特徴 | ポイント |
| --- | --- |
| new String() vs リテラル | 参照が違う（==はfalse） |
| 定数＋定数の連結 | コンパイル時連結→リテラル化され==true |
| 変数を使った連結 | 実行時連結→==false（intern()すれば==trueにできる） |
| equalsIgnoreCase()で部分一致 | ❌ できない（完全一致のみ） |
| nullとの比較 | 呼び出し元nullは例外、引数nullならfalseで安全 |

---

# ✨ 超シンプルまとめ

> new String()に注意！文字列プールの仕組みを意識！
equals()は中身比較、==は参照比較、equalsIgnoreCase()は完全一致のみ！
nullの位置に注意して安全設計！
> 

✅ この感覚を押さえれば、**本番試験でも即答できるプロレベル**です！

以下に、**「== / equals() / equalsIgnoreCase()を活用した実務応用版シミュレーション問題集（ログイン認証・ファイル名検査編）」** をご用意しました！

内容は、**ただのメソッド問題ではなく、実務でよくある場面設定をもとに考えるトレーニング**になっています。

実践的なコードの読み取り＋正しい比較方法の判断力が問われます！

---

# 🔺 実務応用版シミュレーション問題集（ログイン認証・ファイル名検査編）

---

## 【ログイン認証シミュレーション】

---

### ❓ 問題 1：ログインIDとパスワードを比較（基本）

```java
String inputId = "Admin";
String storedId = "admin";

String inputPass = "OpenAI123";
String storedPass = "OpenAI123";

boolean isAuthenticated = 
    inputId.equals(storedId) && inputPass.equals(storedPass);

System.out.println(isAuthenticated);
```

出力結果は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：B. `false`

**解説**：

- ID比較が**大小区別される**ため不一致になる。
- PasswordはOKでもIDがfalseなら認証失敗！

✅ **ID比較はequalsIgnoreCase()を使うべきだった！**

---

### ❓ 問題 2：正しい大小無視認証の書き直し

次のうち「IDだけ大小無視で認証する」正しい書き直しは？

A.

```java
inputId.equalsIgnoreCase(storedId) && inputPass.equals(storedPass)
```

---

**答え**：A.

**解説**：

- IDはequalsIgnoreCase()（大小無視）！
- Passwordはequals()（大小区別、完全一致）！

✅ **IDは許容、パスワードは厳格、が一般的な設計！**

---

### ❓ 問題 3：null安全設計

次のコードには危険な箇所があります。どこでしょう？

```java
String input = null;
String stored = "admin";

if (input.equalsIgnoreCase(stored)) {
    System.out.println("一致しました");
}
```

A. 問題なし

B. NullPointerExceptionが発生する

C. 実行時にtrueになる

D. equalsIgnoreCaseはnullでも大丈夫

---

**答え**：B. NullPointerExceptionが発生する

**解説**：

- 呼び出し元（input）がnullだと**例外発生**！

✅ **nullチェックを必ず先に行う！**

---

---

## 【ファイル名検査シミュレーション】

---

### ❓ 問題 4：拡張子検査（基本）

```java
String filename = "document.PDF";

boolean isPdf = filename.endsWith(".pdf");

System.out.println(isPdf);
```

出力結果は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：B. `false`

**解説**：

- endsWith()は**大文字小文字を区別**する！
- ".PDF"は".pdf"と違うのでfalse！

✅ **拡張子検査は大文字小文字無視できるように対策が必要！**

---

### ❓ 問題 5：正しい拡張子検査（大小無視版）

次のうち「拡張子検査を大小無視で正しくする」コードは？

C.

```java
filename.toLowerCase().endsWith(".pdf")
```

**答え**：C.

**解説**：

- ファイル名を小文字化してから、endsWith(".pdf")で比較する！

✅ **toLowerCase()＋endsWith()が実務標準！**

---

### ❓ 問題 6：ファイル名フィルタリング

リストから`.pdf`拡張子のファイルだけ抽出する正しいコードは？

```java
List<String> files = List.of("doc1.PDF", "doc2.pdf", "image.JPG");

List<String> pdfs = files.stream()
    .filter(f -> ???)
    .collect(Collectors.toList());
```

??? に入るコードは？

A. `f.endsWith(".pdf")`

B. `f.toLowerCase().endsWith(".pdf")`

C. `f.equalsIgnoreCase(".pdf")`

D. `f.toUpperCase().startsWith(".PDF")`

---

**答え**：B.

**解説**：

- 小文字化してからendsWith(".pdf")で正しく抽出できる！

✅ **リストフィルタリングでも大小無視対応は必須！**

---

# ✅ 難易度アップまとめ

| シチュエーション | 正しい比較方法 |
| --- | --- |
| ログインID比較 | equalsIgnoreCase()（大小無視） |
| パスワード比較 | equals()（大小区別・完全一致） |
| ファイル拡張子検査 | toLowerCase()＋endsWith(".拡張子") |
| null対象との比較 | nullチェック後にequals()やequalsIgnoreCase()を使う |

---

# ✨ 超シンプルまとめ

> ログインID、パスワード、ファイル名など、比較対象によってequals/equalsIgnoreCaseを使い分ける！
nullの安全設計も忘れずに！
endsWith()は大小区別ありなので注意！
> 

✅ この感覚を押さえれば、試験・実務どちらでも即戦力です！

以下に、**equals() / equalsIgnoreCase() / toLowerCase()をすべてミックスした最高難易度演習セット（全8問）** をご用意しました！

このセットでは、

**「どのメソッドを使うべきか？」「null対応は？」「完全一致 or 部分一致？」** を一問一問慎重に考える必要があります！

---

# 🔺 equals() / equalsIgnoreCase() / toLowerCase() ミックス応用セット（最高難易度版）

---

## 【超実務的ケースで出題】

---

### ❓ 問題 1：大小無視＋完全一致が求められる場合

```java
String input = "Admin";
String stored = "admin";

System.out.println(input.equals(stored));
System.out.println(input.equalsIgnoreCase(stored));
```

出力は？

A. `true`, `true`

B. `false`, `true`

C. `true`, `false`

D. `false`, `false`

---

**答え**：B. `false`, `true`

**解説**：

- equals()は大小を区別するためfalse。
- equalsIgnoreCase()は大小無視するのでtrue！

---

### ❓ 問題 2：部分一致（含む）を大小無視でしたい

```java
String text = "WelcomeToJava";

System.out.println(text.toLowerCase().contains("java"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：A. `true`

**解説**：

- 小文字化してから"java"を探すので部分一致できる！

✅ equals() / equalsIgnoreCase()では部分一致できない！

---

### ❓ 問題 3：拡張子検査（大小無視）

```java
String fileName = "document.PDF";

System.out.println(fileName.toLowerCase().endsWith(".pdf"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：A. `true`

**解説**：

- 小文字化してからendsWithで拡張子をチェック！

✅ **toLowerCase()＋endsWith()**は実務必須テク！

---

### ❓ 問題 4：ログイン認証処理（null混入版）

```java
String id = null;
String pass = "password";

boolean isAuth = 
    id != null && id.equalsIgnoreCase("admin") && 
    pass.equals("password");

System.out.println(isAuth);
```

出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：B. `false`

**解説**：

- idがnullなので、id != nullの時点でfalse判定。

✅ nullチェックを先に書けば安全！

---

### ❓ 問題 5：空白文字を無視できるか？

```java
String input = " Admin ";
String stored = "admin";

boolean result = input.equalsIgnoreCase(stored);
System.out.println(result);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：B. `false`

**解説**：

- equalsIgnoreCase()は空白を無視しない！
- 内容そのものを大小無視で比較するだけ。

✅ **空白無視にはtrim()が必要！**

---

### ❓ 問題 6：空白を無視して大小無視比較する方法は？

正しいコードは？

A,

```java
input.trim().equalsIgnoreCase(stored)
```

**答え**：A.

**解説**：

- **trim()で空白除去→equalsIgnoreCase()で大小無視**が正解！

---

### ❓ 問題 7：部分一致で拡張子を検査するコード

```java
String file = "archive.PDF";

boolean check = file.toLowerCase().contains(".pdf");
System.out.println(check);
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

---

**答え**：A. `true`

**解説**：

- 小文字化して".pdf"が含まれているかを見ている！

✅ **contains()は部分一致検査！**

---

### ❓ 問題 8：equalsIgnoreCase()とtoLowerCase()＋equals()の違いを問う

次の2つは常に同じ結果になるか？

```java
a.equalsIgnoreCase(b)
a.toLowerCase().equals(b.toLowerCase())
```

A. 常に同じ結果になる

B. たまに異なる結果になる

C. 常に異なる結果になる

D. コンパイルエラー

---

**答え**：A. 常に同じ結果になる

**解説**：

- equalsIgnoreCase()と、toLowerCase()後のequals()は**理論上同じ動作**！

✅ 両方「大文字小文字無視で完全一致比較」しているだけ！

---

# ✅ 難易度アップまとめ

| シチュエーション | 正しい方法 |
| --- | --- |
| 完全一致（大文字小文字区別あり） | equals() |
| 完全一致（大文字小文字無視） | equalsIgnoreCase() |
| 部分一致（含む、始まる、終わる） | toLowerCase()＋contains()/startsWith()/endsWith() |
| 空白無視＋大小無視 | trim()＋equalsIgnoreCase() |
| null対策 | nullチェック必須（呼び出し側nullは例外） |

---

# ✨ 超シンプルまとめ

> 比較は「何を無視するか（大小？空白？）」に応じて使い分け！
完全一致ならequals()/equalsIgnoreCase()、
部分一致ならtoLowerCase()＋contains()系！
null対策は絶対に忘れずに！
> 

✅ ここまで押さえれば、試験も実務も**トップレベルの比較設計力**が身につきます！