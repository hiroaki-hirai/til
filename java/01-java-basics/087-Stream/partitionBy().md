# partitionBy()

# Q,

partitionBy()について解説してください。

`Collectors.partitioningBy()` は、Java Stream API における `Collector` のひとつで、

**ストリーム内の要素を「条件（true/false）」に基づいて2つのグループに分ける**ためのものです。

SQL で言えば、**WHERE 条件による2分割のような処理**です。

---

## 🔹 基本構文

```java
Collectors.partitioningBy(Predicate<? super T> predicate)
```

または

```java
Collectors.partitioningBy(Predicate<? super T> predicate, 
                          Collector<? super T, A, D> downstream)
```

- `predicate`: true / false を返す条件関数
- `downstream`: 各グループに対する集計処理（省略時は `toList()`）

---

## 🔸 例1：基本的な使用（true / false にグループ分け）

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

Map<Boolean, List<Integer>> partitioned =
    numbers.stream().collect(Collectors.partitioningBy(n -> n % 2 == 0));

System.out.println(partitioned);
```

📌 出力：

```java
{
  false=[1, 3, 5],
  true=[2, 4, 6]
}
```

- `true` キー：条件を満たす（偶数）
- `false` キー：条件を満たさない（奇数）

---

## 🔸 例2：downstream Collector を追加する

各グループの合計を求めたい場合：

```java
Map<Boolean, Integer> sumPartitioned =
    numbers.stream().collect(Collectors.partitioningBy(
        n -> n % 2 == 0,
        Collectors.summingInt(n -> n)
    ));

System.out.println(sumPartitioned); // {false=9, true=12}
```

---

## ✅ `partitioningBy()` と `groupingBy()` の違い

| 特徴 | `partitioningBy()` | `groupingBy()` |
| --- | --- | --- |
| 分類の基準 | `Predicate`（真偽値） | 任意の関数（分類キー） |
| 出力される Map のキー | `Boolean`（true/false） | 任意の型（`K`） |
| グループ数の上限 | 常に最大2つ（true / false） | 任意個（ユニークなキー数） |
| 実行時の最適化 | 一部 Collector で高速化される | 比較的柔軟（だが一般的） |

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 目的 | 条件により2分割（true / false） |
| 戻り値の型 | `Map<Boolean, List<T>>`（または他の型） |
| 利点 | 明確な二分法に強く、読みやすい |
| 応用 | downstream により集計処理も可能 |

---

必要であれば、`groupingBy()` との **パフォーマンス差や内部実装の違い**、または **ネストした partitioning の実装例** もお見せできます！