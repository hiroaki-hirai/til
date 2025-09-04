# javac コマンド

# Q,

javac コマンドとそのオプションを中心に、用途・役割・使い方を一覧で整理してください。

了解しました、Hiroakiさん。

ここでは `javac` コマンドとその代表的なオプションについて、**用途・役割・使い方**を一覧形式で丁寧に整理します。

---

# ✅ `javac` コマンドとは？

> Javaのソースファイル（.java）をバイトコード（.class）にコンパイルするコマンド。
> 
> 
> 実行はせず、「クラスファイルを生成する」ためのツールです。
> 

---

# ✅ 基本構文

```java
javac [オプション] ファイル名.java
```

例：

```java
javac -d bin -cp lib/mylib.jar src/com/example/Main.java
```

---

# ✅ よく使う `javac` の主要オプション一覧

| オプション | 用途・意味 | 使用例例 |
| --- | --- | --- |
| `-d` | **出力先ディレクトリ**の指定 | `-d bin` → `.class` ファイルを `bin/` に出力 |
| `-cp` / `-classpath` | **クラスパスの指定**（外部ライブラリや依存クラスの検索パス） | `-cp lib/` → `lib/` 以下のクラス/JARを参照 |
| `-sourcepath` | `.java` ファイルの**検索場所**を指定（パッケージ構造に合わせて探索） | `-sourcepath src` → `src/com/example/*.java` など |
| `-encoding` | ソースファイルの**文字エンコーディング**指定 | `-encoding UTF-8` |
| `-source` | ソースの**記述バージョン指定**（例: Java 8 構文で書かれている） | `-source 8` |
| `-target` | 出力 `.class` の**バージョン指定**（例: Java 8 の JVM 向け） | `-target 8` |
| `--module-path` | **モジュールの探索パス**を指定（モジュールシステム用） | `--module-path mods` |
| `--module-source-path` | モジュール構造に対応したソースのルート指定 | `--module-source-path src` |
| `-verbose` | コンパイル時の詳細ログ出力 | `-verbose` |
| `-Xlint` | 潜在的な問題点の**警告を出力** | `-Xlint:unchecked` |
| `-parameters` | `.class` に**引数名の情報を埋め込む**（リフレクションで取得可能になる） | `-parameters` |

---

# ✅ 使用例まとめ

### ① 基本的なコンパイル

```java
javac Hello.java
```

➡ `Hello.class` がカレントディレクトリに出力されます。

---

### ② 出力先を指定（`d`）

```java
javac -d bin src/Hello.java
```

➡ `bin/` に `Hello.class` を出力。

---

### ③ 外部JARファイルを使う（`cp`）

```java
javac -cp lib/library.jar -d bin src/MyApp.java
```

➡ ライブラリを参照しつつコンパイル。

---

### ④ モジュール付きプロジェクト（Java 9+）

```java
javac --module-source-path src -d mods $(find src -name "*.java")
```

➡ `module-info.java` を含めた**モジュール構成**でビルド。

---

# ✅ 補足：Javaバージョンと互換性

Java 11 以降では、`-source` や `-target` を組み合わせて古いバージョンに対応可能です。

```java
javac -source 8 -target 8 -d bin Main.java
```

> ✅ Java 11で書かれたコードを Java 8用にコンパイルできます（※APIの利用制限には注意）
> 

---

# ✅ まとめ

| 区分 | オプション | 一言まとめ |
| --- | --- | --- |
| 出力指定 | `-d` | `.class` の出力先ディレクトリを指定 |
| 参照先指定 | `-cp` / `-classpath` | 依存 `.class` や `.jar` の検索パスを指定 |
| ソース制御 | `-sourcepath` | `.java` の検索対象ルートを指定 |
| バージョン | `-source`, `-target` | 記述バージョンと生成バージョンをコントロール |
| モジュール系 | `--module-path` | モジュールパスを指定（Java 9以降） |
| その他 | `-verbose`, `-Xlint` | 詳細出力や警告表示などの補助機能 |

---

必要であれば：

- `javac` + `java` 実行フローの図解
- 試験や実務でよく使う「最小セットのコマンドレシピ集」
- EclipseやMavenでの `javac` 相当の動作

などもご案内できます。ご希望ありますか？

[-d](javac%20%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%2023562cef7350808b94ffe6270fefcb66/-d%2023562cef7350802f8082c4f53deb05b3.md)