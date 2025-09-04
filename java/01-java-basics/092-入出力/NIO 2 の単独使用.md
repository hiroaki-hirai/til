# NIO.2 の単独使用

# Q、

NIO.2は、StreamAPIと組み合わさず単独で使用する場合もありますか？

はい、**NIO.2（`java.nio.file` パッケージ）は、Stream API と組み合わせずに単独で使用するケースも十分にあります。**

むしろ、**NIO.2 の主目的は「高機能なファイルシステム操作 API の提供」であり、Stream API はそれを補完的に扱う手段の1つ**に過ぎません。

---

## ✅ NIO.2 単独使用が一般的な場面

### 1. **ファイル・ディレクトリの操作**

```java
Path path = Paths.get("sample.txt");

if (Files.exists(path)) {
    Files.delete(path);
}

Files.createFile(path);
Files.write(path, List.of("Line 1", "Line 2"));
```

- ファイルの作成・削除・上書き
- `Path` と `Files` の静的メソッドのみで完結
- Stream を使う必要なし（むしろ冗長）

---

### 2. **属性の取得・比較**

```java
BasicFileAttributes attrs = Files.readAttributes(path, BasicFileAttributes.class);
System.out.println("更新日時: " + attrs.lastModifiedTime());
```

- ファイルのメタ情報（サイズ、作成日時、アクセス日時など）を取得
- 集約でも変換でもなく、**点の操作**なので Stream は不要

---

### 3. **コピー・移動**

```java
Path source = Paths.get("input.txt");
Path target = Paths.get("backup/input.txt");

Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);
```

- `Files.copy()`, `Files.move()` は 1 回の処理で完結
- Stream の中間処理のような「流れ」は必要なし

---

### 4. **例外分類による安全な操作**

```java
try {
    Files.deleteIfExists(Paths.get("temp.txt"));
} catch (NoSuchFileException e) {
    System.out.println("ファイルが見つかりませんでした。");
}
```

- `NoSuchFileException`, `FileAlreadyExistsException` などで細かく制御可能
- Stream API の終端操作では例外をうまく分岐できないため、**NIO.2 単体が適している**

---

## ✅ 一方で Stream API を組み合わせるのが有効なケース

| 処理内容 | 備考 |
| --- | --- |
| テキストファイルの読み込みと加工 | `Files.lines(Path)` → `Stream<String>` で変換 |
| 複数ファイルの再帰探索 | `Files.walk()` → `Stream<Path>`でフィルタ適用 |
| 条件付きファイル操作 | `.filter(Files::isRegularFile)` などが可能 |

---

## ✅ 結論

| 使い方 | Stream API と組み合わせる？ | 解説 |
| --- | --- | --- |
| ファイルや属性の単発取得・変更 | ❌ 単独で十分 | `Files.exists()`, `Files.copy()` など |
| 行単位のテキスト読み取り・加工 | ✅ 組み合わせると便利 | `Files.lines()` → `filter`, `map` など |
| 再帰的ファイル走査（階層探索） | ✅ 効率的 | `Files.walk()` → `.filter()` で条件適用 |
| ディレクトリの作成・削除 | ❌ 単独で使用 | `Files.createDirectories()`, `Files.delete()` |

---

つまり、**NIO.2 は汎用的に「ファイルシステム API」として設計されているため、Stream API を使わずとも十分に強力**です。

**Stream はあくまで「パイプライン処理が必要な場面」で導入する手段**と理解するのが正しい位置付けです。

---

ご希望があれば「NIO.2 単独の処理だけで構成されたファイル操作ユーティリティ」や「Stream を使うべきかどうかの判断基準チェックリスト」などもご案内できます！