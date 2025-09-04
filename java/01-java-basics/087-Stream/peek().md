# peek()

# Q,

peek()について解説してください。

`peek()` は、Java Stream API の **中間操作メソッド**で、

**各要素に対して副作用のある処理（主にデバッグ目的）を挟みたいときに使用する**メソッドです。

---

## ✅ peek() の定義

```java
Stream<T> peek(Consumer<? super T> action)
```

- `Consumer` 型の関数（`T -> void`）を受け取る
- 元の要素を変えずに、そのまま次に流す（中間操作）
- **ストリームの各要素に対して何らかの副作用を実行**
- 処理自体は `forEach()` と似ているが、**終端操作ではない**

---

## 🔸 主な用途：デバッグ・ログ出力

```java
List<String> list = List.of("apple", "banana", "cherry");

List<String> result = list.stream()
    .peek(s -> System.out.println("処理前: " + s))   // ← ログ出力
    .map(String::toUpperCase)
    .peek(s -> System.out.println("処理後: " + s))   // ← ログ出力
    .toList();
```

📌 出力例：

```java
処理前: apple
処理後: APPLE
処理前: banana
処理後: BANANA
処理前: cherry
処理後: CHERRY
```

---

## ❗ peek の注意点

### ① 終端操作がなければ実行されない

```java
list.stream()
    .peek(s -> System.out.println(s));  // これは何も出力しない！
```

➡️ **`peek()` は遅延評価（lazy）されるため、終端操作がないと実行されない**。

---

### ② 副作用目的に限定すること（値の変更は避ける）

```java
list.stream()
    .peek(s -> s = s.toUpperCase())  // ❌ 効果なし！元の要素は変わらない
    .toList();
```

➡️ `peek()` の中で値を変更しても、ストリームの流れには影響しない（破壊的でない）

---

### ③ 本来の用途は **「デバッグやログ」**

- `map()`：値を変換する
- `peek()`：値を**見て何かするだけ**（変換しない）

---

## ✅ peek() と forEach() の違い

| 比較項目 | `peek()` | `forEach()` |
| --- | --- | --- |
| ストリーム種別 | 中間操作（lazy） | 終端操作（eager） |
| 戻り値 | `Stream<T>` | `void` |
| 処理の継続 | 可能（チェーンできる） | 不可（ストリーム消費） |
| 主な用途 | デバッグ、ログ出力 | 最終出力、処理の実行 |

---

## 🔚 まとめ

| 項目 | 内容 |
| --- | --- |
| メソッド | `peek(Consumer<? super T> action)` |
| 戻り値 | `Stream<T>` |
| 用途 | デバッグ、ログ出力、動作確認 |
| 注意点 | 終端操作がないと実行されない（遅延評価） |
| 禁止事項 | 値の変更目的で使うのは NG（副作用限定） |

---

必要であれば、**実務で `peek()` を安全に活用するコツ**や、**バグにつながる NG 例**も紹介できます！