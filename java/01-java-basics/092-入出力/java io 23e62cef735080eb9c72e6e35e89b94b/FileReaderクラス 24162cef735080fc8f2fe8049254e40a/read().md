# read()

# Q,

FileReaderクラスのread()について解説してください。

はい、`FileReader` クラスの `read()` メソッドは、**テキストファイル（文字データ）を読み込むための基本メソッド**です。

`FileReader` は `Reader` クラスを継承しており、**ファイルの文字入力**に特化しています。

---

## ✅ `FileReader` とは？

- クラス：`java.io.FileReader`
- 親クラス：`java.io.InputStreamReader` → `java.io.Reader`
- 用途：**ファイルから文字単位で読み込む**（エンコーディングはデフォルトでプラットフォーム依存）

---

## ✅ 主な `read()` メソッド（オーバーロード）

| メソッドシグネチャ | 説明 |
| --- | --- |
| `int read()` | 1文字読み込んで、Unicode（0〜65535）の int 値で返す。 |
| `int read(char[] cbuf)` | 複数文字を配列に読み込む。読み込んだ文字数を返す。 |
| `int read(char[] cbuf, int off, int len)` | 指定した範囲に読み込む。offset から最大 len 文字まで。 |

```java
FileReader reader = new FileReader("example.txt");
int ch = reader.read();  // 1文字読み込み（int 値）

while (ch != -1) {
    System.out.print((char) ch);  // char にキャストして表示
    ch = reader.read();
}
reader.close();

```

### ✅ 補足：

- **戻り値：読み込んだ文字（Unicode 値）**
- **ファイルの終わりに達すると -`1` を返す**
- 読み込んだ値は `int` 型だが、実質は 1 文字

---

## 🔸 2. `int read(char[] cbuf)`

```java
char[] buffer = new char[100];
int readChars = reader.read(buffer);  // 最大100文字まで読み込む
```

- 読み込んだ文字数を返す（-`1` は EOF）
- 読み込んだ結果は `buffer` に格納される

---

## ✅ EOF（end of file）制御のポイント

```java
while ((ch = reader.read()) != -1) {
    // 読み込み処理
}
```

- `read()` 系メソッドは**末尾に達すると -`1` を返す**ので、これがループ終了の合図
- ファイルのサイズに合わせて動的に読み込める

---

## ✅ `read()` 使用時の注意点

| 注意点 | 内容 |
| --- | --- |
| 文字コード | デフォルトはプラットフォーム依存 → 明示的に指定したい場合は `InputStreamReader` 使用 |
| バッファサイズが小さいと非効率 | 長文・大容量ファイルでは `BufferedReader` と組み合わせると効率的 |
| 読み込み結果の int 値の意味 | `char` にキャストして文字として扱う必要がある |

---

## ✅ 実用的にはどう使う？

```java
try (FileReader reader = new FileReader("sample.txt");
     BufferedReader br = new BufferedReader(reader)) {

    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }

} catch (IOException e) {
    e.printStackTrace();
}
```

- 実務では **`BufferedReader` + `readLine()`** を併用するのが一般的です。

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.io.FileReader`（`Reader`のサブクラス） |
| 主なメソッド | `read()`、`read(char[])` |
| 処理単位 | 1文字または複数文字 |
| 戻り値 | 読み込んだ文字（int） or 読み込み数 |
| 終了条件 | EOF 到達時は `-1` を返す |
| 実務での利用法 | `BufferedReader` と組み合わせるのが主流 |

---

ご希望であれば、`InputStreamReader` との違いや、文字コードを指定するパターン (`new InputStreamReader(new FileInputStream(...), "UTF-8")`) も解説できます！