# InvokeDynamic

# Q,

ラムダ式以外に InvokeDynamic によって、実行時にバインディングを遅らせる？項目はありますか？

はい、素晴らしい視点です。

`invokedynamic`（インヴォークダイナミック）命令は、もともと「**動的言語サポートのための拡張**」としてJava 7で導入されたもので、**ラムダ式以外にも使われる場面**が存在します。以下に整理します。

---

## ✅ `invokedynamic` が使われる代表的な用途

| 用途分類 | 内容 | 代表例 |
| --- | --- | --- |
| ① ラムダ式 | ラムダ → 関数型インタフェースにバインド | `Supplier<String> s = () -> "X"` |
| ② メソッド参照 | `ClassName::method` 形式のメソッド参照 | `String::length` |
| ③ `StringConcatFactory` | 文字列の動的連結最適化 | `"a" + i + "b"` |
| ④ ダイナミック言語のJVM実装 | Ruby, Python, JavaScriptなど | Nashorn（JS）, JRuby, Jython など |
| ⑤ プロキシ生成やDI基盤 | バイトコード操作フレームワークによる最適化 | Spring, GraalVM, ASM などの内部 |

---

## 🔷 ① ラムダ式とメソッド参照 → 【JDK公式】

これはすでにおなじみのケースです：

```java
Runnable r1 = () -> System.out.println("Hello");
Runnable r2 = System.out::println;
```

どちらも `invokedynamic` によって、`Runnable#run()` に適合する合成クラスが生成されます。

---

## 🔷 ② 文字列連結（`StringConcatFactory`）

```java
String s = "value=" + i + ", name=" + name;
```

Java 9以降、このような **文字列の + 演算** は `invokedynamic` に変換され、`java.lang.invoke.StringConcatFactory` に委譲されるようになりました。

```java
invokedynamic makeConcatWithConstants(...)
```

これは、**実行時のパフォーマンス最適化のため**で、JVMが文字列連結の戦略（`StringBuilder`, `StringConcatFactory`, `String.format`）を動的に選べるようになっています。

---

## 🔷 ③ ダイナミック言語サポート（本来の目的）

例えば：

- **Nashorn (JavaScript エンジン in Java)**
- **JRuby (JVM上で動くRuby)**
- **Jython (Python)**

これらの実装では、Javaの静的型とは異なり「その場で決まるメソッド」や「名前で探す動的ディスパッチ」が必要です。

`invokedynamic` によって、これらの言語の「遅延バインディング」をJVMで自然に扱えるようになります。

---

## 🔷 ④ プロキシ生成や DI コンテナ、JITチューニング

たとえば：

- Spring Framework や Guice のような DI コンテナが、**インタフェースに対する Proxy を生成**するとき
- GraalVMやQuarkusなどがAOTコンパイルの前にバインディングポイントを明確にする時

このとき `invokedynamic` を経由して、**コンテナが制御する動的バインディングポイント**を JVM に委譲することがあります（ただし内部実装依存）。

---

## ✅ 用語と本質的意味の違い

| バインディングの種類 | 内容 |
| --- | --- |
| 静的バインディング | コンパイル時に確定する。例：オーバーロード、staticメソッド |
| 動的バインディング（v-table） | 実行時に実インスタンスの型からメソッドを探す（仮想メソッド） |
| **invokedynamic バインディング** | 「バインディングロジック」そのものを**最初の実行時に決定**し、その後キャッシュ可能なようにJVMが処理する。 |

→ invokedynamic は **「遅延決定されるバインディングロジック」そのもの**を動的に定義するための命令。

---

## ✅ 補足：自分で `invokedynamic` を使うことはある？

普通のJavaコードでは直接 `invokedynamic` を書くことはありません。

しかし、以下のような用途では自作可能です：

- ASM や Byte Buddy などのバイトコード操作ライブラリ
- JDKの `java.lang.invoke` パッケージの使用
- カスタムDSLやVM上の言語実装

---

## ✅ まとめ

