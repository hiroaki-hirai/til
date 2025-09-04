# java コマンド

# Q、

java -cp などコマンドをその役割などともにまとめてください。

了解しました！

ここでは Java の代表的なコマンドのうち、`java` コマンドとそのオプション（特に `-cp`）を中心に、**用途・役割・使い方を一覧で整理**します。

---

## ✅ `java` コマンドの概要

> java は .class や .jar ファイルを実行するためのコマンドです。
> 
> 
> Javaバイトコードを JVM 上で実行します。
> 

```java
java [オプション] クラス名 または -jar ファイル名
```

---

## 🔷 よく使うオプション一覧とその役割

| コマンド | 役割・意味 |
| --- | --- |
| `java -cp` または `java -classpath` | **クラスパスを指定**。クラスファイルやJARファイルの場所を教える |
| `java -jar` | 実行可能なJARファイルを実行。`MANIFEST.MF` に記載された Main-Class を使う |
| `java -version` | Javaのバージョンを表示 |
| `java --module-path` | Java 9以降のモジュールパス（JPMS用）。モジュールを使う場合に指定 |
| `java --enable-preview` | 新しい言語機能のプレビューを有効にする（例：Javaの新構文など） |
| `java -Dキー=値` | システムプロパティの定義（アプリ内で `System.getProperty()` で取得可能） |

---

## 🔶 `cp` / `classpath` の使い方詳細

### 📌 目的：

Java がクラスやJARを探す **検索パス（classpath）** を明示的に指定する

---

### 📄 構文例

```java
java -cp bin com.example.Main
java -cp lib/gson.jar:bin com.example.MyApp       # Linux/macOS
java -cp lib\gson.jar;bin com.example.MyApp       # Windows
```

| パスの意味 | 説明 |
| --- | --- |
| `bin` | 自分でコンパイルした `.class` の場所 |
| `lib/gson.jar` | 外部ライブラリJAR |
| `:` または `;` | パス区切り（OSによって異なる） |

---

## ✅ `jar` オプションと `cp` の違い

| オプション | 用途 | 注意点 |
| --- | --- | --- |
| `-cp` / `-classpath` | `.class` や `.jar` を直接指定して起動 | `Main-Class` を指定する必要がある |
| `-jar` | `実行可能JARファイル`を起動 | JARに `MANIFEST.MF` で `Main-Class` が必要 |

---

### 🔸 例：実行可能JARの起動

```java
java -jar MyApp.jar
```

※ `MyApp.jar` の中に `META-INF/MANIFEST.MF` があり、

```java
Main-Class: com.example.MyApp
```

のように指定されている必要があります。

---

## 🧩 補足：`java` vs `javac` の役割の違い

| コマンド | 役割 | 入力 | 出力 |
| --- | --- | --- | --- |
| `javac` | コンパイル | `.java` ソース | `.class` バイトコード |
| `java` | 実行 | `.class` や `.jar` | プログラム実行 |

---

## ✅ まとめ：`java` コマンドと主なオプション

| コマンド例 | 目的 | 備考 |
| --- | --- | --- |
| `java -cp bin com.example.Main` | `.class` を直接実行 | パスの指定が必要 |
| `java -cp lib/foo.jar:bin com.App` | 外部ライブラリ付き実行 | JARの依存関係を含める |
| `java -jar MyApp.jar` | 実行可能JARの実行 | JAR内に `Main-Class` が必要 |
| `java -version` | Javaのバージョン確認 | 実行環境の確認用 |
| `java --module-path out -m my.module/com.example.Main` | モジュールの実行 | Java 9以降のモジュールシステム用 |

---

## 🔧 おまけ：開発中によく使う流れ

```java
# ① コンパイル
javac -d bin -cp lib/gson.jar src/com/example/MyApp.java

# ② 実行
java -cp lib/gson.jar:bin com.example.MyApp
```

---

ご希望があれば：

