# classpath

# Q,

モジュールシステム以前のclasspathとはどのようなファイル管理方法でしょうか？

とても良い質問です！

Javaの**モジュールシステム以前の `classpath` とは何か？**

これは Java の**伝統的なファイル管理・依存管理の仕組み**です。

---

## ✅ 一言でいうと：

> classpath は Java に「どこにクラスファイルやJARがあるか」を教えるための設定方法です。
> 
> 
> モジュールシステムができる前は、この `classpath` を使ってクラスやライブラリを読み込んでいました。
> 

---

## 🔸 classpath の役割

- Javaコンパイラ（`javac`）やJava実行環境（`java`）が
    
    **「どの場所からクラスファイルやライブラリを探すか」** を定義します。
    

---

## ✅ 基本の使い方

```java
javac -classpath lib/mylib.jar MyApp.java
java -classpath lib/mylib.jar:. MyApp
```

- `lib/mylib.jar`：外部ライブラリのJAR
- `.`：カレントディレクトリ（自分のクラスファイル）

---

## 🔍 classpath の特徴

| 特徴 | 説明 |
| --- | --- |
| 📂 フォルダやJARを列挙するだけ | 階層や依存関係を定義しない |
| 🔄 重複や衝突に弱い | 同名のクラスが複数JARにあるとどれが使われるか不明（JAR地獄） |
| 🔒 カプセル化できない | クラス単位で全てアクセス可能（非公開にできない） |
| ⚙ 自動解決がない | 依存関係の解決・順序は人が手動で調整する |

---

## ✅ 例：簡単な構成（classpath）

```java
project/
├── lib/
│   └── gson-2.10.jar
├── src/
│   └── MyApp.java
```

### 実行方法（コマンド）

```java
javac -cp lib/gson-2.10.jar src/MyApp.java
java -cp lib/gson-2.10.jar:src MyApp
```

---

## 🆚 classpath とモジュールシステムの比較

| 項目 | classpath（従来） | モジュールシステム（Java 9+） |
| --- | --- | --- |
| 依存関係の定義 | なし（JARを並べるだけ） | `requires` で明示的に定義 |
| 公開範囲の制御 | なし（すべてのパッケージが公開） | `exports` で制御可能 |
| クラスの衝突 | 起こる（どのJARが優先されるか不明） | モジュール名で管理、重複を防げる |
| ツールによる最適化 | 困難 | `jlink` で最小構成JREを構築可能 |
| SpringやMavenとの相性 | ◎ 非常に一般的 | △ 対応が限定的・制限がある |

---

## ✅ 結論

> classpath は、JavaでどのクラスファイルやJARファイルを使うかを手動で指定する仕組みです。
> 
> 
> モジュールシステム以前は、すべてのJavaプロジェクトがこの方式を使っていました。
> 
> 現在でも多くのプロジェクトが `classpath` を使い続けています（特にSpringやMaven環境）。
> 

---

ご希望があれば：

- Maven/Gradleがどのようにclasspathを扱っているか
- EclipseやIDEが自動でclasspathをどう構築しているか
- classpath を用いた開発のベストプラクティス

などもご説明できます！

[準備](classpath%2023562cef735080f4ae38c25c159ff7b8/%E6%BA%96%E5%82%99%2023562cef73508050a499dedf1041c689.md)

[java コマンド](classpath%2023562cef735080f4ae38c25c159ff7b8/java%20%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%2023562cef735080bd8fffe2e1ed1f8975.md)

[javac コマンド](classpath%2023562cef735080f4ae38c25c159ff7b8/javac%20%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%2023562cef7350808b94ffe6270fefcb66.md)