# forEachメソッド

[Iterable](forEach%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%2020b62cef735080228183f7ccfea78db6/Iterable%2020b62cef7350804da95acf1a46d6c782.md)

[Map](forEach%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%2020b62cef735080228183f7ccfea78db6/Map%2020b62cef7350807580a2ec0d02e7122a.md)

[Stream](forEach%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%2020b62cef735080228183f7ccfea78db6/Stream%2020b62cef735080719d7cdf16b26277a3.md)

# Q,

forEach については、Iterable、Map、Streamの3種類がある認識であっていますか？

はい、Hiroakiさんの認識は**正確で的確**です。

`forEach` には主に以下の **3種類の実装・意味合い**があります：

---

## ✅ `forEach` の3種類の分類

| 種類 | 所属 | 引数 | 特徴 |
| --- | --- | --- | --- |
| ① `Iterable` の `forEach` | `List`, `Set`, `Queue` など | `Consumer<T>` | 単純な順次処理・ループ代替。最も基本的な `forEach`。 |
| ② `Map` の `forEach` | `Map<K, V>` | `BiConsumer<K, V>` | キーと値のペアに対する処理。`Map`は`Iterable`非継承のため独自定義。 |
| ③ `Stream` の `forEach` | `Stream<T>` | `Consumer<T>` | 終端操作。**遅延評価されたストリームを即時実行**する。並列処理対応も可。 |

---

## 🔍 各 `forEach` の構造と用途

### ① `Iterable#forEach(Consumer<T>)`

- Java 8 で追加された **デフォルトメソッド**
- 実体は `for-each` 文の syntactic sugar（糖衣構文）

```java
List<String> list = List.of("A", "B", "C");
list.forEach(item -> System.out.println(item));
```

- 主な用途：**順序付きループ処理（早く書きたいだけ）**
- 中断不可、並列処理なし、簡潔

---

### ② `Map#forEach(BiConsumer<K, V>)`

- `Map` は `Iterable` ではないので、自身で `forEach` を定義

```java
Map<String, Integer> map = Map.of("A", 1, "B", 2);
map.forEach((k, v) -> System.out.println(k + " = " + v));
```

- 主な用途：**キーと値の同時処理**
- 内部で `entrySet()` に対してイテレートしている

---

### ③ `Stream#forEach(Consumer<T>)`

- **終端操作（terminal operation）**
- 中間操作（`filter`, `map`, etc.）の後に評価を実行する

```java
list.stream()
    .filter(s -> s.startsWith("A"))
    .forEach(System.out::println);
```

- 主な用途：**フィルタ・変換後の出力**
- `parallelStream()` を使えば並列処理にも対応
- 中断不可だが、**副作用を伴う操作でよく使われる**

---

## 📌 注意：それぞれは**明確に使い分けるべき**

| 使用場面 | 推奨される `forEach` |
| --- | --- |
| シンプルなループ | `Iterable#forEach`（構文短縮） |
| キーと値の操作 | `Map#forEach`（明快で高速） |
| ストリーム処理の終端 | `Stream#forEach`（パイプライン処理後） |

---

## ⚠️ `Stream#forEach` の注意点

- 並列ストリームで順序が保証されない場合がある
- **副作用（外部状態の変更）を避けるべき**とされる（関数型の原則）
- 順序保証が必要な場合は `forEachOrdered()` を使う

```java
list.parallelStream()
    .forEachOrdered(System.out::println);
```

---

## 🔚 結論：3つの `forEach` の違いを意識する

| 名前 | 適用先 | 処理モデル | 並列処理 | キーと値 | 中断 |
| --- | --- | --- | --- | --- | --- |
| `Iterable#forEach` | コレクション | 命令型 | ❌ | ❌ | ❌ |
| `Map#forEach` | Map | 命令型 | ❌ | ✅ | ❌ |
| `Stream#forEach` | Stream | 関数型（終端） | ✅（`parallelStream`） | ❌ | ❌ |