# downstream Collector

# Q,

downstream Collector は、groupingBy()に限られますか？

いい質問です。

結論から言うと、**downstream Collector という概念は `groupingBy()` に限られず、他の複合 Collector でも使われます。**

---

## 🔹 「downstream Collector」とは？

`downstream Collector` とは、

> ある Collector が「内部で別の Collector を使って処理を委譲する」構造における、その内側の Collector
> 
> 
> のことです。
> 

### ✅ 代表的な「downstream を受け取る Collector」

| Collector | downstream を受け取る？ | 説明例 |
| --- | --- | --- |
| `groupingBy(classifier, downstream)` | ✅ | グループ化後に downstream で集約 |
| `partitioningBy(predicate, downstream)` | ✅ | true/false の2分割後に downstream を適用 |
| `mapping(mapper, downstream)` | ✅ | 値を変換してから downstream に渡す |
| `flatMapping(mapper, downstream)` | ✅ | 複数要素に変換後、まとめて downstream に渡す |
| `collectingAndThen(downstream, finisher)` | ✅ | downstream の結果に対して後処理を追加 |

---

## 🔸 例1：`partitioningBy` でも downstream を使う

```java
Map<Boolean, Long> partitioned = 
    List.of("a", "bb", "ccc").stream()
        .collect(Collectors.partitioningBy(
            s -> s.length() > 1,
            Collectors.counting()    // ← downstream
        ));
```

📌 `true` グループと `false` グループに分けたあと、それぞれの件数をカウント。

---

## 🔸 例2：`mapping` の中でも downstream が活躍

```java
Map<Integer, Set<Character>> mapped =
    List.of("cat", "car", "dog").stream()
        .collect(Collectors.groupingBy(
            String::length,
            Collectors.mapping(s -> s.charAt(0), Collectors.toSet()) // ← downstream
        ));
```

📌 文字列の長さでグループ分けし、各グループで1文字目の Set を作る。

---

## 🔸 例3：collectingAndThen も downstream を使う

```java
Collector<String, ?, List<String>> immutableListCollector =
    Collectors.collectingAndThen(
        Collectors.toList(),             // ← downstream
        Collections::unmodifiableList    // ← 最終変換処理
    );
```

📌 結果の List を不変（immutable）に変換。

---

## ✅ まとめ

- `downstream Collector` は **内部に別の Collector を含むような複合 Collector 全般に使われる概念**
- `groupingBy()` だけでなく、`partitioningBy()` や `mapping()` なども対象
- 実装上は、「部分的な変換」「集約処理」「後処理」などを委譲する役割を持つ

---

必要であれば、**downstream をカスタム実装する方法**や、**ネストの活用例**もお見せできます！

# Q,

downstream Collector の各メソッドは、groupingBy()などの引数としてだけではなく、単独でcollect()の引数として直接渡せる認識はあっていますか？

はい、**その認識は完全に正しいです。**

---

## ✅ 結論

> summingInt() や toList()、counting()、mapping() などの各種 Collector は、
> 
> 
> **`groupingBy()` の引数としても使えるし、単独で `collect()` に渡すこともできます。**
> 

それらはすべて `Collector<T, A, R>` 型のオブジェクトなので、

Stream API の `.collect(Collector)` にそのまま渡せます。

---

## 🔸 単独で使う例

```java
List<String> list = List.of("apple", "banana", "cherry");

// summingInt を単独で使う
int total = list.stream()
    .collect(Collectors.summingInt(String::length));

System.out.println(total); // 18
```

```java
// counting を単独で使う
long count = list.stream()
    .collect(Collectors.counting());

System.out.println(count); // 3
```

```java
// mapping を単独で使う（length を Set にする）
Set<Integer> lengths = list.stream()
    .collect(Collectors.mapping(String::length, Collectors.toSet()));

System.out.println(lengths); // [5, 6]
```

---

## 🔸 groupingBy の中でも使える（downstream）

```java
Map<Character, Integer> totalLengthByInitial =
    list.stream()
        .collect(Collectors.groupingBy(
            s -> s.charAt(0),
            Collectors.summingInt(String::length)
        ));
```

---

## ✅ まとめ

| Collector名 | `.collect()` に単独で使える？ | `.groupingBy()` の引数として使える？ |
| --- | --- | --- |
| `summingInt()` | ✅ | ✅ |
| `counting()` | ✅ | ✅ |
| `toList()` | ✅ | ✅（デフォルト） |
| `mapping()` | ✅ | ✅ |
| `reducing()` | ✅ | ✅ |

> 🔍 つまり「downstream Collector」として使えるものは、ふつうにトップレベルの Collector としても使えるのです。
> 

---

必要であれば、「downstream 専用」と「単独でも使える」Collector の違いや、カスタム Collector の作り方も解説できます。興味ありますか？