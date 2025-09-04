# map()

# Q,

```java
List<Integer> list = Arrays.asList(1,2,3,4,5);
Stream<String> result
        = list.stream().map(n -> String.valueOf(n));
```

上記のコードにて、resultの型がStream<String> であるのは、map()にてStirngクラスのvalueOfメソッドを使用しているからということでしょうか？

はい、ほぼその通りです。

もう少し厳密に整理すると、「**`map()` に渡されたラムダ式の戻り値の型によって、`Stream<T>` の型が決まる**」というのが本質です。

では、詳しく見ていきましょう。

---

## ✅ あなたのコードの流れ

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);

Stream<String> result = list.stream()
                            .map(n -> String.valueOf(n));
```

---

## 🔍 `map()` の定義（Stream API）

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper)
```

- **T**：元のストリーム要素の型（この場合 `Integer`）
- **R**：変換後の型（この場合 `String`）

したがって：

- `list.stream()` の型は `Stream<Integer>`
- `map(n -> String.valueOf(n))` により各 `Integer` を `String` に変換
- 戻り値の型が `Stream<String>` になる

---

## ✅ なぜ `Stream<String>` になるのか？

ポイント：

```java
n -> String.valueOf(n)
```

- このラムダ式は、**`Integer` を `String` に変換している**
- `String.valueOf(n)` の戻り値の型は `String`
- `map()` はこの戻り値型を使って、新たな `Stream<R>` を生成する

したがって、**結果のストリームは `Stream<String>`** になります。

---

## 🎯 補足：`String.valueOf()` とは？

```java
public static String valueOf(Object obj)
```

- 引数が `null` でなければ → `obj.toString()` を返す
- 引数が `null` なら `"null"` という文字列を返す（← 例外は出さない）

---

## ✅ まとめ

| 観点 | 内容 |
| --- | --- |
| `map()` の役割 | 各要素を別の型に変換して、新しいストリームを作る |
| 戻り値の型 | ラムダ式（Function）の**戻り値の型**に従って決まる |
| 今回の型変換 | `Integer` → `String` |
| 結果の型 | `Stream<String>` |

---

もしご希望であれば、`mapToInt()` や `flatMap()` との違い、あるいは Function の型推論の流れも説明できます！

# Q,

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper)
```

Fanction<入力の型(引数), 出力の型(戻り値)> であることは把握していますが、<? super T, ? extends R>といった表記になっていることについて解説してください。

素晴らしい視点です！

`map(Function<? super T, ? extends R> mapper)` のように、**ジェネリクスにおけるワイルドカード（`? super T`, `? extends R`）が指定されている理由**は、Java の**型安全性と柔軟性の両立**のためです。

---

## ✅ まず前提：`Function<T, R>` の役割

- `Function<T, R>` は、
    - 引数：`T` 型の値
    - 戻り値：`R` 型の値
- つまり、`T → R` へのマッピング関数

---

## ✅ では、なぜ `Function<? super T, ? extends R>` なのか？

### ▶ `? super T`（下限ワイルドカード）

- **引数の型に対して**：
    
    `Function` の `apply(T t)` メソッドに `T` を渡すため、
    

```java
T t = ...;
f.apply(t); // T を引数に渡せる必要あり
```

- ここで `Function<? super T, ?>` にしておくと：
    - `T` のスーパークラス（例えば `Object` や `Number`）を引数として受け取る `Function` も使用可能に！
    - → より柔軟に使える（下限ワイルドカード）

---

### ▶ `? extends R`（上限ワイルドカード）

- **戻り値の型に対して**：
    
    `map()` の戻り値は `Stream<R>` 型になるので、
    

```java
R result = f.apply(t);
```

- ここで `Function<?, ? extends R>` にしておくと：
    - `R` を返す関数だけでなく、`R` のサブクラス（例えば `String`）を返す関数も OK
    - → より広い型の関数を許容（上限ワイルドカード）

---

## ✅ まとめ：なぜこう書くのか？

| 記述 | 意味 | 目的 |
| --- | --- | --- |
| `? super T` | T の親クラスでもよい | 引数に使える型の幅を広げる |
| `? extends R` | R のサブクラスでもよい | 戻り値の型の柔軟性を持たせる |
| `Function<? super T, ? extends R>` | T を受け取り R を返す関数なら、それに近い範囲も許容 | 柔軟で型安全な設計 |

---

## ✅ 実例で比較

```java
List<Integer> list = List.of(1, 2, 3);

// Function<Number, String> も許容される
Function<Number, String> func = n -> "値は " + n;

Stream<String> result = list.stream().map(func); // OK!
```

- `list.stream()` は `Stream<Integer>`
- `Function<Number, String>` は `Function<? super Integer, ? extends String>` に合致
    - `Number` は `Integer` の親クラス → `? super T` に合う
    - `String` は `String` のサブクラス → `? extends R` に合う

---

## ✅ 補足：ワイルドカードの使い分け

| ワイルドカード | 意味 | 主な使いどころ |
| --- | --- | --- |
| `? extends T` | T のサブクラスすべて | **読み出し専用**（get）で安全 |
| `? super T` | T のスーパークラスすべて | **書き込み専用**（put）で安全 |

> 暗記フレーズ：PECS
> 
> 
> **Producer Extends, Consumer Super**
> 
> → 生産する（返す）なら `extends`、消費する（受け取る）なら `super`
> 

---

ご希望であれば、実際に `? extends` / `? super` の使い分けが必要になるケース（例：コレクションに add できない例など）も紹介できます！