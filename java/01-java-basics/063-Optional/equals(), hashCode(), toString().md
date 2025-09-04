# equals(), hashCode(), toString()

# Q,

```java
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        return obj instanceof Optional<?> other
                && Objects.equals(value, other.value);
    }

    public int hashCode() {
        return Objects.hashCode(value);
    }

    public String toString() {
        return value != null
            ? ("Optional[" + value + "]")
            : "Optional.empty";
    }
```

上記のオーバーライド系のメソッドは、どのようなケースで使用しますか？

Hiroakiさんのコード例にある `equals` / `hashCode` / `toString` の3つのメソッドは、Javaの**Objectクラスから継承される代表的なオーバーライド対象**であり、主に以下のような目的・ケースで使用されます。

---

## ✅ 1. `equals(Object obj)`

### 【目的】2つの `Optional` の「中身の等価性」を比較

### 🔸 使用される場面：

| ケース | 具体例 |
| --- | --- |
| リスト・Setで `Optional` を含むオブジェクトの重複排除 | `Set<Optional<String>>` のような場合、値が同じかで重複チェック |
| テストコードでの比較 | `assertEquals(Optional.of("abc"), result)` のようなアサーション |
| 明示的な比較 | `opt1.equals(opt2)` |

### ✅ 実装ポイント：

```java
return obj instanceof Optional<?> other
        && Objects.equals(value, other.value);
```

- **`Optional.of("abc")` と `Optional.of("abc")` は true**
- **`Optional.of("abc")` と `Optional.of("xyz")` は false**
- **`Optional.empty()` 同士も true**

---

## ✅ 2. `hashCode()`

### 【目的】`Optional` を **HashMapやHashSetのキー・要素として使う場合**の整合性確保

### 🔸 使用される場面：

| ケース | 具体例 |
| --- | --- |
| `HashMap<Optional<String>, Integer>` などのキー比較 |  |
| `Set<Optional<T>>` での重複排除 |  |
| `Optional` をフィールドに持つオブジェクトの `hashCode()` 実装内 |  |

### ✅ 実装ポイント：

```java
return Objects.hashCode(value);
```

- `Optional.of("abc").hashCode()` と `Optional.of("abc").hashCode()` は一致
- `Optional.empty().hashCode()` は常に同じ固定値（＝nullのhash）

---

## ✅ 3. `toString()`

### 【目的】`Optional` の中身を**人間が見やすい形で表現**

### 🔸 使用される場面：

| ケース | 具体例 |
| --- | --- |
| ログ出力 | `System.out.println(opt)` → `Optional[abc]` |
| デバッグ表示 | IDE の変数ウォッチやログ |
| 開発者向けUI出力 | 文字列変換して画面に出す場合など |

### ✅ 出力例：

```java
Optional.of("Hiroaki") → "Optional[Hiroaki]"
Optional.empty()       → "Optional.empty"
```

---

## ✅ まとめ表

| メソッド | 主な用途 | 実務での使われ方 |
| --- | --- | --- |
| `equals()` | 値の等価性の判定 | 比較・テスト・コレクション操作 |
| `hashCode()` | ハッシュ構造での利用 | Map, Set のキーや要素 |
| `toString()` | 表示用文字列 | ログ出力・デバッグ・表示用UI |

---

## ✅ これらをオーバーライドする目的

- `Optional` を **他の通常クラスと同じように自然に扱える**ようにするため
- Javaの一貫したオブジェクト設計方針（`equals` と `hashCode` は常に対にする）

---

Hiroakiさんのようにクラス設計に関心がある方には、「`equals/hashCode` は必ずペアで設計」「toString は人間が読むための設計」などの設計原則も非常に役立ちます。

必要であれば、`Optional` を含むカスタムクラスに `equals/hashCode/toString` を正しく実装するパターン例もご紹介できます！