- `MANIFEST.MF` の書き方
- Gradle/Maven を使った場合の `java` 実行方法
- モジュール環境での `-module-path` の使い方

などもご説明できます！

# Q、

windows のファイル管理に関するコマンドと対比できますか？

はい、できます！

ここでは、Javaのコマンド（`java`, `javac`, `-cp` など）と、**Windowsのファイル管理系コマンド（`cd`, `dir`, `copy` など）**を対比して、**役割の違いや連携の仕方**をわかりやすく整理します。

---

## 🔷 Javaコマンド vs Windowsファイル操作コマンド：対比表

| Javaコマンド例 | 役割 | 対応するWindowsコマンド例 | 役割 |
| --- | --- | --- | --- |
| `javac MyApp.java` | Javaソースのコンパイル | `copy`, `xcopy` | ファイルのコピー（生成物の操作に近い） |
| `java com.example.Main` | Javaプログラムの実行 | `start`, `call`, `run` | 他のプログラムを実行する |
| `java -cp lib/*;bin MainClass` | クラス/JARのパス指定 | `cd`, `set PATH` | ファイル検索パスや作業ディレクトリの操作 |
| `jar cvf app.jar -C bin .` | JARファイルの作成 | `zip`, `copy`, `ren` | ファイル圧縮、移動、リネームなど |
| `java -version` | Java実行環境の確認 | `ver`, `systeminfo` | Windowsシステムやバージョンの確認 |
| `javac -d bin src\MyApp.java` | 出力ディレクトリ指定 | `mkdir`, `move` | ディレクトリ作成・ファイル移動 |

---

## 🔶 Java開発とWindows操作の対応関係（流れに沿って）

| ステップ | Javaでの操作 | Windowsでの補助的操作（例） |
| --- | --- | --- |
| ① ソースの保存 | `src\com\example\Main.java` | `mkdir src\com\example` |
| ② コンパイル | `javac -d bin src\com\example\Main.java` | `dir`, `mkdir bin` |
| ③ 実行 | `java -cp bin com.example.Main` | `cd bin`, `echo`, `cls` |
| ④ 配布 | `jar cvf MyApp.jar -C bin .` | `copy MyApp.jar D:\app\` |

---

## 🔍 特に混同しやすいもの：`cp` と `cd`

| コマンド | 目的 | 備考 |
| --- | --- | --- |
| `-cp` / `-classpath`（Java） | Javaの**クラス検索パス**の指定 | これは**実行対象を探すパス** |
| `cd`（Windows） | **現在の作業ディレクトリ**の移動 | これは「どこで操作するか」を切り替える |

💡 `-cp` でパスを指定しても、**実行中のカレントディレクトリは変わりません**。

→ `cd` は作業フォルダの移動、`-cp` は**Javaのファイル検索対象**の指定です。

---

## ✅ まとめ：目的別に見る対応

| 操作目的 | Javaコマンド | Windowsコマンド |
| --- | --- | --- |
| Javaコードの実行 | `java` | `start`, `call`（他プログラム実行） |
| クラスパス指定 | `-cp`, `-classpath` | `set PATH`, `cd`（検索パス制御） |
| コンパイル | `javac` | `copy`, `move`（生成物の操作） |
| 出力JARの作成 | `jar` | `zip`, `ren`, `mkdir` |
| 作業ディレクトリの操作 | ― | `cd`, `dir`, `mkdir` |
| 実行環境の確認 | `java -version` | `ver`, `systeminfo` |

---

## 🚀 実用的な組み合わせ例（Windows＋Java）

```java
cd D:\MyJavaProject
mkdir bin
javac -d bin src\com\example\Main.java
java -cp bin com.example.Main
```

---

必要であれば、**バッチファイル（`.bat`）にまとめる方法**や、**PowerShellでの実行の仕方**などもご説明できます！

興味ありますか？

[-cp](java%20%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%2023562cef735080bd8fffe2e1ed1f8975/-cp%2023562cef73508019867defcf1649ae2a.md)