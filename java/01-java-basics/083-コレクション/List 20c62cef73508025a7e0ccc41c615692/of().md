# of()

# Q,

```java
static <E> List<E> of() {
        return (List<E>) ImmutableCollections.EMPTY_LIST;
    }

    static <E> List<E> of(E e1) {
        return new ImmutableCollections.List12<>(e1);
    }

    static <E> List<E> of(E e1, E e2) {
        return new ImmutableCollections.List12<>(e1, e2);
    }

...

    static <E> List<E> of(E e1, E e2, E e3, E e4, E e5, E e6, E e7, E e8, E e9, E e10) {
        return ImmutableCollections.listFromTrustedArray(e1, e2, e3, e4, e5,
                                                         e6, e7, e8, e9, e10);
    }

    static <E> List<E> of(E... elements) {
        switch (elements.length) { // implicit null check of elements
            case 0:
                @SuppressWarnings("unchecked")
                var list = (List<E>) ImmutableCollections.EMPTY_LIST;
                return list;
            case 1:
                return new ImmutableCollections.List12<>(elements[0]);
            case 2:
                return new ImmutableCollections.List12<>(elements[0], elements[1]);
            default:
                return ImmutableCollections.listFromArray(elements);
        }
    }
```

List.ofメソッドの宣言部分が複雑です。
大まかな構造や目的を上記se21の宣言を元に解説してください。

Hiroakiさんのご質問の `List.of(...)` メソッド群は、**Java 9** で導入された**簡潔で不変なリスト作成メソッド群**です。Java SE 21 でもこの構造は基本的に同じで、以下のような意図と構造で設計されています。

---

### 🔷 全体の目的：**イミュータブルなリストを簡潔に生成**

```java
List<String> list = List.of("a", "b", "c");
```

上記のような構文で、不変な（変更不可な）`List` インスタンスを簡潔に作れるように設計されています。

---

## ✅ 大まかな構造と分岐の流れ

### 【1】オーバーロードの構造

```java
static <E> List<E> of()
static <E> List<E> of(E e1)
static <E> List<E> of(E e1, E e2)
...
static <E> List<E> of(E e1, ..., E e10)
static <E> List<E> of(E... elements)
```

- **1〜10個**の要素まで、**引数数に応じたオーバーロード**が用意されています（パフォーマンス・型安全性のため）。
- **11個以上**の場合は、最後の `varargs`（可変長引数）版 `List.of(E... elements)` が呼ばれます。

---

## 🔍 具体的な各メソッドの目的

| メソッド定義 | 処理内容 | 使用される内部クラス |
| --- | --- | --- |
| `of()` | 空リストを返す | `ImmutableCollections.EMPTY_LIST`（シングルトン） |
| `of(e1)` | 要素1つのリストを返す | `ImmutableCollections.List12` |
| `of(e1, e2)` | 要素2つのリストを返す | 同上 |
| `of(e1, ..., e10)` | 最大10個まで最適化 | `ImmutableCollections.listFromTrustedArray(...)` |
| `of(E... elements)` | 可変長：個数に応じて処理を振り分け | `EMPTY_LIST` / `List12` / `listFromArray` |

---

## 🧱 使用される内部構造（ImmutableCollections）

### 1. `EMPTY_LIST`：空リストのシングルトン

```java
static final List<?> EMPTY_LIST = new EmptyImmutableList<>();
```

- メモリ節約のため、共通のインスタンスを使い回します。

---

### 2. `List12<E>`：要素1〜2個用の軽量な専用クラス

```java
static final class List12<E> extends AbstractImmutableList<E>
```

- 最小限のフィールドと処理で高速・軽量。

---

### 3. `listFromArray()` / `listFromTrustedArray(...)`

- 可変長引数など、3個以上のときに使われる。
- 内部的には配列を持ち、イミュータブルな構造に変換。

---

## 📌 なぜこのような構造にしているのか？

| 理由 | 解説 |
| --- | --- |
| 🔒 不変性 | `List.of(...)` で生成されるリストは **要素の追加・削除ができない**。安全性向上。 |
| 🚀 パフォーマンス | 少数の要素（0～2）については高速な専用クラス `List12` を使って最適化。 |
| 🧼 可読性 | `new ArrayList<>(...)` や `Collections.unmodifiableList(...)` を使うよりも簡潔。 |
| ☑ 型推論 | `<E>` の型がコンパイル時に推論されるため安全。 |

---

## 🧪 例：処理の選択ロジック（varargs）

```java
static <E> List<E> of(E... elements) {
    switch (elements.length) {
        case 0: return EMPTY_LIST;
        case 1: return new List12<>(elements[0]);
        case 2: return new List12<>(elements[0], elements[1]);
        default: return listFromArray(elements);
    }
}
```

- 実行時の要素数で最適な構造を選択。
- `elements == null` のチェックは不要（暗黙の NPE）。

---

## 🧩 補足：変更不可リストと標準リストの違い

| 特徴 | `List.of(...)` | `new ArrayList<>(...)` |
| --- | --- | --- |
| 変更可否 | ❌ 不可（UnsupportedOperationException） | ✅ 可 |
| null要素許容 | ❌ 不可（NullPointerException） | ✅ 可 |
| パフォーマンス | 高速（用途に特化） | 汎用だがやや重い |

---

## 🏁 まとめ

- `List.of(...)` は **イミュータブルなリスト生成を簡潔に行うためのAPI**。
- **引数数に応じて最適な方法でリストを生成**（1〜2は軽量専用クラス、それ以降は配列ベース）。
- **変更不可、null非許容、パフォーマンス最適化**を特徴とし、安全なデフォルト値として使用されます。

---

この構造は `Map.of(...)` や `Set.of(...)` でも同様の思想で使われています。

必要ならそれらについても構造比較できますので、お知らせください。