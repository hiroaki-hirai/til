# mods

# Q,

```java
java --module-path mods -m com.example.myapp/com.example.myapp.Main
```

まずは、上記解説中の mods はどの階層のファイル？フォルダ？なのかが知りたいです。
あと、module-info.class と module-info.java は何なのか？教えて下さい。

とても良いご質問です、Hiroakiさん。

モジュールシステムを実行する上で重要な `"mods"` の正体と、`module-info.java` / `module-info.class` の違いを丁寧に解説します。

---

## ✅ ① `-module-path mods` の `mods` とは何か？

### 📌 結論：

> mods は、モジュール化されたクラスファイルやJARファイルを格納したディレクトリ（フォルダ）です。
> 
> 
> モジュールシステムにおける**検索対象のルートディレクトリ**を指定しています。
> 

---

### 📁 例：プロジェクト構成イメージ

```java
project-root/
├── mods/                       ← ← ← これが module-path（フォルダ）
│   └── com.example.myapp/      ← モジュール名と一致
│       ├── module-info.class   ← モジュール定義（コンパイル済）
│       └── com/example/myapp/
│           └── Main.class
```

### ✅ つまり：

```java
--module-path mods
```

とは：

> 「この mods フォルダ内にある **モジュール定義済みのクラスファイルやJARファイルを探索対象とします」」という指定です。
> 

---

## ✅ ② `module-info.java` と `module-info.class` の違い

| ファイル名 | 役割 |
| --- | --- |
| `module-info.java` | モジュールの**ソースファイル**。依存関係や公開範囲を記述する。 |
| `module-info.class` | `module-info.java` を `javac` で**コンパイルしたバイトコード**。 |

---

### 📄 module-info.java の中身（例）

```java
module com.example.myapp {
    requires java.sql;
    exports com.example.myapp.api;
}
```

これは **モジュールの仕様書のようなもの**です。

- どのモジュールに依存するか（`requires`）
- どのパッケージを外部に公開するか（`exports`）

---

### 🧾 module-info.class の生成方法

```java
javac -d mods/com.example.myapp \
      src/com.example.myapp/module-info.java \
      src/com.example.myapp/com/example/myapp/*.java
```

- `d`：出力先ディレクトリ
- `mods/com.example.myapp`：このフォルダに `module-info.class` と `.class` を配置
- `javac` が `module-info.java` をコンパイルして、`module-info.class` を出力

---

### ⚠️ モジュールとして有効になるには：

> module-info.class がそのモジュールのルートにある必要があります。
> 

```java
mods/
└── com.example.myapp/        ← モジュール名と一致
    ├── module-info.class     ← 必須！
    └── com/example/myapp/    ← パッケージ構造
        └── Main.class
```

---

## ✅ 全体まとめ

| 項目 | 内容 |
| --- | --- |
| `mods` | モジュールクラスファイルやモジュールJARを格納した**フォルダ** |
| `--module-path mods` | Javaに「このフォルダでモジュールを探せ」と指示 |
| `module-info.java` | モジュールの構成を**ソースコードで記述**するファイル |
| `module-info.class` | 上記ソースを `javac` でコンパイルした**バイトコード（実行時に使う）** |

---

ご希望があれば：

- この `mods` フォルダを自動生成する Gradle/Maven の設定例
- 自分で手作業で `.java → .class → jar` にしてモジュールとして動かす一連の流れ
- `module-info.java` を使った実用的な multi-module の構成例

などもご案内できます！どうしましょうか？

# Q,

モジュールシステムを利用しようとすると初期設定でmodsフォルダが作成されるということでしょうか？
ルートフォルダということは、モジュールを作成すれば、この mods が src のように自動で作成されるという認識です。

とても良い着眼点です！

ただ、ここは少し **注意が必要なポイント**でもあります。

---

## ✅ 結論

> mods フォルダは自動で作成されるわけではありません。
> 
> 
> モジュールシステムを使う場合でも、**`mods` はあくまで**「**開発者が任意で使う出力先ディレクトリ名の一例**」です。
> 
> つまり、**JavaやEclipseなどが `mods` を自動で作るわけではありません**。
> 

---

## 🔍 なぜ `mods` という名前がよく使われるのか？

- 教材・解説サイト・書籍などで、**「module の略」**として `mods` というディレクトリ名がよく使われているだけです。
- Java公式ツール（`javac`, `java`）では**任意のフォルダ名が使えます**。

### 📄 例：自分で作る場合

```java
mkdir -p mods/com.example.myapp
javac -d mods/com.example.myapp \
      src/com.example.myapp/module-info.java \
      src/com.example.myapp/com/example/myapp/*.java
```

---

## ✅ `mods` は「src」とは意味が違う

| フォルダ名 | 自動作成される？ | 意味・役割 |
| --- | --- | --- |
| `src` | ✅ IDEで作ると自動 | Javaの**ソースコード**を置くフォルダ |
| `mods` | ❌ 自分で作る | **モジュール単位での `.class` ファイル出力先**としてよく使われる |

> mods は「モジュール化された .class ファイルの配置場所」として使うことが多いですが、ルールで決まっているわけではありません。
> 

---

## 🔧 Eclipse の場合（補足）

Eclipse でモジュールプロジェクトを作っても：

- `src/` は作られます（通常のソースフォルダ）
- `module-info.java` もテンプレートで生成されます
- ただし `mods/` というフォルダは**自動では作成されません**

🔸 Eclipseでは、モジュールのビルド出力先は `bin/` フォルダなどに設定されているのが普通です。

（この出力先はプロジェクトプロパティで変更可能）

---

## ✅ まとめ：Hiroakiさんの認識の整理

| 質問項目 | 回答 |
| --- | --- |
| モジュールを作ると `mods` が自動生成される？ | ❌ 自動では作られません。開発者が任意に使う名前です。 |
| `mods` はルートフォルダのような扱い？ | ✅ よく「モジュールごとの出力先」として使われますが、決まりではありません |
| `src` と同じような自動構成フォルダか？ | ❌ いいえ。`src` はIDEが構成上作りますが `mods` は手動です。 |

---

ご希望であれば：

- `javac` 〜 `java` コマンドで `mods` を使ってモジュールをビルド＆実行する手順
- Eclipseでモジュール構成を作成し、出力先を明示的に確認する方法
- Maven/Gradleでのモジュール出力構成の指定例

などもご案内できます。どうしましょうか？