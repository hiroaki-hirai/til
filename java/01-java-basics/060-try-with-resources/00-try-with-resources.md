# try-with-resources

はい、Hiroakiさん。

`try-with-resources` は、Java 7 以降で導入された **自動リソース管理**（Automatic Resource Management）を可能にする構文です。ファイルやネットワーク接続、データベース接続のように **明示的にクローズが必要なリソース**を安全に扱うためのものです。

---

## 🔸 基本構文

```java
try (ResourceType resource = new ResourceType()) {
    // リソースを使った処理
} catch (Exception e) {
    e.printStackTrace();
}
```

`try()` に宣言されたリソースは、**ブロックを抜けると自動で close() される**ため、`finally` ブロックでのクローズが不要です。

---

## 🔹 対象となる「リソース」とは？

Javaでは、「**`java.lang.AutoCloseable`**」インタフェース（もしくはそのサブインタフェースである `java.io.Closeable`）を実装しているクラスを「クローズ可能リソース」とみなします。

### 例: `BufferedReader` の使用

```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}
```

このコードでは、`BufferedReader` が `AutoCloseable` を実装しているため、tryブロックを抜けると `br.close()` が自動で呼ばれます。

---

## 🔹 複数リソースの管理

複数のリソースはセミコロン `;` で区切って宣言できます。

```java
try (
    InputStream in = new FileInputStream("in.txt");
    OutputStream out = new FileOutputStream("out.txt")
) {
    // in から out へコピーなど
}
```

※ 宣言順とは**逆順でクローズ**されます（out → in の順）

---

## 🔸 `try-with-resources` の利点

| 利点 | 説明 |
| --- | --- |
| ✅ リソースの確実なクローズ | `try` が終了する際に `close()` が確実に呼ばれる |
| ✅ コードが簡潔になる | `finally` ブロックで手動で `close()` する必要がない |
| ✅ 例外処理が一貫して行える | `close()` 中の例外も `SuppressedException` として扱われ、追跡可能 |

---

## 🔹 内部的な動作（補足）

```java
try (MyResource res = new MyResource()) {
    // 処理
}
```

これは、以下のように展開されるイメージです：

```java
MyResource res = new MyResource();
try {
    // 処理
} finally {
    if (res != null) {
        res.close();
    }
}
```

---

## 🔸 カスタムリソースの例

```java
class MyResource implements AutoCloseable {
    @Override
    public void close() {
        System.out.println("MyResource closed.");
    }

    public void doSomething() {
        System.out.println("Working...");
    }
}
```

```java
try (MyResource res = new MyResource()) {
    res.doSomething();
}
```

---

## 🔸 まとめ

| 項目 | 内容 |
| --- | --- |
| 導入バージョン | Java 7 |
| 対象インタフェース | `AutoCloseable` / `Closeable` |
| 自動で呼ばれる処理 | `close()` メソッド |
| finallyブロック | 原則不要（ただし catch の補足は可） |

---

必要であれば、`try-with-resources` を使った DB接続（JDBC）や Socket 通信の具体例もご紹介できます。興味があれば教えてください！

[resource](try-with-resources%2022362cef73508038870ede6e16f40dd7/resource%2022362cef735080ffadf0ee3f36333074.md)

[close()](try-with-resources%2022362cef73508038870ede6e16f40dd7/close()%2022362cef73508018a09df6a22e69ed00.md)

[インスタンス生成 - close()](try-with-resources%2022362cef73508038870ede6e16f40dd7/%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E7%94%9F%E6%88%90%20-%20close()%2024762cef7350804ea08ed13a4b78ddbb.md)

[糖衣構文](try-with-resources%2022362cef73508038870ede6e16f40dd7/%E7%B3%96%E8%A1%A3%E6%A7%8B%E6%96%87%2022362cef735080268d90d97b970c2291.md)

[`try` 文の外で初期化](try-with-resources%2022362cef73508038870ede6e16f40dd7/try%20%E6%96%87%E3%81%AE%E5%A4%96%E3%81%A7%E5%88%9D%E6%9C%9F%E5%8C%96%2024762cef7350804cbaafda729a9357fa.md)

[リソースの閉じ順序](try-with-resources%2022362cef73508038870ede6e16f40dd7/%E3%83%AA%E3%82%BD%E3%83%BC%E3%82%B9%E3%81%AE%E9%96%89%E3%81%98%E9%A0%86%E5%BA%8F%2024762cef735080768105c613aac44d16.md)

[実行順](try-with-resources%2022362cef73508038870ede6e16f40dd7/%E5%AE%9F%E8%A1%8C%E9%A0%86%2024762cef735080a48b2ecc536f1affc8.md)