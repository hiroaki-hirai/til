# hachcode と null

# Q,

初期化されていないStringなどの参照クラス型の変数を条件式にとるswitch文を実行すると、switch文がそれぞれのcaseをhashcodeメソッドにて特定する際に、変数を初期化(nullが代入)し、その変数からhashcodeメソッドにアクセスすることから、NullPointerException の例外が発生する認識はあっていますか？

はい、Hiroakiさんのご認識は**概ね正しい**です。以下のように整理して説明します。

---

### 🔸【結論】

`switch`文の式に **null の参照型変数（例：String）** を渡すと、**`NullPointerException` が発生します**。

---

### 🔸【発生の理由】

```java
String value = null;

switch (value) {
    case "A":
        System.out.println("A");
        break;
    case "B":
        System.out.println("B");
        break;
}
```

このコードは **コンパイルは通ります** が、実行時に以下のような `NullPointerException` が発生します。

```java
Exception in thread "main" java.lang.NullPointerException
	at ...
```

これは以下のような理由によります：

- Javaの`switch`文は、`String`を条件に使う場合（Java 7以降）、内部的に `String.hashCode()` などを使って高速にマッチングを行うように最適化されています。
- そのため、`null` を `switch` に渡すと、暗黙的に `null.hashCode()` を呼び出そうとして **`NullPointerException`** になります。

---

### 🔸【補足：この仕様の背景】

- `switch (String)` の裏では、各 `case` のラベルの `hashCode()` を事前に計算し、それと比較するような仕組み（効率的な分岐）を取ります。
- `null` に対して `hashCode()` を呼ぶことはできないため、必ず例外となります。

---

### ✅【対策】

`switch` に入れる前に、`null` チェックをしておく必要があります：

```java
if (value != null) {
    switch (value) {
        case "A":
            System.out.println("A");
            break;
        case "B":
            System.out.println("B");
            break;
    }
} else {
    System.out.println("value is null");
}
```

---

### 🔸まとめ

| 項目 | 説明 |
| --- | --- |
| `switch(String)`の構文 | Java 7以降、使用可能 |
| `null` を渡すとどうなる？ | 実行時に `NullPointerException` |
| 原因 | `hashCode()` にアクセスしようとするため |
| 対策 | `switch` の前に `null` チェックを入れる |

---

ご認識を補足すると、**「switch文がhashCodeを用いてcaseを特定する際にnullにアクセスしてしまう」ために例外が発生する**という理解は正しいです！

# Q,

```java
static String strl;

switch (str) {
    case "A":
        System.out.println("A");
   default : str += "abc";
    case "B":
        System.out.println("B");
}
```

switch文にてインスタンス変数が初期化され初期値のnull が代入される場合にはどのようなものがありますか？

とても良い質問です。以下で順を追って丁寧に説明します。

---

## ✅ 質問の要点の整理

```java
static String str;
```

上記のように宣言された `str` は **初期化されていないように見えます** が、Javaでは：

- **参照型のフィールド（static/instance）は暗黙的に `null` に初期化** されます。
- したがって、この `str` の初期値は明示しなくても `null` です。

---

## 🔸このコードの動作と例外

```java
static String str;

public static void main(String[] args) {
    switch (str) {  // strはnull
        case "A":
            System.out.println("A");
        default:
            str += "abc"; // ここで NullPointerException
        case "B":
            System.out.println("B");
    }
}
```

### ✅ 動作とエラーの原因

- `switch(str)` に渡される `str` は `null`
- そのため、**実行時に `str.hashCode()` を呼び出そうとして `NullPointerException` が発生します**
- よってこのコードは実行すると **`switch` の最初で落ちます**

---

## 🔸質問の核心：「どのような場合に null が代入されるか？」

以下のような参照型変数は、明示的に初期化しなくても **自動的に `null` になります**。

| 宣言の種類 | 例 | 初期値 |
| --- | --- | --- |
| static フィールド | `static String str;` | `null` |
| インスタンスフィールド | `String name;` | `null` |
| 配列の要素（参照型） | `String[] arr = new String[5];` | 各要素が `null` |

### ❌ ただし **ローカル変数** は対象外！

```java
public static void main(String[] args) {
    String s;
    // System.out.println(s); // エラー: 初期化されていない
}
```

- ローカル変数（メソッド内で定義された変数）は **コンパイラが明示的な初期化を強制**します。
- 初期化しないまま使用すると **コンパイルエラー** になります。

---

## 🔸まとめ

| 状況 | 初期値（代入） | 補足 |
| --- | --- | --- |
| `static String s;` | 自動で `null` | 明示的に `= null` は不要 |
| `String s;`（インスタンス変数） | 自動で `null` | 同上 |
| `String[] arr = new String[5];` | 各要素が `null` | 配列の各要素も自動初期化対象 |
| `String s;`（ローカル変数） | ❌ 自動で初期化されない | 使用前に必ず明示的に代入する必要 |

---

必要なら、`null`が代入されるメモリ領域（ヒープかスタックか）や、JVMの初期化処理の観点からも説明できます。興味があればお知らせください。