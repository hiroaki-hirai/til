# equalsIgnoreCase()

`String.equalsIgnoreCase()` は、

**「大文字小文字を無視して、2つの文字列が等しいかどうかを比較するメソッド」**です！

Java Silver/Gold試験でも、文字列比較問題で必ず押さえておきたい基本メソッドですね！

---

# ✅ `String.equalsIgnoreCase()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `equalsIgnoreCase(String anotherString)` |
| 戻り値の型 | `boolean`（等しければtrue、異なればfalse） |
| 主な用途 | **大文字小文字を無視**して文字列が等しいか比較する |

---

# 🔹 シグネチャ（定義）

```java
public boolean equalsIgnoreCase(String anotherString)
```

- **引数**：比較対象の文字列
- **戻り値**：
    - `true` → 大文字小文字無視で等しい
    - `false` → 等しくない

---

# 🔹 使用例（基本）

```java
String a = "Java";
String b = "java";

System.out.println(a.equalsIgnoreCase(b)); // → true
```

✅ "Java"と"java"は大文字小文字以外同じなので、**true**！

---

# 🔹 注意ポイントまとめ

| 項目 | 内容 |
| --- | --- |
| 大文字小文字を区別する？ | ❌ 区別しない（無視する） |
| 完全一致が必要？ | ✅ 文字数・内容が完全一致している必要あり |
| 部分一致できる？ | ❌ できない（**equals**系なので完全一致のみ） |
| nullを渡すと？ | ❌ NullPointerExceptionが発生する（null安全でない） |

---

# 🔹 よくある用途例

- **ログインIDやユーザー名の照合（大文字小文字を無視）**
- **入力データとDB登録データの比較**
- **コマンド文字列の判定（例えば "EXIT" コマンドとか）**

✅ **大小を無視した「完全一致」比較にはとても便利！**

---

# 🔸 `equals()`との違いまとめ

| 比較項目 | equals() | equalsIgnoreCase() |
| --- | --- | --- |
| 大文字小文字を区別？ | ✅ 区別する | ❌ 区別しない（無視する） |
| 完全一致が必要？ | ✅ 必要 | ✅ 必要 |
| 部分一致できる？ | ❌ できない（完全一致だけ） | ❌ 同じくできない（完全一致だけ） |

---

# 🔹 equalsIgnoreCase()の弱点

- **部分一致や前方一致などはできない！**（完全一致専用）
- **nullを渡すと例外が発生する！**（nullチェック必須）

---

# ✅ equalsIgnoreCase()のnull対策例

```java
String a = null;

if (a != null && a.equalsIgnoreCase("test")) {
    System.out.println("一致しました");
}
```

✅ **nullチェック＋equalsIgnoreCase()が鉄則**！

---

# 🔹 典型的な実務パターン例

| シチュエーション | 使い方例 |
| --- | --- |
| ログインIDの比較（大小無視） | `inputId.equalsIgnoreCase(storedId)` |
| コマンド入力の比較（大小無視） | `command.equalsIgnoreCase("EXIT")` |
| 固定値との比較（大小無視） | `status.equalsIgnoreCase("SUCCESS")` |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 変換対象 | アルファベット（A～Z, a～z）の大文字小文字を無視 |
| 元の文字列 | 変更しない（immutable） |
| 数字・記号・漢字 | 大小無視比較には影響しない（そのまま比較される） |
| 完全一致だけ判定できる？ | ✅ はい（部分一致できない） |
| null安全設計が必要？ | ✅ 必要（nullチェックしないと例外が発生する） |

---

# ✨ 超シンプルまとめ

> equalsIgnoreCase()は「完全一致」を大文字小文字無視で判定する！
部分一致には使えない！
null安全設計が必須！
> 

✅ この感覚を押さえておけば、試験も実務もバッチリ対応できます！

---

# 🔺 `equalsIgnoreCase()` 引っかけ問題集（全6問）

---

### ❓ 問題 1：基本的な大小無視の比較

```java
String a = "OpenAI";
String b = "openai";

System.out.println(a.equalsIgnoreCase(b));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 大文字小文字を無視して比較するため、内容は一致する！

✅ **equalsIgnoreCase()は大小無視で完全一致比較！**

---

### ❓ 問題 2：部分一致できるか？

```java
String s = "Java Programming";