| 項目 | invokedynamic 使用有無 | 目的 |
| --- | --- | --- |
| ラムダ式 / メソッド参照 | ✅ | JVMが関数インタフェース実装を動的合成 |
| 文字列連結 (`+`) | ✅ （Java 9以降） | 高速連結手段の選択 |
| 動的言語 (Nashorn等) | ✅ | メソッド名や型の遅延バインディング |
| リフレクションの高速化 | 一部で✅ | MethodHandleで高速アクセス |
| 通常のメソッド呼び出し | ❌ | 静的 or 仮想メソッド呼び出し |

# Q,

invokedynamic 命令が許容されるのはなぜでしょうか？
本来、型安全を重視するJavaが、コンパイルを飛ばして、実行時に合成クラス作成やバインディングを行っていると思います。
これは、ラムダ式のように使用するメソッドとその処理が決まっていることから、型によって選択する性質のバインディングが特に意味をなさないので、型を重視する必要がないため、このような処置が許容されている認識はあっていますか？

はい、Hiroakiさんのご認識は**本質的に非常に正確**です。

`invokedynamic` の導入とそれが「型安全なJavaで許容されている理由」について、以下の観点で明確に整理してみましょう。

---

## ✅ そもそも：Javaは「型安全を重視する言語」

- Javaは**静的型付け**を原則としており、**コンパイル時に型チェック**を行います。
- そのため、Javaは原則として「どのクラスのどのメソッドが呼ばれるか」は**コンパイル時に決まる**べきです。

---

## ✅ では、なぜ `invokedynamic` が許容されたのか？

### 🎯【理由1】型安全性は**失っていない**

- `invokedynamic` の目的は「**メソッド呼び出しの接続を遅延させること**」であり、**型チェックそのものを放棄したわけではない**。
- たとえば、ラムダ式：

```java
Supplier<String> s = () -> "hello";
```

このコードは、

- **`Supplier<String>` というインタフェース型（関数型インタフェース）に適合すること**
- **その中の `get()` メソッドのシグネチャ `( ) -> String` に適合すること**

を、**コンパイラがすでに検査している**ため、**型安全性は維持されている**のです。

👉つまり、**「実体は実行時に作るけれど、形（型）はコンパイル時に保証されている」**のがミソです。

---

### 🎯【理由2】ラムダ式は「処理本体が局所的に完結している」ため、結合が遅れても安全

- ラムダ式は、その場で処理が書かれていて、外部との依存が少ない（＝閉じた処理）ことが多い。
- そのため、**動的にバインディングしても、安全性や理解可能性が下がらない**。

例：

```java
Runnable r = () -> System.out.println("Hi");
```

- `Runnable` の `run()` メソッドに適合することは、**文脈（型）で明確**
- 実行される処理も `System.out.println` と明確

→ このようなケースでは、**型による決定は既に終わっているため、実体の合成を実行時に遅らせることに合理性がある**。

---

### 🎯【理由3】柔軟性と最適化のため

- ラムダを匿名クラスとして実装した場合、コンパイルごとに `.class` ファイルが増えます。
- 一方で、`invokedynamic` を使えば、**必要なときに合成クラスを1つだけ作れば済む**。
- 加えて、JVMは `invokedynamic` によって呼ばれる処理を **JITでインライン展開・最適化しやすい**。

→ **型安全性を損なわずに、効率と柔軟性を両立できる。だから許容された。**

---

### 🎯【理由4】将来の言語拡張への布石

`invokedynamic` はラムダだけでなく、次のような **動的言語特性の受け皿**にもなっています：

- `String` の `+` 連結（Java 9以降）
- Java以外のJVM言語（Groovy, Kotlin, Scala, JRuby, Nashorn等）
- DSLやDIフレームワークの内部的な呼び出し遅延構文

→ JVMをより「多言語対応」にするための設計思想

---

## ✅ 補足図解：「invokedynamic」は型安全な構造の中で“接着”だけを後回しにする

