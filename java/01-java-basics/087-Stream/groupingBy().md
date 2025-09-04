# groupingBy()

# Q,

Collectors.groupingBy() について解説してください。

`Collectors.groupingBy()` は、Java Stream API において、**要素を指定した分類関数に基づいてグループ化する Collector** です。SQL の `GROUP BY` に相当する処理ができます。

---

## 🔹 基本構文

```java
Map<K, List<T>> result = stream.collect(Collectors.groupingBy(classifier));
```

- `T`: 元のストリームの要素型
- `K`: 分類キーの型（`classifier` の戻り値の型）
- `classifier`: `Function<? super T, ? extends K>`

---

## 🔸 例1：基本的なグループ化

```java
List<String> names = List.of("Alice", "Bob", "Charlie", "David", "Daniel");

Map<Character, List<String>> grouped =
    names.stream().collect(Collectors.groupingBy(name -> name.charAt(0)));

System.out.println(grouped);
```

📌 出力例：

```java
{A=[Alice], B=[Bob], C=[Charlie], D=[David, Daniel]}
```

---

## 🔸 例2：`downstream` Collector を組み合わせる

第二引数に別の Collector（例：`Collectors.counting()`）を渡すことで、**各グループに対する集計処理**も可能です。

```java
Map<Character, Long> countByInitial =
    names.stream().collect(Collectors.groupingBy(
        name -> name.charAt(0),
        Collectors.counting()
    ));

System.out.println(countByInitial);
```

📌 出力例：

```java
{A=1, B=1, C=1, D=2}
```

---

## 🔸 例3：`Map` の種類を指定（TreeMap など）

第三引数で `Map` のファクトリを渡すこともできます：

```java
Map<Character, List<String>> sortedGrouped =
    names.stream().collect(Collectors.groupingBy(
        name -> name.charAt(0),
        TreeMap::new,
        Collectors.toList()
    ));
```

---

## 🔸 オーバーロードまとめ

```java
// 1. 基本グループ化（List<T> でまとめられる）
groupingBy(Function<? super T, ? extends K> classifier)

// 2. 集約処理付き
groupingBy(Function<? super T, ? extends K> classifier,
           Collector<? super T, A, D> downstream)

// 3. Map の種類指定 + 集約処理
groupingBy(Function<? super T, ? extends K> classifier,
           Supplier<M> mapFactory,
           Collector<? super T, A, D> downstream)
```

---

## 🔍 よく使う downstream Collector

| Collector | 説明 |
| --- | --- |
| `toList()` | グループごとのリストに変換 |
| `counting()` | グループごとの件数 |
| `mapping()` | 値を変換してリスト化 |
| `summarizingInt()` など | グループごとの集計（平均、最大等） |
| `reducing()` | グループ内で reduce 処理 |

---

必要であれば、さらに応用的な例（ネスト・複数キー・カスタム集計など）も紹介できます！

# Q,

downstream Collector とは何ですか？

いい質問です。

`downstream Collector` とは、**`Collectors.groupingBy()` などの「複合的な Collector」の中で、各グループに対して適用される二次的な Collector** のことを指します。

---

## 🔹 イメージ図

```java
groupingBy(keyExtractor, downstreamCollector)
              │
              └── 各グループに対してさらに処理をする Collector
```

つまり：

- **groupingBy** でキーごとに要素を分類し、
- **downstreamCollector** で「分類された各グループに対する処理（集約や変換）」を行う

---

## 🔸 例1：基本（downstream を省略）

```java
Map<String, List<String>> result = 
    list.stream().collect(Collectors.groupingBy(s -> s.substring(0, 1)));
```

- 各グループの値は `List<String>` になる
- これは `Collectors.toList()` が **暗黙的な downstream** として適用されているから

---

## 🔸 例2：counting を downstream として指定

```java
Map<String, Long> countMap = 
    list.stream().collect(Collectors.groupingBy(
        s -> s.substring(0, 1),
        Collectors.counting()   // ← これが downstream Collector
    ));

```

- 各グループに対して `.count()` のような動作をする
- 値は `Long` 型になる（カウント数）

---

## 🔸 よく使う downstream Collector の例

| downstream Collector | 説明例 | 出力型 |
| --- | --- | --- |
| `toList()` | 要素をそのままリストにする | `List<T>` |
| `counting()` | 件数をカウント | `Long` |
| `mapping(f, toList())` | 値を変換してからリストにする | `List<R>` |
| `summarizingInt()` | 数値の要約統計情報（合計・平均・最大・最小など） | `IntSummaryStatistics` |
| `reducing()` | 値を reduce 演算でひとつにまとめる（累積） | 任意の型 |
| `collectingAndThen()` | 結果に後処理を加える | 任意の変換結果 |

---

## 🔸 例3：mapping + toSet の組み合わせ

```java
Map<String, Set<Character>> initialToChars =
    list.stream().collect(Collectors.groupingBy(
        s -> s.substring(0, 1),
        Collectors.mapping(s -> s.charAt(1), Collectors.toSet())
    ));
```

- 各グループの要素に `charAt(1)` を適用
- 結果を `Set` に格納

---

## 🔸 まとめ

- `downstream Collector` は、「グループごとの値の扱い方」を定義する
- `groupingBy()` の第2引数として使う
- 組み合わせることで、**SQL 的な集計や変換が柔軟に可能**

---

必要なら、「ネストした groupingBy（2段階グループ化）」や「カスタム Collector を downstream に使う例」も紹介できます。ご希望ありますか？