# java.nio

# Q,

java.nio の主なクラスやインターフェイスをまとめてください。

`java.nio` パッケージ（NIO = New I/O）は、Java 1.4 で導入された **高性能で柔軟な入出力処理**のための API 群です。

さらに Java 7 で拡張された `java.nio.file` などを含めて、以下のように整理できます。

---

## ✅ 1. パッケージ構成（全体像）

| パッケージ名 | 主な内容 |
| --- | --- |
| `java.nio` | バッファ（Buffer）・基本データ構造 |
| `java.nio.channels` | チャネル（ファイル、ソケットなど） |
| `java.nio.file` | ファイル/パス操作のモダンAPI（NIO.2） |
| `java.nio.file.attribute` | ファイル属性（パーミッション、更新日時など） |
| `java.nio.charset` | 文字エンコーディング/デコーディング |
| `java.nio.channels.spi` | チャネルの拡張ポイント |

---

## ✅ 2. 主なクラス・インターフェイスまとめ（カテゴリ別）

---

### 🔸 A. Buffer（データ格納とアクセス）

| クラス名 | 説明 |
| --- | --- |
| `Buffer` | すべてのバッファの抽象基底クラス |
| `ByteBuffer` | バイト型のバッファ |
| `CharBuffer` | 文字型バッファ |
| `IntBuffer` | 整数バッファ |
| `LongBuffer` | 長整数バッファ |
| `FloatBuffer` | 単精度浮動小数バッファ |
| `DoubleBuffer` | 倍精度浮動小数バッファ |
| `ShortBuffer` | ショート型バッファ |

---

### 🔸 B. Channels（入出力ソース）

| インターフェイス / クラス | 説明 |
| --- | --- |
| `ReadableByteChannel` | 読み取り可能なチャネル（byteベース） |
| `WritableByteChannel` | 書き込み可能なチャネル |
| `FileChannel` | ファイルとの読み書きを扱うチャネル |
| `SocketChannel` | ソケットベースのチャネル |
| `ServerSocketChannel` | サーバーソケットのチャネル |
| `DatagramChannel` | UDPパケットの送受信用 |
| `SelectableChannel` | 非同期I/O 用の基底チャネル |
| `Channels`（ユーティリティ） | チャネルとストリームの相互変換 |

---

### 🔸 C. Paths / Files（ファイルシステム操作）

| クラス / インターフェイス | 説明 |
| --- | --- |
| `Path` | ファイルやディレクトリのパス（抽象表現） |
| `Paths` | `Path` インスタンスのファクトリクラス |
| `Files` | ファイルやディレクトリの操作ユーティリティ |
| `DirectoryStream` | ディレクトリ一覧取得（`Files.newDirectoryStream()`） |
| `FileSystem` | OSのファイルシステム情報の抽象表現 |
| `FileSystems` | デフォルトファイルシステムを取得するファクトリ |

---

### 🔸 D. 属性・パーミッション関連

| クラス / インターフェイス | 説明 |
| --- | --- |
| `BasicFileAttributes` | ファイルの基本属性（サイズ・更新日時など） |
| `PosixFileAttributes` | POSIX形式の詳細属性（Linuxなど） |
| `DosFileAttributes` | Windows用のDOS属性 |
| `FileAttributeView`（階層あり） | 属性ビューのインターフェイス群 |

---

### 🔸 E. 文字エンコーディング

| クラス名 | 説明 |
| --- | --- |
| `Charset` | 文字コード（例：UTF-8, Shift_JIS） |
| `CharsetDecoder` | バイト列 → 文字列のデコード |
| `CharsetEncoder` | 文字列 → バイト列のエンコード |
| `StandardCharsets` | 標準で利用可能な文字コードの定数群 |

---

## ✅ NIO vs java.io の違い（補足）

| 比較項目 | `java.io` | `java.nio` |
| --- | --- | --- |
| 処理単位 | ストリーム（順次） | バッファ（ランダムアクセス） |
| 同期／非同期 | 同期処理 | 非同期（チャネル + セレクタ）も可 |
| パフォーマンス | 小規模向き | 高性能・大容量向き |
| 機能 | 基本的 | 高度な機能あり |
| 学習コスト | 低め | やや高め（構造が複雑） |

---

## ✅ よく使う組み合わせ例

| 用途 | 使用クラス例 |
| --- | --- |
| ファイルの存在確認 | `Files.exists(Path)` |
| ファイルの読み書き | `Files.readAllLines(Path)`, `Files.write(...)` |
| ファイルコピー | `Files.copy()`, `Files.move()`, `Files.delete()` |
| ディレクトリ走査 | `Files.walk()`, `Files.newDirectoryStream()` |
| 非同期ファイル転送 | `FileChannel.transferTo()`, `transferFrom()` |
| バイナリ処理 | `ByteBuffer`, `FileChannel` |

---

## ✅ まとめ

| カテゴリ | 主な代表クラス |
| --- | --- |
| バッファ | `ByteBuffer`, `CharBuffer`, `IntBuffer` |
| チャネル | `FileChannel`, `SocketChannel` |
| パスとファイル | `Path`, `Files`, `Paths` |
| 属性情報 | `BasicFileAttributes`, `FileAttributeView` |
| 文字コード | `Charset`, `CharsetDecoder`, `CharsetEncoder` |

---

必要であれば、実際のコード例（`Path` を使ったファイル読み込み／コピーなど）や、NIO.2 のエラー処理戦略なども詳しく解説できます。ご希望ありますか？

[導入](java%20nio%2024162cef7350800bb0bde83869263b16/%E5%B0%8E%E5%85%A5%2024162cef73508006b9c4ca486b9a2022.md)

[Files - lines() と readAllLines()](java%20nio%2024162cef7350800bb0bde83869263b16/Files%20-%20lines()%20%E3%81%A8%20readAllLines()%2024b62cef7350807b91e2e4b3e875e376.md)