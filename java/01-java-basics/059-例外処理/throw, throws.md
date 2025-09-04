# throw, throws

# Q,

例外処理の throw と throws について解説してください。

はい、Javaにおける `throw` と `throws` は、**例外処理**に関連するキーワードですが、役割が異なります。以下にそれぞれの違いと使い方を解説します。

---

## 🔹 `throw`：**例外をスロー（投げる）する**

### 目的：

処理中に**実際に例外を発生させる**ために使用します。

### 構文：

```java
throw new ExceptionType("エラーメッセージ");
```

### ポイント：

- `throw` の後には、**Throwable型（例外オブジェクト）**が1つ必要です。
- 多くの場合、`new`キーワードと一緒に使われます。
- `throw`を使うとその時点でメソッドの実行が中断され、例外がスローされます。

### 使用例：

```java
public void checkAge(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("18歳未満は許可されていません。");
    }
}
```

---

## 🔹 `throws`：**例外のスローを宣言する**

### 目的：

**そのメソッドが例外をスローする可能性があることを宣言**するために使用します。

### 構文：

```java
public void someMethod() throws IOException, SQLException {
    // 処理
}
```

### ポイント：

- **チェック例外**（例：`IOException`, `SQLException`）をスローする可能性がある場合に必要。
- メソッドを呼び出す側に、「このメソッドは例外を投げるかもしれない」と伝える。
- 複数の例外をカンマ区切りで列挙できます。
- **実際に例外をスローするのは `throw`** であり、`throws` はその事前宣言。

### 使用例：

```java
public void readFile(String path) throws IOException {
    BufferedReader reader = new BufferedReader(new FileReader(path));
    // 処理
}
```

---

## 🔸 `throw` と `throws` の比較表

| 項目 | `throw` | `throws` |
| --- | --- | --- |
| 役割 | 例外を実際にスローする | 例外をスローすることを**宣言**する |
| 使用箇所 | メソッドの**内部** | メソッドの**宣言部** |
| 後に続くもの | 例外オブジェクト（`new Exception()`） | 例外クラス名（`IOException`, `SQLException`） |
| 必須の対象 | Throwableクラスを継承したインスタンス | チェック例外（※非チェック例外は任意） |
| 使用例 | `throw new IllegalArgumentException();` | `public void method() throws IOException` |

---

## ✅ 実践コード例

```java
public class Example {

    // throws：このメソッドはIOExceptionをスローする可能性があると宣言
    public void loadFile(String filename) throws IOException {
        if (filename == null) {
            // throw：実際に例外をスロー
            throw new IOException("ファイル名が null です");
        }
        // ファイル読み込み処理...
    }

    public static void main(String[] args) {
        Example ex = new Example();
        try {
            ex.loadFile(null);  // ここで IOException がスローされる
        } catch (IOException e) {
            System.out.println("例外発生: " + e.getMessage());
        }
    }
}
```

---

ご希望であれば、「チェック例外」「非チェック例外」の違いも含めた補足もできます。必要であればお知らせください。