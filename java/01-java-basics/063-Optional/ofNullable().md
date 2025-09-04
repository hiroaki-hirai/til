# ofNullable()

# Q,

```java
    @SuppressWarnings("unchecked")
    public static <T> Optional<T> ofNullable(T value) {
        return value == null ? (Optional<T>) EMPTY
                             : new Optional<>(value);
    }
```

上記は、se21のofNullableメソッドの宣言ですが、  @SuppressWarnings("unchecked") はどういったアノテーションですか？

`@SuppressWarnings("unchecked")` は **Java コンパイラに対して「このコードで unchecked（未検査）キャストの警告を抑制してほしい」と指示するアノテーション** です。

---

### 🔍 用語の整理

### ✅ `unchecked` 警告とは？

Javaの**ジェネリクスは型消去（type erasure）**により、実行時には型情報が失われるため、以下のような **安全が保証できないキャスト** に対して警告を出します。

```java
List<String> list = (List<String>) new ArrayList(); // ← ここで unchecked 警告
```

型が失われた `ArrayList` を `List<String>` にキャストしても、**実行時には本当に String 型かどうか分からない**ためです。

---

### 🔧 今回のコードとの関係

```java
@SuppressWarnings("unchecked")
public static <T> Optional<T> ofNullable(T value) {
    return value == null ? (Optional<T>) EMPTY
                         : new Optional<>(value);
}
```

- `EMPTY` は `Optional<?>` 型（ワイルドカード）で宣言されています。
- それを `Optional<T>` にキャストして返しているため、**unchecked cast** になります：

```java
(Optional<T>) EMPTY  // ← Optional<?> を Optional<T> にキャストしている
```

→ **このときコンパイラは「Tの型が何か分からないから危ないよ」と警告します**。

---

### 🎯 なぜ suppress してもいいのか？

`Optional.empty()` で返されるのは「値を持たない」Optionalであり、**型の安全性に関係しない（中身がない）**ので、開発者側で「大丈夫」と判断して明示的に suppress しています。

---

### ✅ 補足まとめ

| 項目 | 内容 |
| --- | --- |
| アノテーション名 | `@SuppressWarnings("unchecked")` |
| 対象 | ジェネリクスにおける「安全が保証されないキャスト」 |
| この例での用途 | `Optional<?>` → `Optional<T>` へのキャスト |
| なぜOKか？ | `EMPTY` は値を持たないので、キャストの実害がないため |

---

必要であれば、`EMPTY` 定数やこのメソッドの使われ方の全体設計も整理できます。