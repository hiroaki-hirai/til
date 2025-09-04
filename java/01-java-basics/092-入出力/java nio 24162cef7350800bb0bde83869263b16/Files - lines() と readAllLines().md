# Files - lines() と readAllLines()

# Q,

java.nio.file.Filesクラスのlines()とreadAllLines()を比較してまとめてください。

了解です。`Files.lines()` と `Files.readAllLines()` を実務視点でサクッと比較します。

# ざっくり結論

- **大きいファイル / ストリーム処理したい** → `lines()`（遅延・低メモリ・要クローズ）
- **小さめのファイルを一気に扱いたい** → `readAllLines()`（即時・高メモリ・クローズ不要）

# API比較表

| 観点 | `Files.lines(Path[, Charset])` | `Files.readAllLines(Path[, Charset])` |
| --- | --- | --- |
| 返り値 | `Stream<String>`（遅延） | `List<String>`（即時） |
| メモリ特性 | **低**（1行ずつ処理） | **高**（全行を一括読み込み） |
| ファイルハンドル | **ストリームを閉じるまで開いたまま** | 読み込み完了時にクローズ済み |
| リソース管理 | **必ず `try-with-resources`** で閉じる | 追加のクローズ不要 |
| 改行の扱い | 行末 `\n / \r / \r\n` を境に分割、返却文字列に改行は含まない |  |
| 文字コード | `Charset` 指定推奨（**未指定はUTF‑8既定※**） |  |
| 例外の出方 | オープン時は `IOException`、**読み取り中は `UncheckedIOException`** としてスローされ得る（ストリーム処理中に発生） | 呼び出し時にまとめて `IOException`（読み込み時に発生） |
| 短絡処理 | 可能（`findFirst/anyMatch/limit` で途中打ち切り） | 不可（全行ロード後に処理） |
| 並列処理 | `.parallel()` は可能だが通常はI/O律速で効果薄 | リストに対して並列操作は可能（読み込みは単発） |
| 適した用途 | ログ等の**逐次処理**、巨大ファイル、ストリームAPIとの連携 | **小～中規模**ファイルの一括処理、ランダムアクセス（`list.get(i)`） |

※「未指定はUTF‑8既定」は Java 11以降の `files.*(Path)` 系での既定。混在環境では**常に明示指定**が安全です。

# 使い分けの指針

- **巨大ファイル／途中で打ち切りたい処理**（例：最初の一致だけ探す）→ `lines()`

```java
try (Stream<String> s = Files.lines(path, StandardCharsets.UTF_8)) {
    Optional<String> hit = s.filter(l -> l.contains("ERROR")).findFirst();
}
```

- **全行をすぐ配列/リストで持ちたい**（サイズが読める設定/テストデータ等）→ `readAllLines()`

```java
List<String> lines = Files.readAllLines(path, StandardCharsets.UTF_8);
String third = lines.get(2);
```

# 実務Tips

- `lines()` は**閉じ忘れ注意**（ファイルがロックされ続ける）。`try-with-resources`必須。
- 末尾改行がなくても**最終行は取得される**（両者共通）。
- どちらも**返す文字列に改行は含まれない**。必要なら自分で付与。
- 文字化け事故を避けるため、**常に `Charset` を明示**（`StandardCharsets.UTF_8` など）。