# of（）

# Q,

```java
    public static <T> Optional<T> of(T value) {
        return new Optional<>(Objects.requireNonNull(value));
    }

    public static <T> T requireNonNull(T obj) {
        if (obj == null)
            throw new NullPointerException();
        return obj;
    }
```

上記はse21のOptional.ofメソッドの宣言ですが、requireNonNullメソッドにてnull出ない場合はそのまま値を戻してその値をOptional型で包んで戻しているだけの構造という理解は合っていますか？

はい、Hiroakiさんの理解は**完全に正しい**です。

以下に、動作の流れと意図を補足しながら整理します。

---

## ✅ 結論

> Optional.of(value) は、null でないことを強制し、そのまま値をラップして返すだけの構造です。
> 
> 
> そのチェックには `Objects.requireNonNull(...)` を使っています。
> 

---

## 🔍 実装の意味と流れ（Java SE 21）

```java
public static <T> Optional<T> of(T value) {
    return new Optional<>(Objects.requireNonNull(value));
}
```

補助メソッド：

```java
public static <T> T requireNonNull(T obj) {
    if (obj == null)
        throw new NullPointerException();
    return obj;
}
```

---

## ✅ 動作フロー（逐次）

1. `Optional.of(value)` に値が渡される
2. `Objects.requireNonNull(value)` で null チェックが行われる
    - `null` だったら `NullPointerException` を即時スロー
    - `非null` だったらそのまま値を返す
3. `new Optional<>(value)` により `Optional` のインスタンスが作られる（中身は `value`）

---

## ✅ 目的

- 呼び出し元に対して **「nullは絶対に許されない」**という契約を明示する
- 実行時に `null` が渡されたら即座にエラーを出すことで、**バグの早期発見**が可能になる
- ラップ対象の `value` が明らかに「存在することが前提」なときに使う

---

## ✅ Hiroakiさんの理解の要約表現と対応

| Hiroakiさんの理解 | 実装の対応 | コメント |
| --- | --- | --- |
| nullでないかチェック | `requireNonNull(value)` | ✔️ 正確 |
| nullでなければ値をそのまま戻す | `return obj;` | ✔️ 正確 |
| その値をOptionalで包む | `new Optional<>(...)` | ✔️ 正確 |

---

## 💡 実用ポイント

このメソッドを使うときは、例えば次のような状況です：

```java
User user = ... // nullじゃないと確定している
Optional<User> opt = Optional.of(user);  // ✅ 明示的な設計意図
```

逆に少しでも null の可能性があるなら：

```java
Optional.ofNullable(user);  // ✅ 安全性重視
```

---

必要であれば、Optionalの内部構造（フィールド構成やisPresentの実装など）まで掘り下げてご説明できます。ご希望ですか？

# Q,

```java
    public Optional<String> getNameIfPresent() {
        return (name == null || name.isBlank()) ? Optional.empty() : Optional.of(name);
```

上記解説のコードでは、nullではないことが確定している条件分岐にOptional.ofメソッドがあります。
nullではないことが確定しているのであれば、その値を戻せば良いように思いますが、なぜOptional.ofメソッドを使用するのでしょうか？

非常に良い視点です、Hiroakiさん。

ご指摘のとおり、「nullではないと確定しているのなら、値そのものを返せばよいのでは？」という疑問はもっともです。

そのうえで、**なぜ `Optional.of(name)` を使うのか**を整理してお答えします。

---

## ✅ 結論

> このメソッドの戻り値の型が Optional<String> であるため、
> 
> 
> `String` をそのまま返すのではなく、`Optional` にラップして返す必要がある。
> 
> その際、「nullではない」と確定しているので `Optional.of(...)` を使うのが適切。
> 

---

## 🔍 具体的に見てみましょう

```java
public Optional<String> getNameIfPresent() {
    return (name == null || name.isBlank()) ? Optional.empty() : Optional.of(name);
}
```

- 戻り値の型：`Optional<String>`
- `name` の型：`String`

---

### ❌ 間違った例（型不一致）

```java
return name;  // ← コンパイルエラー！
```

- `name` は `String` 型
- 戻り値は `Optional<String>` 型 → ラップされていない値を返すと **型が合わない**

---

## ✅ ではなぜ `Optional.of(...)` を使うのか？

### `Optional.of(...)` と `Optional.ofNullable(...)` の違い：

| メソッド | nullを許容するか | 特徴 |
| --- | --- | --- |
| `Optional.of(value)` | ❌ 非許容（nullだとNPE） | nullでないことが**明白なときに使う** |
| `Optional.ofNullable(value)` | ✅ 許容（nullなら empty） | 値がnullの可能性があるときに使う |

---

### ここでは `name == null` を条件で除外しているため：

```java
(name == null || name.isBlank())  // ← nullを除外
```

➡ つまり、`else` 側では `name` は **非null確定**

➡ だから `Optional.of(name)` を安心して使える

➡ そして明示的に「ここではnullではない」ことをコードで示せる（設計意図が明確）

---

## ✅ まとめ

| 観点 | 内容 |
| --- | --- |
| なぜ `Optional.of(name)` を使う？ | 戻り値の型が `Optional<String>` だから、ラップが必要 |
| なぜ `ofNullable` ではなく `of`？ | すでに `null` チェック済みで、値の存在が保証されているから |
| 単に `name` を返すと？ | 型が違うためコンパイルエラーになる |

---

Optionalは「設計の意図（＝値がある／ない）」を**型で表現する道具**なので、`Optional.of(...)` を使うことで「値は確実にある」ということを**呼び出し側に対して明確に示す**ことになります。

ご希望あれば、`Optional` を使った「設計意図の表現力」例なども紹介できますよ。