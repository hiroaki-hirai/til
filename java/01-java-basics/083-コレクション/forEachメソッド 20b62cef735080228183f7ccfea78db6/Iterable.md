# Iterable

ここでは、`Iterable` インターフェースに定義された `forEach` メソッドにフォーカスして詳しく解説します。

---

## ✅ `Iterable` の `forEach` とは？

`Iterable` は Java のすべてのコレクション（`List`, `Set`, `Queue`など）の**最上位インターフェース**であり、`for-each` 構文（拡張for文）や `forEach()` メソッドの基盤となります。

```java
public interface Iterable<T> {
    Iterator<T> iterator();

    // Java 8 で追加された default メソッド
    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
}
```

---

## 🔍 特徴まとめ

| 項目 | 説明 |
| --- | --- |
| 所属 | `java.lang.Iterable<T>` |
| 対象クラス | すべてのコレクション（`List`, `Set`, `Queue`など） |
| 引数 | `Consumer<? super T>`：1つの引数を受け取って返さない関数型インターフェース |
| 戻り値 | なし（`void`） |
| 実行内容 | 要素に対して `Consumer#accept(T)` を順番に実行 |

---

## ✨ 使用例（List）

```java
List<String> names = List.of("Hiroaki", "Taro", "Yuki");

names.forEach(name -> System.out.println("Hello, " + name));
```

これは次のコードと本質的に同じ処理です：

```java
for (String name : names) {
    System.out.println("Hello, " + name);
}
```

---

## 🔧 内部構造（自動変換）

`forEach()` は実際には以下のようなことをしている：

```java
for (T element : this) {
    consumer.accept(element);
}
```

つまり、**`iterator()` メソッドを使って、各要素に処理を適用している**ということです。

---

## 🧠 `Consumer<T>` とは？

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

- `forEach` に渡すラムダ式はこの `accept(T)` を実装する関数です。
- 例：`x -> System.out.println(x)`

---

## 📌 `Iterable` の `forEach` の制約と注意点

| 注意点 | 内容 |
| --- | --- |
| **途中で break/continue 不可** | `forEach` はループ制御構文を使えない。代替は `return`。 |
| **例外処理がしづらい** | ラムダ内で `try-catch` が必要 |
| **並列処理なし** | 並列処理には `Stream.forEach()` or `parallelStream()` を使用 |

---

## 🧩 よくある誤解との違い

| 誤解 | 正しい理解 |
| --- | --- |
| `forEach()` は Collection に定義されている？ | ✖ `Iterable` に定義されている |
| `forEach()` と Stream の `forEach()` は同じ？ | ✖ 別物（Stream は遅延処理＋並列対応） |
| `Map` にも使える？ | ✖ `Map` は `Iterable` を継承していないが、`Map#forEach(BiConsumer)` が別途ある |

---

## 🧪 応用：メソッド参照を活用

```java
List<String> list = List.of("A", "B", "C");

// メソッド参照を使用
list.forEach(System.out::println);
```

---

## 🔚 まとめ

| 項目 | 内容 |
| --- | --- |
| 定義場所 | `Iterable` インターフェース |
| 目的 | 要素ごとに処理（`Consumer`を使う） |
| メリット | 簡潔、ラムダ式との相性が良い |
| デメリット | 中断不可、例外処理しづらい |
| 類似構文との違い | `for-each`文より柔軟 / `Stream#forEach`とは別物 |

Javaの `forEach` メソッドは、**コレクションの各要素に対して処理を繰り返すためのメソッド**で、Java 8 から導入された**関数型インターフェース**（特に `Consumer`）と組み合わせて使います。

---

## ✅ 基本情報：`forEach` メソッドとは

| 項目 | 内容 |
| --- | --- |
| 所属 | `Iterable` インターフェース（`List`, `Set`, `Queue` などで使用可能） |
| 導入 | Java 8 |
| 目的 | 各要素に順にアクセスして処理を行う（ラムダ式と相性◎） |
| 引数 | `Consumer<T>` 型の関数的インターフェース |
| 戻り値 | なし（`void`） |
| 処理の流れ | 各要素に対して `accept(T t)` を順に実行 |

---

