# Error

# Q,

Errorクラスにはどのようなものが挙げられますか？

はい、Hiroakiさん。

`Error` クラスは Java の例外階層において特別な位置づけを持つ存在で、**アプリケーションが回復不能な「重大なエラー」** を表します。

---

# ✅ `Error` クラスとは？

- `java.lang.Throwable` のサブクラスで、`Exception` の兄弟クラスです。
- 基本的に **開発者が catch したり処理しようとするべきではない**。
- 多くは **JVM やランタイム環境の問題**に起因します。

```java
Throwable
├── Exception           ← チェック例外（回復可能）
│   └── RuntimeException← 非チェック例外（開発者ミス）
└── Error               ← 非チェック例外（**回復不能**）
```

---

# 🔥 主な `Error` クラスと内容

| クラス名 | 内容 |
| --- | --- |
| `OutOfMemoryError` | メモリ不足。JVMがヒープ/Perm領域の確保に失敗 |
| `StackOverflowError` | スタックオーバーフロー（再帰無限など） |
| `VirtualMachineError` | JVMそのものの異常の基底クラス |
| ├─ `InternalError` | JVMの内部実装で予期しない問題が発生 |
| └─ `UnknownError` | 詳細不明なJVMのエラー |
| `NoClassDefFoundError` | 実行時に必要なクラスが見つからない |
| `UnsatisfiedLinkError` | JNIで呼び出すネイティブライブラリが解決できない |
| `ClassFormatError` | クラスファイルの形式が無効 |
| `AssertionError` | `assert` 文が失敗した場合 |
| `ExceptionInInitializerError` | static初期化子で例外が発生した場合 |
| `LinkageError`（系） | クラスの再リンクに失敗 |
| ├─ `ClassCircularityError` | クラスの循環参照検出 |
| └─ `IncompatibleClassChangeError` | バージョン不整合などの変更エラー |

---

# ❌ `Error` を catch すべきか？

原則として **catch すべきではありません**。

例外と異なり、`Error` は：

- **アプリケーションレベルで回復できない（するべきでない）**
- **問題の根本はJVMや環境、設定ミス、設計ミス**

たとえば：

```java
try {
    // 無限再帰で StackOverflowError を発生
    recurse();
} catch (StackOverflowError e) {
    // やるべきではない。ログに残すくらいにとどめる。
    System.err.println("回復不能なエラーが発生しました");
}
```

---

# 🧠 `Error` の実用的なポイント

| 観点 | 内容 |
| --- | --- |
| 回復不能 | アプリケーション内で解決不能な問題が中心 |
| catchすべきか？ | 原則 NO（ログなどに限定） |
| 開発時の注意 | 無限再帰・メモリリーク・初期化処理の例外発生などに注意 |
| モニタリング対象 | APMや監視システムで JVM の `Error` をキャッチするのは有効 |

---

# ✅ まとめ：代表的な `Error` クラス

| 種別 | クラス名 | 発生原因 |
| --- | --- | --- |
| メモリ・スタック | `OutOfMemoryError`, `StackOverflowError` | メモリ不足、無限再帰 |
| JVM・実行環境 | `VirtualMachineError`, `InternalError` | JVM内部異常 |
| クラスロード | `NoClassDefFoundError`, `ClassFormatError` | クラスファイルや依存の問題 |
| ネイティブコード | `UnsatisfiedLinkError` | JNIのネイティブライブラリ未解決 |
| リンク・互換性 | `LinkageError`系 | バージョン・依存関係の不整合 |
| アサーション | `AssertionError` | assert によるチェック失敗 |
| static初期化失敗 | `ExceptionInInitializerError` | staticブロックでの例外 |

---

必要であれば、各 `Error` の発生条件や具体的な再現コード、またはモニタリング方法についてもお伝えできます。興味がある分野があればお気軽にお知らせください。