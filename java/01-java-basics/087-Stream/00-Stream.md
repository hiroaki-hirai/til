# Stream

# Q,

Streamに関連するクラスやインターフェースとその構成（継承・実装関係）をまとめてください。

了解しました、Hiroakiさん。

ここでは **Javaの `Stream` API に関連する主要なクラス・インタフェースとその構成（継承・実装関係）** を図解的に整理してご紹介します。

---

## ✅ 1. Stream API の中心インタフェース

```java
           BaseStream<T, Stream<T>>
                  ▲
                  │
               Stream<T>
```

- `BaseStream<T, S extends BaseStream<T, S>>` が基本インタフェース（汎用）
- `Stream<T>` はそのサブインタフェースで、通常使うストリーム

---

## ✅ 2. 基本構成図（継承・実装関係）

```java
                      ┌──────────────────────────────┐
                      │        java.util.stream       │
                      └──────────────────────────────┘
                                 ▲
            ┌────────────────────┼────────────────────┐
            │                    │                    │
      BaseStream<T, S>      BaseStream<Integer, S>  BaseStream<Long, S> ...
            ▲                    ▲                    ▲
            │                    │                    │
       Stream<T>           IntStream             LongStream ...（primitive専用）
            ▲
            │ implements
            ▼
      classes like:
      ┌────────────────────────────────────────┐
      │ ReferencePipeline<T> extends AbstractPipeline<T> │
      └────────────────────────────────────────┘
```

---

## ✅ 3. よく使う関連クラス・インタフェースまとめ

| 名前 | 型 | 説明 |
| --- | --- | --- |
| `Stream<T>` | インタフェース | 一般的な参照型ストリーム |
| `IntStream`, `LongStream`, `DoubleStream` | インタフェース | プリミティブ型専用ストリーム |
| `BaseStream<T, S>` | インタフェース | `Stream`, `IntStream` などの共通基底インタフェース |
| `StreamSupport` | クラス | ストリームのユーティリティ、Spliterator から Stream を作成 |
| `Collectors` | クラス | `collect()` 用の収集ロジック（例：toList(), joining()） |
| `Collector<T,A,R>` | インタフェース | `collect()` の内部処理仕様 |
| `Spliterator<T>` | インタフェース | 分割可能なイテレーター、Stream生成時に利用 |
| `Stream.Builder<T>` | インタフェース | 手動で要素を追加して Stream を構築する |

---

## ✅ 4. 実装クラスの例（通常意識しないが内部で使われる）

| 実装クラス名 | 説明 |
| --- | --- |
| `ReferencePipeline` | 通常の `Stream<T>` の実装クラス |
| `AbstractPipeline` | 全ストリームの共通基底実装（状態管理など） |
| `Streams.StreamBuilderImpl` | `Stream.builder()` の内部実装 |
| `StreamSpliterators.*` | Spliterator 経由のストリーム変換実装 |

---

## ✅ 5. 使用イメージ（例）

```java
Stream<String> stream = Stream.of("A", "B", "C");
stream.map(String::toLowerCase)
      .filter(s -> s.startsWith("a"))
      .forEach(System.out::println);
```

この例では：

- `Stream<T>` が使われている
- `map`, `filter`, `forEach` は `Stream<T>` に定義されている中間・終端操作
- 実際は `ReferencePipeline.map()` などの内部実装が呼ばれる

---

## ✅ 補足：BaseStreamのシグネチャ

```java
interface BaseStream<T, S extends BaseStream<T, S>> extends AutoCloseable {
    Iterator<T> iterator();
    Spliterator<T> spliterator();
    boolean isParallel();
    S sequential();
    S parallel();
    void close();
    ...
}
```

→ 型パラメータ `S` を使って**戻り値の型を自分自身にする**ことで、メソッドチェーンを自然にしています（"self-referential generic type"）。

---

## ✅ まとめ図

```java
BaseStream<T, S>
    ▲
 ┌─┴────────────┐
 Stream<T>   IntStream / LongStream / DoubleStream
    ▲
    │ implements
    ▼
 ReferencePipeline<T> → AbstractPipeline<T> → PipelineHelper
```

---

ご希望があれば、`Stream` の内部動作（遅延評価、パイプライン構築、終端操作トリガー）についても解説できます！

[of()](Stream%2021d62cef7350800f98b7f77df8b2715c/of()%2021d62cef73508098ac7ce675388122f6.md)

[map()](Stream%2021d62cef7350800f98b7f77df8b2715c/map()%2023d62cef7350802db7ced7d52412e339.md)

[peek()](Stream%2021d62cef7350800f98b7f77df8b2715c/peek()%2023e62cef73508040aabfdb5c36baac46.md)

[findAny()](Stream%2021d62cef7350800f98b7f77df8b2715c/findAny()%2023d62cef735080e1875ce3fa3cedeaab.md)

[reduce()](Stream%2021d62cef7350800f98b7f77df8b2715c/reduce()%2023d62cef7350806783c6fb2d9a71afab.md)

[max(), min()](Stream%2021d62cef7350800f98b7f77df8b2715c/max(),%20min()%2023e62cef735080ad8ee4ec5307719452.md)

[Collector](Stream%2021d62cef7350800f98b7f77df8b2715c/Collector%2023e62cef7350807f803fe8504f3fb960.md)

[Collector.of()](Stream%2021d62cef7350800f98b7f77df8b2715c/Collector%20of()%2023e62cef735080468819cbbb81f8d5af.md)

[Collectors](Stream%2021d62cef7350800f98b7f77df8b2715c/Collectors%2023e62cef735080c8983ff627f8bfa81d.md)

[groupingBy()](Stream%2021d62cef7350800f98b7f77df8b2715c/groupingBy()%2023e62cef735080b89328db1c62a4bc93.md)

[partitionBy()](Stream%2021d62cef7350800f98b7f77df8b2715c/partitionBy()%2023e62cef73508054ab74eb1b5ae6ddd8.md)

[downstream Collector](Stream%2021d62cef7350800f98b7f77df8b2715c/downstream%20Collector%2023e62cef73508035a6e6e415d41b5ac9.md)

[summingInt()](Stream%2021d62cef7350800f98b7f77df8b2715c/summingInt()%2023e62cef735080ea8b1afe62b987a012.md)