## 🧪 使い方例（List）

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Hiroaki", "Taro", "Yuki");

        // ラムダ式で出力
        names.forEach(name -> System.out.println("こんにちは " + name));
    }
}
```

### 🔍 出力結果：

```java
こんにちは Hiroaki
こんにちは Taro
こんにちは Yuki
```

---

## 🔄 for-each文との比較

| 比較項目 | 拡張for文 (`for(String name : names)`) | `forEach` メソッド |
| --- | --- | --- |
| 構文 | 文法ベース | メソッドベース（関数型） |
| 柔軟性 | 制限あり（中断・例外処理は書ける） | 高い（ラムダ/匿名クラス使用可能） |
| 直感性 | Java従来の書き方に近い | 関数型志向に近い |
| Streamとの連携 | × | ◎（`stream().forEach(...)`） |

---

## 🧰 よくある用途

| 用途 | 例 |
| --- | --- |
| デバッグ出力 | `list.forEach(System.out::println);` |
| 集計・処理ログ | `map.forEach((k, v) -> System.out.println(k + "=" + v));` |
| UI・DB処理の繰り返し呼び出し | `users.forEach(user -> db.save(user));` |

---

## ⚠️ 注意点

| 注意点 | 内容 |
| --- | --- |
| 中断できない | `forEach` は `break` や `continue` を使えない（代わりに `return` でスキップ） |
| 例外処理が書きにくい | `try-catch` をラムダの中に明示的に書く必要がある |
| `Map`では`Iterable`ではない | `Map` 自体には `forEach` あり（Java 8～）が、`Collection` とは独立実装 |

```java
Map<String, Integer> scores = Map.of("Hiroaki", 90, "Taro", 80);
scores.forEach((key, value) -> System.out.println(key + ": " + value));
```

---

## 🧩 応用：メソッド参照

```java
List<String> names = List.of("A", "B", "C");
names.forEach(System.out::println);  // ラムダの簡略表記
```

---

## 💡 まとめ

| 特徴 | 説明 |
| --- | --- |
| モダンで簡潔な記法 | Java 8以降のラムダ式に最適 |
| イテレーションの簡素化 | ループの記述を減らせる |
| 処理の柔軟性 | メソッド参照・匿名クラス・ラムダ式に対応 |
| 注意点 | 途中中断不可、例外処理は工夫が必要 |

# Q,

```java
List<String> list = List.of("A", "B", "C");
list.forEach(item -> System.out.println(item));
```

上記の場合、forEachメソッド内部ではどのような経緯で展開されていきますか？

はい、Hiroakiさんのこのコード：

```java
List<String> list = List.of("A", "B", "C");
list.forEach(item -> System.out.println(item));
```

は、内部的には `List`（具体的には `ImmutableCollections.ListN` など）の `forEach` メソッドが呼び出されます。

その処理は `Iterable<T>` インタフェースの **defaultメソッド** として実装されているので、以下のように展開されます：

---

## ✅ 展開されるイメージ

```java
Consumer<? super String> action = item -> System.out.println(item);

for (String t : list) {
    action.accept(t);  // ← item -> System.out.println(item) が呼び出される
}
```

つまり `forEach` の中身はこうです：

```java
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```

この `T` は `String` に置き換わり、ラムダ式 `item -> System.out.println(item)` は `Consumer<String>` として機能します。

---

## ✅ 全体の流れ（具体的に）

1. `list.forEach(...)` を呼び出す
2. `List` は `Iterable` を実装しているため、`forEach` の default メソッドが使われる
3. `Consumer<String>` 型のラムダ式が引数として渡される
4. `for (String t : list)` でリストを1件ずつ取り出す
5. `action.accept(t)` で、ラムダ式が実行される（＝ `System.out.println(t)`）

---

## ✅ ラムダ式とConsumerの関係

このラムダ：

```java
item -> System.out.println(item)
```

は、次の無名クラスと等価です：

```java
new Consumer<String>() {
    public void accept(String item) {
        System.out.println(item);
    }
}
```

---

## ✅ 最終的に実行されるコードイメージ（まとめ）

```java
for (String item : list) {
    System.out.println(item);
}
```

という `拡張for文` に等しい動作です。

ただし、ラムダを渡せることでより柔軟で再利用可能な処理ができるという利点があります。

---

ご希望があれば、`forEach` のラムダ式をメソッド参照に置き換える例や、自作クラスに `Iterable<T>` を実装して `forEach` を有効化するパターンも紹介できます！