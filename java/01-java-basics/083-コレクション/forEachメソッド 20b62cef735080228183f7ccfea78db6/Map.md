# Map

Q,

Iterable を継承していない Map に forEach が使用できるのはなぜですか？
Iterable とは別で Map 独自の forEach が規定されているということでしょうか？

はい、Hiroakiさんのご認識は**完全に正確**です。

---

## ✅ 結論

`Map` は `Iterable` を継承していないため、`Iterable` に定義された `default void forEach(Consumer<? super T>)` は**使えません**。

その代わりに、**`Map` 独自で `forEach(BiConsumer<? super K, ? super V>)` を定義**しています。

---

## 🔍 詳しく解説

### 1. `Map` は `Collection` も `Iterable` も継承していない

```java
public interface Map<K,V> {
    // MapはCollectionやIterableを継承していない
}
```

そのため、以下のようなコードは**コンパイルエラー**になります

```java
Map<String, Integer> map = Map.of("A", 1, "B", 2);
// map.forEach(x -> System.out.println(x)); // ← これはNG（Iterableではないため）
```

---

### 2. 代わりに `Map` 独自の `forEach()` を定義（Java 8〜）

```java
default void forEach(BiConsumer<? super K, ? super V> action);
```

これは `Map` インターフェース自身に `default` メソッドとして定義されています。

📘 ソース：`java.util.Map`

```java
default void forEach(BiConsumer<? super K, ? super V> action) {
    Objects.requireNonNull(action);
    for (Map.Entry<K, V> entry : entrySet()) {
        action.accept(entry.getKey(), entry.getValue());
    }
}
```

つまり、内部的には `entrySet()` を使って `Map.Entry<K, V>` に変換し、それに対してループしています。

---

### 3. 使用例（`BiConsumer` を使う）

```java
Map<String, Integer> map = Map.of("A", 1, "B", 2);

map.forEach((key, value) -> System.out.println(key + " → " + value));
```

- `key`：キー（`K`）
- `value`：値（`V`）

→ `BiConsumer<K, V>` の `accept(K k, V v)` に渡されます。

---

## 🔁 補足：どうして `Iterable` を継承しないの？

理由は以下の通りです：

| 理由 | 説明 |
| --- | --- |
| `Iterable<T>` は「値の集合」前提 | `Map` は「キーと値のペア」なので、`T` 単体では表現が難しい |
| `Map` の要素は `Entry<K,V>` | 単一の型ではないため、`Iterable<Entry<K,V>>` を強制できない |
| インターフェースの意味を明確に分離したいため | `Map` は集合とは構造が違う、設計思想上の理由も |

---

## 🧩 まとめ

| 観点 | 内容 |
| --- | --- |
| `Map` は `Iterable` を継承しているか？ | ❌ していない |
| `Map` に `forEach` を使える理由は？ | ✅ `Map` 独自で `forEach(BiConsumer)` を定義しているから |
| 処理対象 | `Map.Entry<K, V>` を `entrySet()` で取得し処理 |