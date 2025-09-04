# Class オブジェクトを利用したインスタンス化

# Q,

通常のインスタンス化の場合は、コンパイル時に確定、Class オブジェクトを利用したインスタンス化（リフレクション）の場合は、実行時まで確定しない。
上記の違いは、メモリでの処理の違いが影響していますか？

はい、かなり大きく**メモリでの処理の違い**が影響しています。

この「通常のインスタンス化」と「Classオブジェクトを利用したリフレクションでのインスタンス化」の違いは、

**コンパイル時の情報確定度**と**実行時のクラスメタデータ参照方法**に直結します。

---

## 1. 通常のインスタンス化

```java
MyService service = new MyServiceImpl();
```

### 特徴

- **コンパイル時**に`MyServiceImpl`の型情報が完全に確定
- コンパイラは「`new`命令（`new`バイトコード命令）」を直接生成
- 実行時はクラスローダにより`MyServiceImpl`のクラス定義をロード
    
    → ヒープにインスタンス作成
    
    → スタックに参照を格納
    
- JVMは**直接バイトコードからMetaspaceのクラスメタデータにアクセス**できる

**メモリ面の流れ（簡易図）**

```java
[コンパイル時] ソースコード
   ↓ 型確定
[バイトコード] → new命令 (直接)
   ↓
[実行時] Metaspace: クラス定義読み込み
   ↓
Heap: インスタンス生成
```

---

## 2. Classオブジェクトを利用したインスタンス化（リフレクション）

```java
Class<?> clazz = Class.forName("com.example.MyServiceImpl");
MyService service = (MyService) clazz.getDeclaredConstructor().newInstance();
```

### 特徴

- **コンパイル時**には、`MyServiceImpl`の型は**文字列としてしか存在しない**
- 実行時に`Class.forName()`でクラスローダに名前を渡し、クラスをロード
- `Class`オブジェクト（ヒープ上）経由でMetaspaceのクラス定義を参照し、`Constructor`オブジェクトを取得
- その`Constructor`を通じてインスタンス生成（内部的には`new`命令に相当する処理）

**メモリ面の流れ（簡易図）**

```java
[コンパイル時] ソースコード
   ↓ 型は未確定（Stringで保持）
[バイトコード] → Class.forName()呼び出し
   ↓
[実行時] Metaspace: クラス定義をロード
   ↓
Heap: Classオブジェクト生成
   ↓
Heap: Constructorオブジェクト取得
   ↓
Heap: newInstance()でインスタンス生成
```

---

## 3. 違いが生まれる理由（メモリ処理面）

| 項目 | 通常のインスタンス化 | リフレクションによるインスタンス化 |
| --- | --- | --- |
| 型の確定 | コンパイル時 | 実行時 |
| バイトコード命令 | `new`命令直接埋め込み | `Class.forName()` → メタ情報経由で`new`相当 |
| Metaspaceアクセス | 直接アクセス | `Class`オブジェクト経由 |
| ヒープ処理 | インスタンスを即生成 | Classオブジェクト＆Constructor取得後に生成 |
| オーバーヘッド | 少ない（直接命令） | 多い（リフレクション処理のため） |

---

## 4. ポイント

- **通常のインスタンス化**は、コンパイル時にクラスが分かっているため、JVMは最短経路でMetaspaceから必要情報を引き出せる
- **リフレクション**は、実行時にクラス名を解決 → `Class`オブジェクト経由でインスタンス化、という間接的手順が入る
- この差が**速度差**や**柔軟性（動的ロード）**の違いにつながる

---

もしよければ、これを**「通常インスタンス化 vs リフレクション」のメモリ構造比較図**として作って、

バイトコード・Metaspace・ヒープのアクセス経路を矢印で示すこともできます。

そうすると、なぜ速度と設計方針が変わるかが一目でわかります。

# Q,

```java
MyService service = MyService.class
        .getDeclaredConstructor()
        .newInstance();
```

上記コードの場合も、上の解説のようなメモリ上での流れになりますか？

結論：はい。**クラスリテラル経由のリフレクション生成**として、前に説明した「クラスロード → メタ情報参照 → ヒープに生成」の流れになります（`forName()`版との違いは“初期化のタイミング”程度）。

---

# メモリ上の流れ（`MyService.class.getDeclaredConstructor().newInstance()`）

1. **`MyService.class` の評価（クラスリテラル）**
- バイトコードの `ldc` で **`Class<MyService>` 参照**を取得（ヒープ）。
- 必要ならロード/リンクはされますが、**初期化は通常まだ**（JLS 12.4.1）。
- `Class` オブジェクトはヒープ、クラス定義は **Metaspace**。
1. **`getDeclaredConstructor()`**
- `Class` から Metaspace のメソッド/コンストラクタ表を参照し、対象 **`Constructor<?>` オブジェクト（ヒープ）**を返す。
- ここまでで**実体（インスタンス）は未生成**。
1. **`newInstance()`**
- 必要ならここで **クラス初期化**（`static {}` 等）を実施。
- アクセス検査 → **ヒープにインスタンス領域を確保** → `<init>` 実行（スタックにフレーム作成） → **参照を返す**。
- 参照は呼び出し元の**スタック**に格納される。

> つまり：ヒープ（Class/Constructor/新インスタンス）＋ Metaspace（クラス定義）＋ スタック（呼び出しフレーム/参照）が関与します。
> 

---

# `Class.forName()` との違い（要点だけ）

- `MyService.class` は **初期化を要求しない**（通常）。
- `Class.forName(name)` は **既定で初期化まで行う**。
- 生成そのもの（`getDeclaredConstructor().newInstance()`）の段はどちらも同じく、**Metaspace参照 → ヒープ確保 → `<init>` 実行**です。

---

# 注意点（重要）

- 例のコードの **`MyService` がインタフェースや抽象クラスなら失敗**します（コンストラクタが無い/抽象）。
    - 実際に生成したいのは **具象クラス**：`MyServiceImpl.class.getDeclaredConstructor().newInstance()`
    - もしくは `Class<? extends MyService> impl = ...; impl.getDeclaredConstructor().newInstance();`

必要なら、この流れを **もう少し細かいシーケンス図**（`<clinit>`/`<init>`の発火位置や例外分岐付き）でも描けます。