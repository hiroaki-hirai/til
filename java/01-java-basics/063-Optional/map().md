# map()

# Q,

```java
    public <U> Optional<U> map(Function<? super T, ? extends U> mapper) {
        Objects.requireNonNull(mapper);
        if (isEmpty()) {
            return empty();
        } else {
            return Optional.ofNullable(mapper.apply(value));
        }
    }
```

mapについて解説してください。

はい、`Optional<T>` の `map` メソッドは、Optionalが保持している値に**関数を適用し、その結果を新たなOptionalで返す**ためのメソッドです。

Javaの `Stream#map()` に似ていますが、単一の値に対する処理です。

---

## ✅ `Optional.map()` の基本構文

```java
public <U> Optional<U> map(Function<? super T, ? extends U> mapper)
```

### ▼ 意味の分解：

| 部分 | 意味 |
| --- | --- |
| `<U>` | 変換後の型（ジェネリクス） |
| `Function<? super T, ? extends U>` | `T`型の値を受け取って、`U`型の値を返す関数 |
| `Optional<U>` | 変換後の値をラップしたOptionalを返す |

---

## ✅ 使用イメージ（実例）

```java
Optional<String> nameOpt = Optional.of("Hiroaki");

Optional<Integer> lengthOpt = nameOpt.map(String::length);
// ⇒ Optional[7]
```

- `map` は `"Hiroaki"` に `String::length`（＝関数 `s -> s.length()`）を適用
- 返り値は `Optional<Integer>` になる

---

## ✅ null 安全な変換チェーン

```java
Optional<String> maybeName = Optional.ofNullable(getNameFromDb());

Optional<Integer> length = maybeName.map(String::length);
```

- `maybeName` が `null`（空）であっても `.map(...)` は安全にスキップされる
- `map` は **値が存在する場合のみ関数を適用し、存在しなければそのまま空を返す**

---

## ✅ ソースコード（SE21）

```java
public <U> Optional<U> map(Function<? super T, ? extends U> mapper) {
    Objects.requireNonNull(mapper);
    if (isEmpty()) {
        return empty();
    } else {
        return Optional.ofNullable(mapper.apply(value));
    }
}
```

### ▼ 解説：

| 処理 | 説明 |
| --- | --- |
| `Objects.requireNonNull(mapper)` | 関数そのものがnullであれば即エラー |
| `if (isEmpty())` | 中身が空なら `Optional.empty()` を返す |
| `mapper.apply(value)` | 関数を適用して結果を得る（null でもOK） |
| `Optional.ofNullable(...)` | 関数の実行結果が null の可能性があるため、安全に包む |

---

## ✅ `map` の使いどころ

- **値が存在するときだけ変換したい**
- **null チェックをせず、関数合成的に処理を連鎖させたい**
- **値の存在有無にかかわらず、統一した記述をしたい**

---

## ✅ `map` と `flatMap` の違い（補足）

| メソッド | 適用関数の戻り値 | 戻り値 | 用途 |
| --- | --- | --- | --- |
| `map` | `T -> U` | `Optional<U>` | 単純な変換 |
| `flatMap` | `T -> Optional<U>` | `Optional<U>` | ネストされたOptionalをそのまま返す |

---

## ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 意図 | 値がある場合にだけ関数を適用し、新しいOptionalを返す |
| 安全性 | nullチェック不要・Optionalが中で扱ってくれる |
| 関数型 | `Function<T, R>` による変換処理に適している |
| よくある用途 | 値の変換、長さ取得、属性アクセス（`map(User::getName)` など） |

---

必要であれば、`map()` の連続適用例、`Optional.map()` vs `Stream.map()` の比較、`map()` vs `ifPresent()` の使い分けなどもご案内します。

# Q,

mapメソッドは、引数を受け取って、その引数を使用して処理しその結果を戻り値として返すといった動作より、中間の処理に適している印象です。なのでStreamでも多用される認識です。
戻り値を返さないifPresentメソッドよりも、orElseGetメソッドとの比較が気になります。
引数を受け取らないが戻り値を返すorElseGetメソッドとmapメソッドの役割の違いなどを教えて下さい。

はい、Hiroakiさんの観察は非常に鋭いです。

特に「`map()` は中間処理に適しており、Streamでも多用される」という理解は、**関数合成・遅延評価・null安全処理**を理解する上でとても本質的です。

---

## ✅ `map()` と `orElseGet()` の違いを一言でいうと：

> map() は「Optionalの中に値がある場合にその値を変換する」ための処理であり、
> 
> 
> **`orElseGet()` は「Optionalが空だった場合に代替値を提供する」ための**処理です。
> 

つまり、**役割が真逆**です：

| メソッド | 値がある場合 | 値がない場合 | 目的 |
| --- | --- | --- | --- |
| `map(Function)` | ✅ 関数を適用 | ❌ 何もしない | 値の変換 |
| `orElseGet(Supplier)` | ❌ そのまま返す | ✅ サプライヤの結果を返す | 代替値の生成 |

---

## ✅ 構文と流れの違い

### 1. `map()`

```java
Optional<String> opt = Optional.of("Hiroaki");

Optional<Integer> result = opt.map(String::length);
// → 値あり："Hiroaki" → length(7) → Optional[7]
// → 値なし：そのまま Optional.empty()
```

- `map()` は「**Optionalの中にある値を変換**」
- 戻り値は **Optionalでラップされたまま**

---

### 2. `orElseGet()`

```java
Optional<String> opt = Optional.ofNullable(null);

String result = opt.orElseGet(() -> computeDefault());
// → 値あり：そのまま返す
// → 値なし：computeDefault() を呼んで値を返す
```

- `orElseGet()` は「**Optionalが空だったときに使う代替値の生成方法を提供**」
- 戻り値は **Optionalではなく、プレーンな型**

