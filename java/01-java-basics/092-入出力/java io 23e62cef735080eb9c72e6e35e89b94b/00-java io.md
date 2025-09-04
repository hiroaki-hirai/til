# java.io

# Q,

java.io の主なクラスやインターフェイスをまとめてください。

`java.io` パッケージは、**Java の標準 I/O（入力・出力）処理に関する基本機能**を提供するパッケージで、ファイル・ストリーム・シリアライズなどを扱います。

---

## ✅ 主なクラス・インターフェイス一覧（カテゴリ別）

### 🔹 1. ファイル操作関連

| 名前 | 種別 | 説明 |
| --- | --- | --- |
| `File` | クラス | ファイルやディレクトリを表す |
| `FileDescriptor` | クラス | ネイティブファイル記述子のラッパー |
| `FileReader` | クラス | ファイルから文字ストリームで読み込み |
| `FileWriter` | クラス | ファイルに文字ストリームで書き込み |
| `RandomAccessFile` | クラス | ファイルのランダムアクセス読み書き |

---

### 🔹 2. バイトストリーム（低水準 I/O）

| 名前 | 種別 | 説明 |
| --- | --- | --- |
| `InputStream` | 抽象クラス | バイト単位の入力の基底クラス |
| `OutputStream` | 抽象クラス | バイト単位の出力の基底クラス |
| `FileInputStream` | クラス | ファイルからバイト入力 |
| `FileOutputStream` | クラス | ファイルへのバイト出力 |
| `BufferedInputStream` | クラス | 入力のバッファリング |
| `BufferedOutputStream` | クラス | 出力のバッファリング |
| `DataInputStream` | クラス | プリミティブ型をバイトストリームから読み込み |
| `DataOutputStream` | クラス | プリミティブ型をバイトストリームに書き込み |
| `ByteArrayInputStream` | クラス | メモリ上のバイト配列からの入力 |
| `ByteArrayOutputStream` | クラス | メモリ上のバイト配列への出力 |
| `ObjectInputStream` | クラス | オブジェクトのデシリアライズ |
| `ObjectOutputStream` | クラス | オブジェクトのシリアライズ |

---

### 🔹 3. 文字ストリーム（Reader / Writer）

| 名前 | 種別 | 説明 |
| --- | --- | --- |
| `Reader` | 抽象クラス | 文字入力の基底クラス |
| `Writer` | 抽象クラス | 文字出力の基底クラス |
| `BufferedReader` | クラス | 行単位の読み込みなど効率的処理 |
| `BufferedWriter` | クラス | バッファリング付き文字出力 |
| `InputStreamReader` | クラス | バイト→文字変換（デコレータ） |
| `OutputStreamWriter` | クラス | 文字→バイト変換（デコレータ） |
| `CharArrayReader` | クラス | メモリ上の文字配列からの入力 |
| `CharArrayWriter` | クラス | メモリ上の文字配列への出力 |
| `StringReader` | クラス | 文字列からの読み込み |
| `StringWriter` | クラス | 文字列への書き出し（StringBuffer） |

---

### 🔹 4. 補助インターフェイス・クラス

| 名前 | 種別 | 説明 |
| --- | --- | --- |
| `Serializable` | インターフェイス | オブジェクトのシリアライズを許可 |
| `Externalizable` | インターフェイス | カスタムシリアライズ制御を可能にする |
| `ObjectInput` | インターフェイス | `ObjectInputStream` の上位インターフェイス |
| `ObjectOutput` | インターフェイス | `ObjectOutputStream` の上位インターフェイス |
| `FilenameFilter` | インターフェイス | ファイル名によるフィルタリング |
| `FileFilter` | インターフェイス | `File` 単位のフィルタリング |
| `IOException` | クラス | I/O 全般の例外の基底クラス |
| `EOFException` | クラス | ファイル終端に達したときの例外 |

---

## ✅ 特に実務でよく使う代表クラス

| 目的 | 使用クラス例 |
| --- | --- |
| ファイル存在チェック・操作 | `File` |
| テキストファイル読み書き | `FileReader`, `FileWriter`, `BufferedReader` |
| バイナリ読み書き | `FileInputStream`, `FileOutputStream`, `Data*Stream` |
| オブジェクト保存・復元 | `ObjectInputStream`, `ObjectOutputStream` |
| 文字エンコーディング制御 | `InputStreamReader`, `OutputStreamWriter` |

---

## ✅ 補足：新しい API（NIO.2）

Java 7 以降は `java.nio.file.*` パッケージ（`Files`, `Paths`, `Path` など）を使ったファイル操作が主流になりつつあります。

しかし `java.io` はシンプルかつ軽量で、**基本のファイル操作・学習には今でも重要**です。

---

ご希望があれば、「シリアライズ系クラス」「Buffered クラス群の使い分け」「文字とバイトストリームの変換」なども詳しく解説できます！

# Q,

java.ioパッケージの主なメソッドをまとめてください。

`java.io` パッケージは、Java の**基本的な入出力（I/O）機能**を提供するためのパッケージで、バイト/文字ストリーム、ファイル操作、オブジェクトのシリアライズなどが含まれています。

ここでは「主なクラス」に対して「よく使うメソッド」を**カテゴリ別に整理**してご紹介します。

---

## ✅ 1. `File` クラスの主なメソッド（ファイル・ディレクトリ操作）

