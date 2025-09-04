# reduce()

# Q,

```java
List<Integer> list = Arrays.asList(1,2,3,4,5);
Integer result
        = list.stream().reduce(0, (a, b) -> a + b);
```

上記のコードにて、result の型がIntegerになるのはなぜですか？
Optional<Integer> となる認識でした。

とても良い質問です！

たしかに `Stream#reduce()` を使うと `Optional<T>` が返されるケースもありますが、今回のコードでは **`Optional<Integer>` ではなく `Integer` が返っています**。

その理由を、ステップを追って解説します。

---

## ✅ コードの復習

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
Integer result = list.stream().reduce(0, (a, b) -> a + b);
```

---

## 🔍 `reduce()` のオーバーロードに注目！

### Stream には `reduce()` が3種類あります：

### ① `Optional<T> reduce(BinaryOperator<T> accumulator)`

```java
Optional<Integer> result = list.stream().reduce((a, b) -> a + b);
```

- **初期値なし**
- 結果が空ストリームだった場合に備えて、戻り値は `Optional<T>`

---

### ② `T reduce(T identity, BinaryOperator<T> accumulator)` ← ★今回これ！

```java
Integer result = list.stream().reduce(0, (a, b) -> a + b);
```

- **初期値（identity）あり**
- 初期値があるため、ストリームが空でも戻り値は常に非 `null`
- よって戻り値の型は `Optional<T>` ではなく **`T` そのもの（この場合 `Integer`）**

---

### ③ `U reduce(U identity, BiFunction<U,? super T,U> accumulator, BinaryOperator<U> combiner)`

- 並列処理に使う汎用形。今回は関係ないので割愛。

---

## ✅ つまり：なぜ `Integer` 型になるのか？

- あなたのコードでは `reduce()` に初期値 `0` を渡しているため、
- 戻り値は `Optional<T>` ではなく、**直接 `Integer` 型** になるのです。

---

## 🧪 比較してみよう

```java
List<Integer> list = Arrays.asList();

Integer sum1 = list.stream().reduce(0, (a, b) -> a + b); // → 0（初期値が使われる）
Optional<Integer> sum2 = list.stream().reduce((a, b) -> a + b); // → Optional.empty()
```

---

## ✅ まとめ

| メソッド呼び出し | 戻り値の型 | 空ストリームの場合 |
| --- | --- | --- |
| `reduce((a, b) -> a + b)` | `Optional<Integer>` | `Optional.empty()` |
| `reduce(0, (a, b) -> a + b)` | `Integer` | 初期値（0）が返る |

---

ご希望であれば、それぞれの `reduce()` メソッドの用途・使い分けや、初期値の意味（**モノイド則**との関係）もご説明できますよ！