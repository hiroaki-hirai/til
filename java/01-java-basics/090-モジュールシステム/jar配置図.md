# jar配置図

# Q、

この構成をフォルダ構造＋JAR配置図で作成してください。

了解です。**クラスパス版**と**モジュール（JPMS）版**の2パターンで、フォルダ構成とJARの中身（`META-INF/services`）まで見える図にしました。

# クラスパス構成（非モジュール）

```java
my-app/                             ← 本体アプリ（利用側）
├─ src/
│  └─ main/
│     ├─ java/
│     │  └─ com/example/app/
│     │     └─ Main.java
│     └─ resources/
│
├─ build/
│  └─ classes/java/main/
│     └─ com/example/app/
│        └─ Main.class
│
├─ libs/                            ← ここにプロバイダJARを置く
│  ├─ provider-a-1.0.jar
│  └─ provider-b-1.0.jar
│
└─ my-app.jar                       ← （任意）本体をJAR化
```

- **本体側にあるのはサービス型（インタフェース/抽象クラス）**：

```java
my-app/src/main/java/com/example/spi/MyService.java
```

- **起動例**（クラスパス）：

```java
java -cp "my-app.jar;libs/*" com.example.app.Main
```

### プロバイダJARの中身（例：provider-a-1.0.jar）

```java
provider-a-1.0.jar
├─ com/example/provider/a/
│  └─ MyServiceImplA.class          ← MyService の実装
└─ META-INF/services/
   └─ com.example.spi.MyService     ← テキストファイル（中身は実装クラス名）
```

`META-INF/services/com.example.spi.MyService` の内容：

```java
com.example.provider.a.MyServiceImplA
```

（`provider-b-1.0.jar` も同様に `MyServiceImplB` を記載）

---

# モジュール構成（JPMS）

```java
my-app/
├─ consumer/                        ← 利用側モジュール
│  ├─ src/
│  │  ├─ module-info.java           ← module consumer { requires provider.a; uses com.example.spi.MyService; }
│  │  └─ com/example/app/Main.java
│  └─ out/consumer/                 ← クラス出力（またはJAR化）
│
├─ api/                             ← サービス型モジュール（SPIインタフェース）
│  ├─ src/
│  │  ├─ module-info.java           ← module api { exports com.example.spi; }
│  │  └─ com/example/spi/MyService.java
│  └─ out/api/
│
├─ provider-a/                      ← プロバイダAモジュール
│  ├─ src/
│  │  ├─ module-info.java           ← module provider.a { requires api; provides com.example.spi.MyService with com.example.provider.a.MyServiceImplA; }
│  │  └─ com/example/provider/a/MyServiceImplA.java
│  └─ out/provider.a/
│
└─ provider-b/                      ← プロバイダBモジュール
   ├─ src/
   │  ├─ module-info.java           ← module provider.b { requires api; provides com.example.spi.MyService with com.example.provider.b.MyServiceImplB; }
   │  └─ com/example/provider/b/MyServiceImplB.java
   └─ out/provider.b/
```

- **起動例**（モジュールパス）：

```java
java --module-path "out/api;out/provider.a;out/provider.b;out/consumer" \
     --module consumer/com.example.app.Main
```

- JPMS では `META-INF/services` ではなく **`module-info.java` の `provides ... with ...`** で提供を宣言。
    
    利用側は **`uses com.example.spi.MyService`** を宣言。
    

---

# 参照関係の要点（どちらの方式でも共通）

- `ServiceLoader.load(MyService.class)` の **引数は“サービス型の `Class`”**（実装ではない）
- 実装クラスは
    - クラスパス版：**`META-INF/services/<FQCN>`** に列挙
    - モジュール版：**`module-info.java` の `provides`** で宣言
- いずれも **実装JARをクラスパス/モジュールパスに置けばよく**、本体のディレクトリに実装`.class`を移す必要はありません

必要なら、この構成に合わせた **最小コードひな型**（`Main`/`MyService`/`Impl`/`module-info.java`）もお渡しします。