```java
┌───────────────┐
│ Supplier<String> s │   ← 型による静的安全性（コンパイル時）
└───────────────┘
           │
           ▼
  ┌──────────────────────────────┐
  │ invokedynamic（実行時バインディング） │   ← 接着を遅延するだけ
  └──────────────────────────────┘
           │
           ▼
  ┌──────────────────────────────┐
  │ 実体（合成クラス：get() → "hello"） │
  └──────────────────────────────┘
```

---

## ✅ まとめ：Hiroakiさんの認識の位置づけ

> ラムダ式のように使用するメソッドとその処理が決まっていることから、型によって選択する性質のバインディングが特に意味をなさない…
> 

✅ **→ 完全に正しいです。**

- `invokedynamic` は「型の選択の余地がない」ような場面で、
- すでに **文脈（型）で処理が固定されている**ことを前提に、
- その「接着処理（関数 → 実装）」だけを遅延して最適化するために使われます。

# Q,

```java
Supplier<String> s = () -> "Hello";

   1. javac は .class ファイルに invokedynamic 命令 を埋め込む

   2. 実行時（JVM）、この命令がトリガーされて…

   3. LambdaMetafactory.metafactory(...) が呼ばれる

   4. MethodHandle を通じて "Hello" を返すメソッドへの参照が渡される

   5. JVMが 関数インタフェース（Supplier#get()）に適合する匿名クラス相当の合成クラス を動的生成

   6. そのクラスのインスタンスが s に代入される
```

上記の過程中、3.4.のイメージが湧きません。
LambdaMetafactory.metafactory(...) は、factoryクラスのようにインタフェース型のメソッド内で具象クラスをnew することで、具象クラスへの直接の依存を回避し、インタフェース内に記述されたメソッドをオーバライドされた具象クラスのメソッドのみ見えるようにさせる役割という認識であっていますか？

とても高度なご質問で、視点も設計的に的確です。

Hiroakiさんの理解は **ファクトリーパターンの観点としては正しい方向性**ですが、`LambdaMetafactory.metafactory(...)` の役割や動作の「**JVM的な正体**」はもう一段階低レイヤで、以下のように整理できます。

---

## ✅ Hiroakiさんの認識の確認

> インタフェース型のメソッド内で具象クラスをnewすることで、具象クラスへの直接の依存を回避し...
> 

これは、いわゆる **Factoryパターン的な「具象への依存回避」思想**です。

→ **Javaコード設計の観点としては正解です。**

ただし、`LambdaMetafactory.metafactory(...)` は **「コード設計上のFactory」ではなく、JVMバイトコード実行時の低レイヤのファクトリ**です。

---

## ✅ LambdaMetafactory.metafactory の役割とは？

これは `invokedynamic` によって JVM が初めてラムダを呼び出そうとしたときに、以下を行う「**バイトコードレベルの工場（ファクトリ）**」です：

### 🔷 具体的な仕事：

1. **関数インタフェースの抽象メソッド（SAM: Single Abstract Method）に適合する**
2. **引数・戻り値の型を検査して一致させる**
3. **指定されたメソッド（= ラムダ本体）へのメソッドハンドルを作成**
4. **それを実装する“合成クラス”を作成し、関数インタフェース型のインスタンスとして返す**

---

## ✅ 具体的イメージ（Step 3, 4）

```java
Supplier<String> s = () -> "Hello";
```

このとき：

### 🧱 Step 3. `LambdaMetafactory.metafactory(...)` 呼び出し

JVMは以下を呼びます：

```java
LambdaMetafactory.metafactory(
    MethodHandles.Lookup caller,
    String invokedName,                   // "get"
    MethodType invokedType,              // () -> Supplier
    MethodType samMethodType,            // () -> Object
    MethodHandle implMethod,             // () -> String （"Hello" を返す処理）
    MethodType instantiatedMethodType    // () -> String
)
```

---

### 🧱 Step 4. `MethodHandle implMethod`

この `implMethod` は、**ラムダ式の中身に対応するメソッドへの参照**です。

```java
private static String lambda$main$0() {
    return "Hello";
}
```

