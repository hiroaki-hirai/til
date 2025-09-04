# close()

# Q,

上記のresource に当たる処理の途中でエラーが発生した場合、他のデータ保管場所や他ネットワークへの接続に「～しっぱなし」状態になるので、それの状態を解消するために close する必要がある認識です。
AutoClosableやCloseableインタフェースにてcloseメソッドが規定されていますが、それらのresourceのクラス内にてcloseメソッドがオーバーライドされているのでしょうか？

はい、Hiroakiさんのご認識は正確です。

リソースの使用中にエラーや例外が発生しても、**システムリソース（ファイルディスクリプタ、ソケット、DB接続など）を開きっぱなしにしないために `close()` が必要**です。

そしてご質問の「`close()` メソッドは、リソースクラスでオーバーライドされているのか？」について、以下に詳しく解説します。

---

## ✅ 結論：はい、`close()` メソッドはリソースクラス内でオーバーライドされています。

### 🔹 しくみの流れ

1. `try-with-resources` に使うクラスは `AutoCloseable` もしくは `Closeable` を実装しています。
2. `AutoCloseable` インタフェースには以下のようなシグネチャがあります：

```java
public interface AutoCloseable {
    void close() throws Exception;
}
```

1. これを実装するクラスでは、**実際にクローズ処理（ファイルやソケットの解放など）を行うように `close()` メソッドを** **オーバーライド** しています。

---

## 🔸 具体例：`BufferedReader`

```java
public class BufferedReader extends Reader {
    @Override
    public void close() throws IOException {
        synchronized (lock) {
            if (in == null)
                return;
            in.close();  // 内部のリーダーも閉じる
            in = null;
            cb = null;
        }
    }
}
```

- `BufferedReader` は `Reader` を継承し、`Reader` は `Closeable` を実装。
- `close()` メソッドを明示的にオーバーライドし、内部の `InputStreamReader` などを閉じています。

---

## 🔸 具体例：`Connection`（JDBC）

```java
public class JdbcConnection implements Connection {
    @Override
    public void close() throws SQLException {
        if (!isClosed()) {
            // セッションやネットワークリソースを閉じる処理
            cleanup();
        }
    }
}
```

- JDBC 接続クラス（実装は DB ドライバ依存）は `AutoCloseable` を実装。
- 実際には DBソケットのクローズ、セッションの解放などが行われます。

---

## 🔸 カスタムクラスでも同じように実装可能

```java
public class MyResource implements AutoCloseable {
    @Override
    public void close() {
        System.out.println("MyResource closed.");
    }
}
```

`try-with-resources` はこのようなユーザー定義のクラスにも対応しています。

---

## ✅ 補足：もし `close()` をオーバーライドしていなければ？

`AutoCloseable` を実装していても `close()` の中身が空なら、当然ながら何もリソースは解放されません。

→ そのため、**各クラスが適切にオーバーライドしていることが前提**で、`try-with-resources` の自動解放が機能するということです。

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| Hiroakiさんの認識 | 正確：「開きっぱなし」を防ぐために `close()` が必要 |
| `close()` の定義元 | `AutoCloseable` または `Closeable` インタフェース |
| `close()` の実装場所 | 各リソースクラス内でオーバーライドされており、実際のリソース解放処理を行っている |
| 自作クラスでも可能か？ | ✅ 可能：`AutoCloseable` を実装し、`close()` に解放処理を書く |

---

必要であれば、よく使うリソースの `close()` 実装のソースコード抜粋をいくつかまとめて一覧化もできますので、希望があればお知らせください。