---

## ✅ 並べて比較：視点別まとめ

| 観点 | `map(Function)` | `orElseGet(Supplier)` |
| --- | --- | --- |
| 値の有無による動作 | 値があるときのみ処理 | 値がないときのみ処理 |
| 関数の種類 | `Function<T, R>` | `Supplier<T>` |
| 引数の有無 | あり（T） | なし |
| 戻り値の扱い | `Optional<U>` | `U`（プレーン型） |
| 用途 | 値の変換・加工 | デフォルト値の遅延生成 |
| 位置づけ | 中間操作 | 終端操作 |

---

## ✅ 実務的な使い分け例

### `map()`：中間処理で属性にアクセス

```java
Optional<User> userOpt = Optional.of(new User("Taro"));
Optional<String> nameOpt = userOpt.map(User::getName);
```

- 値があるかどうかを気にせず、`User → String` に変換できる

---

### `orElseGet()`：高コスト処理を必要なときだけ評価

```java
Optional<User> userOpt = Optional.ofNullable(null);
User user = userOpt.orElseGet(() -> fetchFromDatabase());
```

- 値がないときにだけ、DBアクセスやAPIコールを行いたいときに便利

---

## ✅ Streamとの連携も視野に

Hiroakiさんがご指摘の通り、`map()` は Stream API においても「中間操作」として同じ思想で機能します：

```java
List<String> names = List.of("Taro", "Jiro", "Hanako");
List<Integer> lengths = names.stream()
    .map(String::length)
    .toList();  // [4, 4, 6]
```

- `Optional.map()` と `Stream.map()` は発想が共通（値が存在するものにのみ適用）

---

## ✅ まとめ

| 比較軸 | `map()` | `orElseGet()` |
| --- | --- | --- |
| 目的 | 値があるときの変換処理 | 値がないときの代替処理 |
| タイミング | 中間フェーズ | 終端フェーズ（値取り出し） |
| 戻り型 | `Optional<U>` | `U`（元の型） |
| 関数型 | `Function<T, R>` | `Supplier<T>` |

---

> 🔍 ポイント：map() は「あるものを加工」、orElseGet() は「ないときに用意」
> 

ご希望あれば、`map()` → `flatMap()` → `filter()` → `orElseGet()` までの**典型的なOptionalチェーン構文**を1つの流れとして解説することも可能です。

# Q,

```java
Optional<User> userOpt = Optional.of(new User("Taro"));
Optional<String> nameOpt = userOpt.map(User::getName);
```

上記のコード例のように、mapメソッドの引数には関数インタフェースを前提としてラムダ式やメソッド参照を用いて合成する処理を記述するが、それは抽象クラスにて直接処理を記述するようなものなので、mapメソッドの引数に入力されるのは関数インタフェースの参照。
ラムダ式などの中身ではなく関数インタフェースの参照自体がnullの場合のnull安全機能がObjects.requireNonNull(mapper);である認識は合っていますか？

はい、Hiroakiさんのご認識は**完全に正確**です。以下に整理して明確に解説します。

---

## ✅ 結論から

> Objects.requireNonNull(mapper); の目的は、
> 
> 
> **`map()` メソッドに渡された関数インタフェース（＝その参照）が `null` かどうかを検査し、使用前に明示的にエラーを出すため**です。
> 

---

## ✅ 具体的な流れを図式化すると

```java
Optional<String> nameOpt = userOpt.map(User::getName);
```

### 🔁 実際に渡されているものは？

- `User::getName` は `Function<User, String>` という**関数インタフェースを実装したオブジェクト**です（実体はラムダやメソッド参照）
- それは「関数の処理内容」ではなく、**その処理を表す参照（＝ファンクショナルオブジェクト）**

### 🔍 その参照自体が `null` の場合は？

```java
userOpt.map(null); // ← 実際に null を渡すとここで NPE が起こる
```

➡ それを防ぐために：

```java
Objects.requireNonNull(mapper); // ← mapper（Function参照）が null なら明示的にエラーを出す
```

---

## ✅ map() 内の null チェックの2種類（再確認）

| チェック対象 | 変数 | チェック目的 |
| --- | --- | --- |
| 関数オブジェクトの参照（ラムダ式 or メソッド参照） | `mapper` | 呼び出し元のコーディングミスを防ぐ |
| Optionalの中身の値 | `value` | 値がなければ処理をスキップ（= 安全なチェーン処理） |

---

## ✅ 関数インタフェースの性質との関係

- ラムダ式やメソッド参照は「インラインで定義された `Function<T, R>` の匿名実装」
- したがって、`map(Function)` に渡されるのは、**Function型の参照**
- その参照自体が `null` の場合、`mapper.apply(...)` を実行すると `NullPointerException`
- なので、**事前に `Objects.requireNonNull(mapper)` で防衛的チェックをしている**

---

## ✅ 補足：抽象クラスとの違いは？

- 抽象クラスはクラス継承の仕組みで処理を差し替えるのに対し、
- 関数インタフェースは**「1つの処理だけを渡せばよい軽量な戦略オブジェクト」**
- それでも「参照型」である点は同じなので、**nullチェックは必要**

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| `map()` の引数 | `Function<? super T, ? extends U>` の参照（ラムダやメソッド参照） |
| `Objects.requireNonNull(mapper)` の目的 | 引数として渡された **Function型オブジェクトの参照が null** であることを検出し、安全性を確保する |
| Hiroakiさんの理解 | ✅ 正確：「ラムダなどの中身ではなく、参照自体の null 安全性のための防御処理」 |

---

ご希望あれば、関数インタフェースの null チェックがどう他のAPI（たとえば `Stream.map()` や `CompletableFuture.thenApply()`）でも使われているかも確認できます。