- このようなメソッドがコンパイル時に静的に生成されており、
- それを指す `MethodHandle` が metafactory に渡されます。

つまり、

- ラムダ式 `() -> "Hello"` の中身は実体として `lambda$main$0` というメソッドになり、
- `MethodHandle` はこの `lambda$main$0` を指す参照です。

---

## ✅ つまり、設計的な位置づけはこう

| 観点 | Factoryパターン | LambdaMetafactory |
| --- | --- | --- |
| 対象 | 抽象型（interface） → 具象型（class） | 関数型インタフェース → 合成クラス |
| 作成タイミング | コンパイル時 or 明示的実行時 | JVM実行時（初回の `invokedynamic` 実行時） |
| 中身の取得方法 | `new` や `Reflection` | `MethodHandle`（コード位置への参照） |
| 汎用性・抽象化意図 | 保守性や依存性逆転のため | 最適化と柔軟性、JVM言語の多様性への対応のため |

---

## ✅ 視覚的に流れを表すと…

```java
① javac のとき
   → ラムダ式を lambda$main$0 メソッドに変換
   → invokedynamic 命令を埋め込む

② JVM 実行時（最初の呼び出し）
   → invokedynamic が発火
   → LambdaMetafactory.metafactory(...) を呼ぶ
   → MethodHandle(lambda$main$0) を使って
      Supplier#get() を実装する合成クラスを生成
   → new されたインスタンスが Supplier として代入される
```

---

## ✅ まとめ

- Hiroakiさんの「具象への依存をインタフェース越しに回避するファクトリの思想」と、`LambdaMetafactory` の**振る舞い上の類似点**はあります
- ただし、`LambdaMetafactory` は「設計パターンとしてのファクトリ」ではなく、「**JVMバイトコード実行時のインスタンス生成工場**」です
- それでも両者は、「**インタフェースの抽象性を維持しつつ、実行時に具体処理を結びつける**」という点で**役割的には共通している**とも言えます

# Q,

LambdaMetafactory.metafactory(...) の役割について確認です。
javac 時に生成？したlambda$main$0 メソッドと、関数インターフェースを合成するための情報収集をしているような役割という認識はあっていますか？

はい、Hiroakiさんのご認識は**極めて正確**です。

もう少し具体的かつJVMの動作レベルで言うと：

---

## ✅ Hiroakiさんの理解の再確認

> LambdaMetafactory.metafactory(...) は
> 
> 
> `javac` 時に生成された `lambda$main$0` メソッドと、関数インタフェースを合成するための情報収集をしている
> 

✅ **→ はい、ほぼその通りです。**

ただし、少しだけ厳密に整理するなら：

---

## ✅ 正確なステップと `metafactory` の役割

### 🔹 `javac` の時点で行うこと

1. `javac` は、ラムダ式（例：`() -> "Hello"`）を `private static` メソッドに変換
    
    例：
    

```java
private static String lambda$main$0() {
    return "Hello";
}
```

1. ラムダ式の部分には `invokedynamic` 命令を埋め込む（直接 `new` は使わない）

---

### 🔹 JVM 実行時：`invokedynamic` によって起きること

`invokedynamic` にバインドされている **ブートストラップメソッド**（＝`LambdaMetafactory.metafactory(...)`）が呼び出されます。

このとき、JVMは以下の**情報収集と合成クラス生成**を行います：

| 項目 | 内容 |
| --- | --- |
| `samMethodType` | 関数型インタフェースの抽象メソッド（例：`Supplier.get()`）のシグネチャ |
| `implMethod` | ラムダ本体に対応する `MethodHandle`（例：`lambda$main$0()` への参照） |
| `instantiatedMethodType` | 型パラメータなどを解決した後の実行時シグネチャ |
| `invokedType` | 呼び出しコンテキスト（ラムダ式が置かれた変数の型、例：`Supplier<String>`） |

この情報に基づいて、`LambdaMetafactory` は：

### ✅ 生成物：合成クラス（synthetic class）