| メソッド名 | 説明 |
| --- | --- |
| `exists()` | ファイル/ディレクトリが存在するか確認 |
| `isFile()` / `isDirectory()` | ファイル or ディレクトリかどうかを判定 |
| `createNewFile()` | 新しいファイルを作成（存在しない場合） |
| `mkdir()` / `mkdirs()` | 単階層 or 多階層のディレクトリを作成 |
| `delete()` | ファイル/ディレクトリを削除 |
| `getName()` / `getPath()` | ファイル名 / パスを取得 |
| `getAbsolutePath()` | 絶対パスを取得 |
| `list()` / `listFiles()` | ディレクトリ内の一覧取得（名前 or File[]） |
| `renameTo(File dest)` | ファイル名変更または移動 |

---

## ✅ 2. `InputStream` / `OutputStream` 系（バイトストリーム）

| クラス | 主なメソッド | 説明 |
| --- | --- | --- |
| `InputStream` | `read()`, `read(byte[])`, `close()` | バイト単位での読み込み |
| `OutputStream` | `write(int)`, `write(byte[])`, `flush()`, `close()` | バイト単位での書き出し |
| `FileInputStream` | `read()` など | ファイルからのバイト読み込み |
| `FileOutputStream` | `write()` など | ファイルへのバイト書き出し |
| `BufferedInputStream` / `BufferedOutputStream` | 同上（バッファリング付き） | 処理高速化のための中間バッファ使用 |

---

## ✅ 3. `Reader` / `Writer` 系（文字ストリーム）

| クラス | 主なメソッド | 説明 |
| --- | --- | --- |
| `Reader` | `read()`, `read(char[])`, `close()` | 文字単位での読み込み |
| `Writer` | `write(int)`, `write(String)`, `flush()`, `close()` | 文字単位での書き出し |
| `FileReader` | `read()` など | ファイルから文字を読み込む |
| `FileWriter` | `write()` など | ファイルへ文字を書き出す |
| `BufferedReader` | `readLine()` | 1行単位での文字読み込み（高速） |
| `BufferedWriter` | `newLine()`, `write()` | 改行付きの文字出力 |

---

## ✅ 4. オブジェクトのシリアライズ・デシリアライズ

| クラス | 主なメソッド | 説明 |
| --- | --- | --- |
| `ObjectOutputStream` | `writeObject(Object)` | オブジェクトをストリームにシリアライズ |
| `ObjectInputStream` | `readObject()` | ストリームからオブジェクトを復元 |
| `Serializable`（インターフェース） | ― | シリアライズ可能であることを示す |

---

## ✅ 5. ユーティリティクラスとその他

| クラス | 主なメソッド | 説明 |
| --- | --- | --- |
| `PrintStream` | `print()`, `println()`, `printf()` | フォーマット付き出力（System.out など） |
| `PushbackInputStream` | `unread()` | 1バイト戻す（先読み） |
| `SequenceInputStream` | `read()` | 複数の `InputStream` を連結 |
| `DataInputStream` | `readInt()`, `readUTF()` など | プリミティブ型データを読み込む |
| `DataOutputStream` | `writeInt()`, `writeUTF()` など | プリミティブ型データを書き込む |

---

## ✅ よく使われる組み合わせ例

| 処理内容 | 使用クラス例 |
| --- | --- |
| テキストファイルの読み取り | `BufferedReader` + `FileReader` |
| バイナリファイルの保存 | `FileOutputStream` + `BufferedOutputStream` |
| ファイルの存在確認 | `File.exists()`, `File.isFile()` |
| オブジェクトの保存 | `ObjectOutputStream.writeObject()` |
| 標準出力への文字列出力 | `System.out.println()`（`PrintStream`） |

---

## ✅ 補足

- `java.io` は逐次的で同期的な I/O を基本としています。
- **Java 7以降では `java.nio.file`（NIO.2）と併用または置き換えが推奨される場面も増えています**。

---

ご希望があれば、**各クラスの使い分けパターン**や、**BufferedReader vs Scanner** のような比較も詳しく解説できます。お気軽にお知らせください！

[Fileクラス](java%20io%2023e62cef735080eb9c72e6e35e89b94b/File%E3%82%AF%E3%83%A9%E3%82%B9%2023e62cef7350800da418e9245f7176dd.md)

[File.list(), File.listFiles()](java%20io%2023e62cef735080eb9c72e6e35e89b94b/File%20list(),%20File%20listFiles()%2023f62cef735080f7a5c3c49f8f1c5d46.md)

[FileReaderクラス](java%20io%2023e62cef735080eb9c72e6e35e89b94b/FileReader%E3%82%AF%E3%83%A9%E3%82%B9%2024162cef735080fc8f2fe8049254e40a.md)

[BufferedReaderクラス](java%20io%2023e62cef735080eb9c72e6e35e89b94b/BufferedReader%E3%82%AF%E3%83%A9%E3%82%B9%2024162cef73508017b332d30f514f1352.md)

[BufferedInputStreamクラス](java%20io%2023e62cef735080eb9c72e6e35e89b94b/BufferedInputStream%E3%82%AF%E3%83%A9%E3%82%B9%2024162cef7350801fb633c16240f667ad.md)

[行単位読み取り](java%20io%2023e62cef735080eb9c72e6e35e89b94b/%E8%A1%8C%E5%8D%98%E4%BD%8D%E8%AA%AD%E3%81%BF%E5%8F%96%E3%82%8A%2024b62cef735080a0ac6be3a000f12ca7.md)