System.out.println(s.equalsIgnoreCase("java"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- **equalsIgnoreCase()は部分一致できない！**
- 文字列全体が一致しないと`true`にならない。

✅ 「含まれている」かどうかは**contains()を使うべき**！

---

### ❓ 問題 3：nullを対象にすると？

```java
String s = null;

System.out.println(s.equalsIgnoreCase("test"));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- nullでメソッド呼び出しをすると**NullPointerException**が発生する！

✅ **nullチェックを先にする必要がある！**

---

### ❓ 問題 4：記号や数字を含んだ文字列比較

```java
String s1 = "test123";
String s2 = "TEST123";

System.out.println(s1.equalsIgnoreCase(s2));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- アルファベットの大文字小文字だけが無視される。
- 数字や記号はそのまま比較されるが、両方同じなので`true`！

✅ **数字・記号は変換対象外（そのまま比較）！**

---

### ❓ 問題 5：" "（空白だけ）の比較

```java
String a = " ";
String b = " ";

System.out.println(a.equalsIgnoreCase(b));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 空白同士は同じなので大小無視比較でも一致する！

✅ 文字の内容が同じなら空白でも`true`！

---

### ❓ 問題 6：前後空白がある場合の注意

```java
String input = " admin ";
String db = "ADMIN";

System.out.println(input.equalsIgnoreCase(db));
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- 大文字小文字は無視できるが、**前後の空白は無視できない！**
- 内容が違うので`false`になる。

✅ **空白を無視したいならtrim()などの加工が必要！**

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| equalsIgnoreCase()は部分一致できる？ | ❌ できない（**完全一致のみ**） |
| null対象でも安全？ | ❌ NullPointerExceptionが発生する |
| 記号や数字も大文字小文字無視する？ | ❌ 記号や数字は変換対象外。大小無視はアルファベットのみ。 |
| 空白も無視してくれる？ | ❌ 空白や文字数の違いはそのまま比較対象になる（無視されない） |

---

# ✨ 超シンプルまとめ

> equalsIgnoreCase()は「完全一致」を大文字小文字無視で判定するだけ！
部分一致できない！null注意！空白も注意！
> 

✅ この感覚を押さえておけば、試験でも実務でも安心です！

---

# ✅ `equalsIgnoreCase()` が向かないケース（例外パターンまとめ）

---

## 🔹 1. **部分一致検索（contains / startsWith / endsWith）をしたいとき**

### ◆ 問題

- `equalsIgnoreCase()`は「完全一致」しか判定できない！
- 「含まれているか？」「で始まるか？」「で終わるか？」はチェックできない。

### ◆ 例

```java
String title = "Introduction to Java";

if (title.equalsIgnoreCase("Java")) {
    // → false！（完全一致じゃないから）
}
```

**でも本当は「Javaが含まれているか」を知りたい！**

---

### ◆ 解決策

➡ **toLowerCase()やtoUpperCase()を使って加工してからcontains()などでチェック**

```java
if (title.toLowerCase().contains("java")) {
    System.out.println("含まれています！");
}
```

✅ **部分一致ならequalsIgnoreCase()ではなく、toLowerCase()/toUpperCase()＋contains()系！**

---

## 🔹 2. **比較前に文字列を整形（trim/replaceなど）したいとき**

### ◆ 問題

- `equalsIgnoreCase()`は「文字列そのまま」を比較するだけ。
- 不要な空白や記号が入っていたら想定通り動かない。

### ◆ 例

```java
String input = " admin ";
String dbValue = "ADMIN";

if (input.equalsIgnoreCase(dbValue)) {
    // → false！（前後に空白があるため）
}
```

---

### ◆ 解決策

➡ **まずtrim()で整形してからtoLowerCase()/toUpperCase()＋equals()で比較**

```java
if (input.trim().toUpperCase().equals(dbValue.toUpperCase())) {
    System.out.println("一致しました！");
}
```

✅ **比較前に加工したいならequalsIgnoreCase()単体では不十分！**

---

## 🔹 3. **複数条件を組み合わせた比較をしたいとき**

### ◆ 問題

- 「大文字小文字を無視」＋「前後一致のみ」など、複合条件でフィルタリングしたい場合、
- `equalsIgnoreCase()`だけでは柔軟な制御ができない。

### ◆ 例

- 「ログインIDが"admin"で、パスワードが"password"を含むか確認したい」

```java
if (id.equalsIgnoreCase("admin") && password.contains("password")) {
    // OKだけど、大小無視でpasswordも比較したい場合は困る！
}
```

---

### ◆ 解決策

➡ **必要に応じて各フィールドをtoLowerCase()やtoUpperCase()してから細かく比較する**

```java
if (id.toLowerCase().equals("admin") && password.toLowerCase().contains("password")) {
    System.out.println("認証OK");
}
```

✅ **柔軟な比較・組み合わせにはtoLowerCase()/toUpperCase()＋個別判定が必要！**

---

## 🔹 4. **nullが許容される場合**

### ◆ 問題

- `equalsIgnoreCase()`はnull対象に対して使うと即座にNullPointerException発生！
- 例外を防ぐためには**必ずnullチェック**が必要。

### ◆ 例

```java
String a = null;

if (a.equalsIgnoreCase("test")) { // → NullPointerException発生！
    System.out.println("一致");
}
```

---

### ◆ 解決策

➡ **nullチェックを先に行う！**

```java
if (a != null && a.equalsIgnoreCase("test")) {
    System.out.println("一致");
}
```

✅ **nullが混じる可能性がある場合、equalsIgnoreCase()単体では使わない！**

---

# ✅ まとめ表

| ケース | equalsIgnoreCase()は向かない理由 | 推奨代替パターン |
| --- | --- | --- |
| 部分一致（contains, startsWith, endsWith） | 完全一致しかできない | toLowerCase()/toUpperCase()＋contains()など |
| 整形（trim, replaceなど）後に比較したい | 比較前の加工ができない | 加工＋toLowerCase()/toUpperCase()＋equals() |
| 複合条件フィルタリング | 細かい比較制御ができない | 必要に応じて個別加工して比較 |
| null値が混じる場合 | NullPointerException発生のリスクがある | nullチェック＋比較 |

---

# ✨ 超シンプルまとめ

> equalsIgnoreCase()は「単純な完全一致比較」専用！
部分一致・加工込み・柔軟なフィルタリングには向かない！
null対象には特に注意が必要！
> 

✅ この感覚を持っていれば、試験でも実務でもバッチリ対策できます！