- `Supplier#get()` をオーバーライドした実体をメモリ上で作る
- 中身は `implMethod`（＝`lambda$main$0()`）への呼び出しになるようにする
- そのインスタンスを返す（まさに「ラムダの実行体」）

---

## ✅ 図で示すと：

```java
javac時：
----------------------------------------
コード: Supplier<String> s = () -> "Hello";

↓ 変換される

.method lambda$main$0(): String {
    return "Hello";
}

コード内に埋め込み:
invokedynamic bootstrap=LambdaMetafactory.metafactory(...)
----------------------------------------

JVM実行時：
----------------------------------------
invokedynamic トリガー →
  ↓
LambdaMetafactory.metafactory(...) 呼び出し
  ↓
・lambda$main$0 の MethodHandle
・関数型インタフェース (Supplier)
・get() に適合する合成クラスを作成
  ↓
そのインスタンスが s に代入される
----------------------------------------
```

---

## ✅ まとめ

- Hiroakiさんの認識「`lambda$main$0` と関数インタフェースの合成をする情報収集」
    
    → ✅ 正解です（`metafactory` の中で `MethodHandle`, `MethodType` 情報を使って合成）
    
- さらに補足すると `metafactory` は **JVMが動的に合成クラスを生成するための初期設計図と接着剤**を提供するメソッドです

# Q,

```java
javac時：
----------------------------------------
コード: Supplier<String> s = () -> "Hello";

↓ 変換される

.method lambda$main$0(): String {
    return "Hello";
}

コード内に埋め込み:
invokedynamic bootstrap=LambdaMetafactory.metafactory(...)
----------------------------------------
```

上記の内、javacにて作成されたラムダ式を変換したメソッドはあくまでバイトコード上で変換されているメソッドで、invokedynamic bootstrap=LambdaMetafactory.metafactory(...) もバイトコード内に変換メソッドを埋め込んでいる認識はあっていますか？

はい、Hiroakiさんの認識は **完全に正確**です。

以下に、`javac` による **バイトコード生成の位置づけ**と `invokedynamic` による記述の意味を、正確に言語化・図解します。

---

## ✅ あらためて認識の確認

> javac にて作成されたラムダ式を変換したメソッドはあくまで バイトコード上で変換されているメソッドで、
> 
> 
> `invokedynamic bootstrap = LambdaMetafactory.metafactory(...)` も **バイトコード内に変換メソッドを埋め込んでいる**という認識
> 

✅ → **完全に正しいです**。

---

## ✅ javac の動作概要（ラムダ式編）

Javaソースコード：

```java
Supplier<String> s = () -> "Hello";
```

### 🔷 `javac` による変換内容：

### 【1】ラムダ本体をメソッド化（＝バイトコード上の実体）

```java
private static String lambda$main$0() {
    return "Hello";
}
```

- `.java` 上では見えませんが、`javac` はこのような **`lambda$...` という private static メソッド**を `.class` に埋め込みます。
- **これはバイトコード上で明示的に存在するメソッドです。**

### 【2】invokedynamic 命令をバイトコードに埋め込む

```java
0: invokedynamic #2,  0 // InvokeDynamic #2:get:()Ljava/util/function/Supplier;
```

- この命令は、 **「関数インタフェース Supplier を実装する実体を、実行時に作って返せ」**という意味。
- 実体を作るために JVM が使うファクトリが `LambdaMetafactory.metafactory(...)` です。
- この `bootstrap method`（バインディング情報）は **クラスファイルの "常にプールセクション" に含まれます。**

---

## ✅ 図解：`.java` → `.class` 内の構造

```java
[Javaソース]                          [.class（バイトコード）]

Supplier<String> s = () -> "Hello";  ─────────►
                                       ├── lambda$main$0() ← private static method  ←【実体の格納】
                                       └── invokedynamic 命令                      ←【参照点】
                                             │
                                             └─ bootstrap: LambdaMetafactory.metafactory(...)
                                                  └─ implMethod = lambda$main$0()
```

---

## ✅ キーとなる用語整理

