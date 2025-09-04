# Streamとの接点

# Q,

StreamAPIでは、コレクションからStreamオブジェクトを作成して、中間処理があればその処理をして終端操作で出力するなりコレクションに戻すなりしていました。
ファイルの入出力もこれに似た流れ、ファイル読み込み→ファイルの中身の操作→ファイルへの出力といった工程をたどることを想定されているのでしょうか？

はい、Hiroakiさんの認識は非常に的確です。

**Stream API の「流れ（パイプライン）」の考え方と、ファイルの入出力処理の工程は、本質的にとてもよく似ています。**

特に **NIO.2（`java.nio.file.*`）+ Stream API** を組み合わせると、

「ファイル → データ処理 → ファイル/コンソールなどへの出力」

という一連の流れが **非常に Stream 的なスタイル** で記述できます。

---

## ✅ Stream API の基本パイプライン

```java
collection.stream()      // 🔹 入力（データの流れの開始）
          .filter(...)   // 🔸 中間処理（変換・絞り込み）
          .map(...)      
          .collect(...)  // 🔹 終端処理（出力・集約・再構成）
```

---

## ✅ ファイル入出力のパイプライン的構造

```java
Files.lines(Path)               // 🔹 入力（ファイルから Stream<String>）
     .filter(...)               // 🔸 中間処理（データ操作）
     .map(...)                  
     .collect(Collectors.toList()) // 🔹 終端処理（集約 or 書き出しに備える）

Files.write(Path, List<String>) // 🔹 出力（ファイルへ書き出す）
```

---

## 🔸 例：ファイルから読み込んで加工して別ファイルへ出力

```java
Path input = Paths.get("input.txt");
Path output = Paths.get("output.txt");

try (Stream<String> lines = Files.lines(input)) {
    List<String> modified = lines
        .map(String::toUpperCase)
        .filter(s -> !s.isBlank())
        .collect(Collectors.toList());

    Files.write(output, modified);
} catch (IOException e) {
    e.printStackTrace();
}
```

- `Files.lines()` → `Stream<String>` を作る（入力）
- `map`, `filter` などで加工（中間処理）
- `collect(Collectors.toList())` で保持（終端）
- `Files.write()` でファイル出力（出口）

---

## ✅ 類似点と意図的な設計思想

| Stream API | ファイルI/O |
| --- | --- |
| `stream()` | `Files.lines()`, `BufferedReader.lines()` |
| `filter`, `map`, `peek` | データ加工、ログ出力 |
| `collect`, `forEach` | 結果集約 or ファイル/標準出力へ |

→ Java の設計思想として、「データは流れるように処理されるべき」という思想が両方に流れています。

---

## ✅ NIO + Stream が想定されるユースケース

- 大量のログファイルの分析・抽出
- CSVファイルのフィルタリングや整形
- 改行単位でのデータ検証・変換
- ファイル比較や一括変換

---

## ✅ まとめ：質問に対する回答

> ファイルの入出力も StreamAPI に似たパイプライン構造を意図して設計されているのか？
> 

→ **はい、その通りです。**

Java 8 以降では **Stream のように「宣言的・関数型スタイル」でファイルを読み取り・加工・出力することが想定されており、API 設計もそれに沿っています。**

---

ご希望があれば、具体的な応用例（CSVのフィルタ・整形、ログの集計、巨大ファイルの1行ずつ処理）などもお出しできます。気になるユースケースはありますか？