# Fileクラス

# Q,

java.io.Fileクラスの主要メソッドをまとめてください。

もちろんです。`java.io.File` クラスは、**ファイルやディレクトリのパス名を表現・操作するためのクラス**で、ファイルシステム上のオブジェクトの存在確認、属性取得、削除・作成などを行うために使います。

---

## ✅ `java.io.File` の主要メソッドまとめ

以下はカテゴリ別に整理した一覧です。

---

### 🔸 コンストラクタ（インスタンス生成）

| コンストラクタ | 説明 |
| --- | --- |
| `File(String pathname)` | パス名から File インスタンス生成 |
| `File(String parent, String child)` | 親パスと子パスを結合 |
| `File(File parent, String child)` | File オブジェクトと子パスを結合 |

---

### 🔸 基本情報の取得

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `getName()` | `String` | ファイル名のみ取得 |
| `getPath()` | `String` | パス全体（指定された形式） |
| `getAbsolutePath()` | `String` | 絶対パスを取得 |
| `getCanonicalPath()` | `String` | 解決された正規パス（例外あり） |
| `getParent()` | `String` | 親ディレクトリのパス |
| `getParentFile()` | `File` | 親ディレクトリの File オブジェクト |
| `toURI()` | `URI` | URI に変換 |

---

### 🔸 ファイル・ディレクトリの操作

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `exists()` | `boolean` | ファイルやディレクトリの存在確認 |
| `isFile()` | `boolean` | 通常のファイルかどうか |
| `isDirectory()` | `boolean` | ディレクトリかどうか |
| `createNewFile()` | `boolean` | 新しい空ファイルを作成（例外あり） |
| `mkdir()` | `boolean` | ディレクトリ作成（単階層） |
| `mkdirs()` | `boolean` | 必要な親ディレクトリもまとめて作成 |
| `delete()` | `boolean` | ファイル/ディレクトリ削除（空のみ） |
| `renameTo(File dest)` | `boolean` | ファイル名変更 or 移動 |

---

### 🔸 ファイルリスト関連

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `list()` | `String[]` | ファイル名の配列を取得 |
| `listFiles()` | `File[]` | File オブジェクト配列を取得 |
| `list(FilenameFilter filter)` | `String[]` | フィルタ指定で名前取得 |
| `listFiles(FileFilter filter)` | `File[]` | フィルタ指定で File 取得 |

---

### 🔸 読み書き・パーミッション

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `canRead()` | `boolean` | 読み取り可能か |
| `canWrite()` | `boolean` | 書き込み可能か |
| `canExecute()` | `boolean` | 実行可能か |
| `setReadable(boolean)` | `boolean` | 読み取り許可の変更 |
| `setWritable(boolean)` | `boolean` | 書き込み許可の変更 |
| `setExecutable(boolean)` | `boolean` | 実行許可の変更 |

---

### 🔸 その他のユーティリティ

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `length()` | `long` | ファイルサイズ（バイト） |
| `lastModified()` | `long` | 最終更新日時（ミリ秒） |
| `setLastModified(long)` | `boolean` | 更新日時を変更 |
| `isAbsolute()` | `boolean` | 絶対パスかどうか |
| `compareTo(File other)` | `int` | パス名で比較（自然順序） |

---

## ✅ 補足：注意点

- `File` は「ファイルやディレクトリの**パス情報**を表す」クラスであり、**ファイルの中身は扱いません**。
- ファイル内容の読み書きには `FileReader` / `FileWriter` や `Files` クラスを使います。
- `createNewFile()` は例外を投げるので `try-catch` が必要です。

---

必要であれば、`File` クラスと `Path` / `Files` クラス（NIO.2）との使い分けや、実用的なコード例も紹介できます！

# Q,

java.io.File の主要メソッドの中で関数インターフェイスを引数にとるものをまとめてください。

`java.io.File` クラスの中で、**関数型インターフェイス（＝ラムダ式やメソッド参照を渡せる）を引数に取るメソッド**は主に以下の2つです。

---

## ✅ 1. `list(FilenameFilter filter)`

```java
public String[] list(FilenameFilter filter)
```

- **インターフェイス型**: `java.io.FilenameFilter`
- **関数型インターフェイス**: `boolean accept(File dir, String name)`
- **目的**: 指定された条件に合うファイル名だけをリストアップ

🔸 使用例（ラムダ式）：

```java
File dir = new File("sampleDir");

String[] txtFiles = dir.list((d, name) -> name.endsWith(".txt"));

System.out.println(Arrays.toString(txtFiles));
```

---

## ✅ 2. `listFiles(FileFilter filter)`

```java
public File[] listFiles(FileFilter filter)
```

- **インターフェイス型**: `java.io.FileFilter`
- **関数型インターフェイス**: `boolean accept(File pathname)`
- **目的**: 条件に合致する `File` オブジェクトだけを返す

🔸 使用例（ラムダ式）：

```java
File dir = new File("sampleDir");

File[] directories = dir.listFiles(file -> file.isDirectory());

for (File d : directories) {
    System.out.println(d.getName());
}
```

---

## ✅ まとめ表

| メソッド名 | 引数のインターフェイス | メソッドシグネチャ | 主な用途 |
| --- | --- | --- | --- |
| `list(FilenameFilter)` | `FilenameFilter` | `boolean accept(File dir, String name)` | ファイル名による絞り込み |
| `listFiles(FileFilter)` | `FileFilter` | `boolean accept(File pathname)` | File オブジェクトによる絞り込み |

---

## ✅ 補足

- どちらも**関数型インターフェイス**なので、ラムダ式やメソッド参照で書けます。
- **Java 8以降**でラムダが使えるようになってから、より簡潔に記述できるようになりました。
- `Files.list(...)`（NIO.2）と混同しないように注意してください。

---

もし必要であれば、`Predicate` や `Function` など `java.util.function.*` 系と混在する場面や、NIO (`Path`, `Files`) との比較も補足できます！