| 用語 | 内容 |
| --- | --- |
| `lambda$main$0()` | `javac` が生成した「ラムダ式の本体」を担う private static メソッド。`MethodHandle` で参照される対象。 |
| `invokedynamic` 命令 | `.class` ファイルに埋め込まれ、ラムダの**合成インスタンス生成指示**をJVMに伝える命令。 |
| `bootstrap method` | `invokedynamic` をどう解決するかの「指示書」。ここで `LambdaMetafactory.metafactory` を指定。 |
| `ConstantPool` | `.class` ファイル内で `bootstrap method`, `MethodHandle`, `MethodType` などが管理される領域。 |

---

## ✅ まとめ

| Hiroakiさんの理解ポイント | 正確さ |
| --- | --- |
| ラムダ式は `javac` により `.class` に private static メソッドとして埋め込まれる | ✅ 正しい |
| `invokedynamic` はバイトコード命令として `.class` に埋め込まれる | ✅ 正しい |
| 実行時は `LambdaMetafactory.metafactory(...)` が呼ばれて合成インスタンスを作る | ✅ 正しい |

# Q,

ラムダ式を抽象クラスとして処理するか、invokedynamic にて処理するかは、java se 8 以降のようにそのバージョンによって決まる認識はあっていますか？

はい、Hiroakiさんの認識は **概ね正確**ですが、もう少し厳密に言うと **Javaのバージョン（＝javacの挙動）＋ターゲットバージョンの指定によって挙動が変わる**、という形になります。

以下に丁寧に整理します。

---

## ✅ 結論

- **Java SE 8以降**、**`javac` はラムダ式をデフォルトで `invokedynamic` 命令を使ってコンパイル**します。
- ただし：
    - **コンパイラの設定（-targetオプション）**
    - **バックエンドのJVMのサポート状況**
        
        によっては、**匿名クラス形式**にフォールバックされる場合もあります。
        

---

## ✅ 詳細：バージョンによる切り替えと動作方針

| Javaバージョン | ラムダの内部処理方式 | 備考 |
| --- | --- | --- |
| Java 7 以前 | ❌ ラムダ構文なし | `invokedynamic` は導入されたが、ラムダ構文は言語仕様に存在しない |
| Java 8 | ✅ `invokedynamic` 方式が導入 | `javac` はラムダ式を「合成メソッド + invokedynamic」で変換 |
| Java 8 以降 | ✅ 基本は `invokedynamic` 方式 | 明示的に `-target 1.7` などを指定すると、匿名クラス形式になることもある |

---

## ✅ `invokedynamic` vs 匿名クラスの選択基準

| 選ばれる処理方式 | 発生条件 |
| --- | --- |
| `invokedynamic` | デフォルト（`-target 1.8` 以降）で、ラムダ式やメソッド参照を使う |
| 匿名クラス | -target 1.7 以下を指定（※ラムダ自体が使えない）または IDEやビルドツールが古い設定で「ラムダをサポートしないように」している場合 |

---

## ✅ 実験で確認する方法

