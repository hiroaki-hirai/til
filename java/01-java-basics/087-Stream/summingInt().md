# summingInt()

# Q,

summingInt()について解説してください。

`summingInt()` は、Java Stream API の `Collector` のひとつで、**Stream 内の要素に対して「int 値を取り出して合計する」処理を行う Collector** です。

`Collectors.summingInt(ToIntFunction<? super T> mapper)` という形で使い、数値集計を簡潔に記述できます。

---

## 🔹 基本構文

```java
Collectors.summingInt(ToIntFunction<? super T> mapper)
```

- `mapper`: ストリーム要素から `int` を取り出す関数（ラムダ式など）

---

## 🔸 使用例：要素の合計を求める

```java
List<String> list = List.of("apple", "banana", "cherry");

int totalLength = list.stream()
    .collect(Collectors.summingInt(String::length));

System.out.println(totalLength); // 6 + 6 + 6 = 18
```

- 各要素の文字数を `int` として取り出し、全体を合計
- `String::length` が `ToIntFunction<String>` として機能

---

## 🔸 使用例：オブジェクトのフィールドの合計

```java
record Item(String name, int price) {}

List<Item> items = List.of(
    new Item("Pen", 100),
    new Item("Notebook", 200),
    new Item("Pencil", 50)
);

int total = items.stream()
    .collect(Collectors.summingInt(Item::price));

System.out.println(total); // 100 + 200 + 50 = 350
```

---

## 🔸 `summingLong()` / `summingDouble()` との違い

| メソッド名 | 対応する型 | 戻り値の型 |
| --- | --- | --- |
| `summingInt()` | `int` | `Integer` |
| `summingLong()` | `long` | `Long` |
| `summingDouble()` | `double` | `Double` |

※ 戻り値はプリミティブ型ではなくラッパー型になる点に注意。

---

## 🔸 `groupingBy()` と組み合わせる（集計 per グループ）

```java
Map<Character, Integer> totalLengthByInitial =
    list.stream().collect(Collectors.groupingBy(
        s -> s.charAt(0),
        Collectors.summingInt(String::length)
    ));

```

📌 先頭文字ごとに文字数を合計。

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 主な用途 | 要素から `int` を取り出して合計 |
| 関数型引数 | `ToIntFunction<T>` |
| 戻り値 | `Integer`（ラッパークラス） |
| よく使う場面 | 集計（合計金額、総数など） |
| 応用 | `groupingBy()` と組み合わせてグループ別集計 |

---

必要なら `reducing()` を使って同じことを手動でやるパターンとの比較もできますよ。ご希望があれば続けます！