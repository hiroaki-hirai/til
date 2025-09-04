# java.util.ServiceLoader

# Q,

java.util.ServiceLoaderクラスについて解説してください。

はい、`java.util.ServiceLoader` は **Java 標準のサービスプロバイダ発見＆ロード機構** で、

いわゆる **SPI（Service Provider Interface）** を実現するためのクラスです。

簡単にいうと、**特定のインタフェースや抽象クラスの実装クラスを、外部から差し替えて動的にロードできる仕組み** を提供します。

---

## 1. 位置づけ

- パッケージ: `java.util`
- 目的: 実行時に **サービス実装（Provider）** を見つけてインスタンス化
- 主な用途:
    - モジュールやライブラリが利用する**拡張ポイント**の提供
    - JDBC ドライバ自動ロード、`java.nio.file.FileSystem` 実装のロードなど

---

## 2. 基本の用語

| 用語 | 意味 |
| --- | --- |
| **サービス (Service)** | 利用者側が使いたい機能を定義するインタフェースや抽象クラス |
| **サービスプロバイダ (Provider)** | サービスを実装した具象クラス |
| **サービス記述ファイル** | `META-INF/services/` に置くテキストファイル。ロード対象の完全修飾クラス名を書くだけ |
| **サービスローダ (ServiceLoader)** | サービス記述ファイルを読み、実装クラスをインスタンス化して返すクラス |

---

## 3. 使い方の流れ

### 3-1. サービス定義（インタフェース）

```java
// MyService.java
public interface MyService {
    void execute();
}
```

### 3-2. 実装クラス（プロバイダ）

```java
// MyServiceImpl.java
public class MyServiceImpl implements MyService {
    @Override
    public void execute() {
        System.out.println("Hello from MyServiceImpl!");
    }
}
```

### 3-3. サービス記述ファイル作成

- ファイルパス: `src/main/resources/META-INF/services/完全修飾インタフェース名`

```java
com.example.MyService
```

- 中身（テキストファイル）: 実装クラスの完全修飾名

```java
com.example.MyServiceImpl
```

### 3-4. 利用側コード

```java
import java.util.ServiceLoader;

public class Main {
    public static void main(String[] args) {
        ServiceLoader<MyService> loader = ServiceLoader.load(MyService.class);

        for (MyService service : loader) {
            service.execute();
        }
    }
}
```

- `ServiceLoader.load()` が `META-INF/services/...` を探し、指定された実装クラスを反射でインスタンス化します。

---

## 4. 主なメソッド

| メソッド | 説明 |
| --- | --- |
| `static <S> ServiceLoader<S> load(Class<S> service)` | デフォルトのクラスローダでサービスロード |
| `static <S> ServiceLoader<S> load(Class<S> service, ClassLoader cl)` | 指定クラスローダでロード |
| `Iterator<S> iterator()` | 遅延ロードされるサービス実装のイテレータ |
| `Stream<S> stream()` | Java 9 以降、ストリームAPIでサービス実装を処理 |
| `reload()` | 新しい実装を再探索（既存キャッシュを破棄） |

---

## 5. 特徴

- **遅延ロード**：`iterator()` で回すまで実装クラスはインスタンス化されません
- **複数実装対応**：記述ファイルに複数行書くと複数の実装が読み込まれます
- **依存を減らす**：利用側は実装クラスを直接知らず、インタフェースだけ参照すればよい
- **Java Module System 対応**（Java 9〜）：`module-info.java` に `uses` / `provides` 句を記述して同様の仕組みを実現可能

---

## 6. 実例（JDBC）

JDBC では、`DriverManager` が `ServiceLoader` を使って `java.sql.Driver` 実装（例：`com.mysql.cj.jdbc.Driver`）を自動ロードします。

→ 開発者は `Class.forName()` でドライバ登録する必要がなくなります（Java 6 以降）。

---

## 7. メリット・デメリット

**メリット**

- 実装の差し替えが容易（コンパイル不要）
- 実行時に動的に発見できる
- モジュール構造やプラグイン機構を簡単に作れる

**デメリット**

- 仕組みを知らないとブラックボックス化しやすい
- 実装クラス名は文字列（記述ファイル）で管理するためタイプミスに弱い
- クラスロードの失敗が実行時までわからない

---

もしご希望なら、このあと

**「module-info.java を使った ServiceLoader のモジュール版サンプル」** と

**「JDBC の内部での ServiceLoader 利用構造図」**

も作成できます。

こちらを続けますか？

[インスタンス化](java%20util%20ServiceLoader%2024b62cef73508034b945ee6c5f42439c/%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E5%8C%96%2024c62cef73508074a4d7c3f9297b7ea1.md)