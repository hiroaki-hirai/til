# File.list(), File.listFiles()

# Q、

File.list()について解説してください。

`File.list()` は、`java.io.File` クラスのメソッドで、

**指定されたディレクトリ内に存在するファイルやディレクトリの名前一覧を取得**するためのメソッドです。

ファイル名（`String` 型）の配列で返します。

---

## ✅ メソッド定義

```java
public String[] list()

```

- **戻り値**：そのディレクトリに含まれる名前の配列（`String[]`）
- **返されるのは名前のみ（絶対パスやFileオブジェクトではない）**
- **失敗時は `null` を返す**（例：対象がディレクトリでない、存在しない など）

---

## 🔸 使用例

```java
File dir = new File("sampleDir");

String[] files = dir.list();

if (files != null) {
    for (String name : files) {
        System.out.println(name);
    }
} else {
    System.out.println("ディレクトリが存在しないか、アクセスできません。");
}
```

---

## 🔸 特徴と注意点

| 特徴 | 内容 |
| --- | --- |
| 対象がディレクトリでない場合 | `null` を返す |
| アクセス権がない場合 | `null` を返す |
| 戻り値の型 | `String[]`（ファイル名） |
| 絶対パスが必要な場合 | `listFiles()` を使うべき |

---

## 🔸 比較：`list()` vs `listFiles()`

| メソッド | 戻り値の型 | 内容 |
| --- | --- | --- |
| `list()` | `String[]` | 名前のみ |
| `listFiles()` | `File[]` | File オブジェクト（フルパス） |

---

## 🔸 フィルタ付きのオーバーロード

```java
public String[] list(FilenameFilter filter)
```

🔸 例：拡張子 `.txt` のみ取得

```java
String[] txtFiles = dir.list((d, name) -> name.endsWith(".txt"));
```

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| クラス | `java.io.File` |
| メソッド | `list()` |
| 戻り値 | `String[]`（ファイル・ディレクトリ名） |
| null が返る条件 | 対象が存在しない／ディレクトリでない／アクセス不可 |
| よくある用途 | ディレクトリ内の一覧取得（名前のみ） |

---

必要であれば、`list()` と `Files.list(Path)`（NIO.2 API）の違いや、名前一覧からさらに `File` に変換する応用例もご紹介できます。ご希望があればどうぞ！

# Q,

File.listFiles() について解説してください。

`File.listFiles()` は、`java.io.File` クラスの重要なメソッドのひとつで、

**指定したディレクトリ内のファイルやディレクトリを `File` オブジェクトとして一覧取得する**ために使います。

---

## ✅ 基本構文と戻り値

```java
public File[] listFiles()
```

- **戻り値**：`File[]`（そのディレクトリ内のすべてのファイルやディレクトリの `File` オブジェクト）
- **対象が存在しない、ディレクトリでない、アクセス不可などの場合は `null` を返す**

---

## 🔸 使用例

```java
File dir = new File("sampleDir");

File[] files = dir.listFiles();

if (files != null) {
    for (File file : files) {
        System.out.println(file.getName() + "（絶対パス: " + file.getAbsolutePath() + "）");
    }
} else {
    System.out.println("ディレクトリが存在しないか、アクセスできません。");
}
```

---

## 🔸 特徴

| 特徴 | 内容 |
| --- | --- |
| `File` オブジェクトで返す | 名前だけでなく、フルパスやメタ情報も取得可能 |
| ファイルかディレクトリか判別可 | `isFile()`, `isDirectory()` で判定できる |
| null の可能性あり | エラーハンドリングが必要 |

---

## 🔸 オーバーロードメソッド（フィルタあり）

### 1. `listFiles(FileFilter filter)`

```java
public File[] listFiles(FileFilter filter)
```

- `FileFilter` は関数型インタフェース（`boolean accept(File pathname)`）

🔸 使用例：ディレクトリだけを取得

```java
File[] dirs = dir.listFiles(f -> f.isDirectory());
```

---

### 2. `listFiles(FilenameFilter filter)`

```java
public File[] listFiles(FilenameFilter filter)
```

- `FilenameFilter` は `(File dir, String name)` を受け取る関数型インターフェース

🔸 使用例：`.txt` 拡張子のみ取得

```java
File[] txtFiles = dir.listFiles((d, name) -> name.endsWith(".txt"));
```

---

## 🔸 `list()` との違い

| 比較項目 | `list()` | `listFiles()` |
| --- | --- | --- |
| 戻り値の型 | `String[]`（名前） | `File[]`（フル情報付き） |
| パス取得 | 不可（名前のみ） | 可能（`getPath()`, `getAbsolutePath()` など） |
| 追加情報取得 | 不可 | `length()`, `lastModified()` など利用可 |
| フィルタの種類 | `FilenameFilter` のみ | `FilenameFilter` と `FileFilter` 両方対応 |

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| クラス | `java.io.File` |
| メソッド | `listFiles()` |
| 戻り値 | `File[]`（ファイル・ディレクトリのフルオブジェクト） |
| null が返る条件 | 対象が存在しない／ディレクトリでない／アクセス不可 |
| 主な用途 | ファイル一覧取得、パス操作、属性判定 |
| フィルタ対応 | `FileFilter` / `FilenameFilter`（ラムダ式対応） |

---

さらに必要であれば：

- `listFiles()` を使って **再帰的にファイルを列挙する**
- `Files.walk(Path)`（NIO.2）との違い・使い分け

といった発展的な内容もご案内できます！ご希望ありますか？