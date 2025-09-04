# Collectors

# Q,

Collectorインターフェースのメソッドは必ずサブクラスで実装する必要がありますが、CollectorsクラスのCollectorファクトリーメソッドを使用すると、Collectorインターフェース内の５つのメソッドが自動で実装されるのでしょうか？
例えば、toList() を利用すると、内部でsupplier() などの５つのメソッドが自動で実装されるイメージです。

はい、**その理解は正しいです。**

---

## ✅ 要点から先に：

> Collectors.toList() のようなファクトリーメソッドは、内部的に Collector インターフェースの5つのメソッド（supplier() など）を全て実装した匿名クラスやラムダで構成されたオブジェクトを返しています。
> 
> 
> つまり、**開発者が自分で明示的に5メソッドを実装する必要がないように隠蔽されている**ということです。
> 

---

## 🔹 `Collector` インターフェースの5つのメソッドおさらい

| メソッド | 内容 |
| --- | --- |
| `supplier()` | 中間結果の初期化（コンテナの生成） |
| `accumulator()` | 各要素をコンテナに追加する処理 |
| `combiner()` | 並列処理時にコンテナ同士を結合する処理 |
| `finisher()` | 中間結果を最終結果に変換する処理 |
| `characteristics()` | ストリーム処理の特性（UNORDERED、IDENTITY_FINISH など） |

---

## 🔸 例：`Collectors.toList()` の中身（イメージ）

以下のような内容を**内部で自動的に定義・組み合わせています**：

```java
public static <T> Collector<T, ?, List<T>> toList() {
    return new Collector<T, List<T>, List<T>>() {
        public Supplier<List<T>> supplier() {
            return ArrayList::new;
        }
        public BiConsumer<List<T>, T> accumulator() {
            return List::add;
        }
        public BinaryOperator<List<T>> combiner() {
            return (left, right) -> { left.addAll(right); return left; };
        }
        public Function<List<T>, List<T>> finisher() {
            return Function.identity();  // 変換不要
        }
        public Set<Characteristics> characteristics() {
            return Collections.singleton(Characteristics.IDENTITY_FINISH);
        }
    };
}
```

🔹 これが `Collectors.toList()` の実体です（JDKの中で最適化されています）。

ユーザーはこの中身を知らなくても使えるように設計されています。

---

## ✅ 結論

| 質問内容 | 回答 |
| --- | --- |
| `Collectors.toList()` を使うと、`Collector` のメソッドは自動で実装される？ | ✅ **はい**。内部で全てのメソッドが定義済みの `Collector` 実装が返されます。 |
| 自分で5つのメソッドを実装する必要がある？ | ❌ **ない**。必要な処理は `Collectors` が全て肩代わりしています。 |

---

ご希望があれば、`groupingBy()` や `partitioningBy()` などの、より複雑な Collector の中身も解説できます。