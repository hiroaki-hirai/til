# resource

# Q,

try-with-resource の resource にあたるものをまとめてください。

はい、Hiroakiさん。

`try-with-resources` 文の `resource` にあたるのは、**`AutoCloseable` インタフェース**（またはそのサブインタフェースである `Closeable`）を実装しているオブジェクトです。これらは「**クローズ可能なリソース**」と呼ばれます。

---

## ✅ try-with-resources に使える主なリソース一覧

### 📂 【ファイル入出力関連（`java.io` パッケージ）】

| クラス名 | 用途 | 実装インタフェース |
| --- | --- | --- |
| `FileReader` | ファイル読み込み（文字） | `Closeable` |
| `FileWriter` | ファイル書き出し（文字） | `Closeable` |
| `BufferedReader` | 読み込みの高速化 | `Closeable` |
| `BufferedWriter` | 書き込みの高速化 | `Closeable` |
| `PrintWriter` | テキスト出力 | `Closeable` |
| `InputStream`（抽象） | バイト入力 | `Closeable` |
| `OutputStream`（抽象） | バイト出力 | `Closeable` |
| `FileInputStream` | ファイルのバイナリ入力 | `Closeable` |
| `FileOutputStream` | ファイルのバイナリ出力 | `Closeable` |

---

### 🔌 【ネットワーク・通信関連】

| クラス名 | 用途 | 実装インタフェース |
| --- | --- | --- |
| `Socket` | 通信ソケット（TCP） | `Closeable` |
| `ServerSocket` | サーバー用ソケット | `Closeable` |

---

### 🗃️ 【データベース（JDBC）関連】

| クラス名 | 用途 | 実装インタフェース |
| --- | --- | --- |
| `Connection`（java.sql） | DB接続 | `AutoCloseable` |
| `Statement` | SQL実行 | `AutoCloseable` |
| `PreparedStatement` | プリコンパイルSQL | `AutoCloseable` |
| `ResultSet` | クエリ結果の取得 | `AutoCloseable` |

---

### 📦 【Java NIO（非同期IO）関連】

| クラス名 | 用途 | 実装インタフェース |
| --- | --- | --- |
| `FileChannel` | チャンネルベースのI/O | `Closeable` |
| `AsynchronousFileChannel` | 非同期ファイルアクセス | `Closeable` |
| `DirectoryStream` | ディレクトリ一覧取得 | `Closeable` |

---

### ⚙️ 【その他の利用可能なクラス】

| クラス名 | 用途 | 実装インタフェース |
| --- | --- | --- |
| `Scanner` | 入力の読み取り | `Closeable` |
| `ZipFile` | ZIPファイルの読み取り | `Closeable` |
| `JarFile` | JARファイルの読み取り | `Closeable` |
| `ObjectInputStream` | オブジェクトの読み込み | `Closeable` |
| `ObjectOutputStream` | オブジェクトの書き出し | `Closeable` |

---

## 🔧 自作クラスでも使える

自作のクラスも `AutoCloseable`（または `Closeable`）を実装すれば `try-with-resources` で使えます。

```java
class MyResource implements AutoCloseable {
    @Override
    public void close() {
        System.out.println("リソースを閉じました");
    }
}
```

---

## ✅ 補足：`AutoCloseable` vs `Closeable`

| 特徴 | `AutoCloseable` | `Closeable` |
| --- | --- | --- |
| パッケージ | `java.lang` | `java.io` |
| Javaバージョン | Java 7〜 | Java 5〜 |
| 例外の扱い | `Exception` をスロー可 | `IOException` のみ |
| 使用場面 | より汎用的 | 主に I/O 関連で使用 |

---

必要であれば、これらのリソースを実際に使った `try-with-resources` のコード例を一覧でご紹介することもできます。希望があればお知らせください。