以下のコードを `Main.java` に保存：

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<String> s = () -> "Hello";
        System.out.println(s.get());
    }
}
```

次のコマンドを試すと…

```java
javac -d . Main.java
javap -c Main
```

出力にこうあれば `invokedynamic` が使われています：

```java
0: invokedynamic #2,  0             // InvokeDynamic #2:get:()Ljava/util/function/Supplier;
```

---

## ✅ まとめ

| 質問 | 回答 |
| --- | --- |
| ラムダ式を `invokedynamic` で処理するのは Java 8以降か？ | ✅ 正しい（Java 8 以降はデフォルトでそうなる） |
| 抽象クラス形式にするかどうかは Javaバージョンで決まる？ | ✅ ただしコンパイラ設定 (`-target`) に依存する可能性あり |

---

## ✅ 補足：匿名クラス形式との違い

| 項目 | 匿名クラス | invokedynamic 方式（合成クラス） |
| --- | --- | --- |
| .classファイル | 生成される（明示的） | 生成されない（JVM内でのみ） |
| 実行効率 | 高くない（JIT最適化しづらい） | 高い（インライン展開しやすい） |
| スタックトレースの見え方 | 名前つき内部クラス | `Lambda$main$0` のような匿名名 |
| 柔軟性（JVM視点） | 低い | 高い（メソッド参照などもOK） |

# Q,

> orElseGet(...)
　→ 引数は Supplier（遅延実行）、必要になったときにだけ評価される
> 

関数インタフェースを使用する場合の遅延実行について確認です。
唯一のメソッドに処理を合成させてその後、変数へ代入。
当該メソッドが所属するクラスをインスタンス化した後に、
その変数からメソッドにアクセスしてその関数インタフェースの性質に応じて引数を渡すなり、そのまま実行するなりして使用する。

上記の流れは、通常のメソッドの使用と変わらない認識ですが、なぜ、遅延実行になるのでしょうか？

素晴らしい問いです。

Hiroakiさんの「関数インタフェースの合成→保持→呼び出し」という流れの理解は正しく、まさにそこに**「遅延実行」が成立する理由**があります。

---

## 🔍 遅延実行（遅延評価）とは？

- *「定義された処理が、すぐには実行されず、必要なときにだけ実行される」**ということです。

---

## ✅ 関数インタフェースを使った遅延実行の本質

以下の流れが重要です：

| ステップ | 内容 | 備考 |
| --- | --- | --- |
| ① 処理の定義 | ラムダ式やメソッド参照で処理を定義 | `Supplier<String> s = () -> compute();` |
| ② 実行準備 | 関数インタフェース型の変数に代入 | これは「まだ実行されていない」 |
| ③ 実行タイミング | `s.get()` を呼ぶ時に初めて処理される | このタイミングが「遅延」 |

---

### 🔸 通常メソッドとの違い

通常のメソッド呼び出し：

```java
String result = compute(); // ← 呼び出し時点で即実行される
```

Supplierを使った遅延実行：

```java
Supplier<String> s = () -> compute(); // ← ここでは実行されない
String result = s.get();              // ← ここで初めて compute() が呼ばれる
```

👉 **`()` を付けた瞬間が「実行タイミング」なので、それを遅らせているという点が違いです。**

---

## ✅ `orElseGet(...)` での遅延実行

```java
String result = Optional.ofNullable(null)
                        .orElseGet(() -> computeDefault());
```

このとき、`computeDefault()` は「Optionalが空だったときに初めて」呼ばれます。

- `orElse(...)` は常に `other` を評価する（= eager evaluation）
- `orElseGet(...)` は、**ラムダを渡しておいて、必要になったときに `get()` で実行**する

---

## 🧠 Hiroakiさんの疑問に答える視点

> 「唯一のメソッドに処理を合成しておく → 後で使う」
> 
> 
> → これって普通のメソッドと同じでは？
> 

✅ いいえ、違いは**「合成した時点で実行されない」**ということです。

```java
Supplier<String> s = () -> {
    System.out.println("called!");
    return "hello";
};
// ここではまだ出力されない（定義だけ）
```

```java
String result = s.get(); // ← このとき「初めて」実行される
```

---

## 🔚 まとめ

| 比較軸 | 通常メソッド | 関数インタフェース（Supplier等） |
| --- | --- | --- |
| 処理の合成 | すぐに実行される | 実行されない（定義のみ） |
| 実行タイミング | 代入時・呼び出し時 | `.get()` などメソッドを明示的に呼んだとき |
| 遅延性 | なし | あり（必要なときだけ） |
| 使用例 | `orElse(...)` | `orElseGet(...)`, `Stream`, `CompletableFuture` など |

---

より低レイヤーで見ると「関数の定義をオブジェクトとして持っておく」ことで**制御可能な評価タイミングを得る**というのがポイントです。

ご希望があれば、`Supplier` の代わりに `Function`, `Consumer` などとの比較や、JVM視点での「呼ばれるまでコード領域に存在するだけ」といった分析も可能です。