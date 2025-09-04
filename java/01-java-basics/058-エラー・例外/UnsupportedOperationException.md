# UnsupportedOperationException

# Q,

UnsupportedOperationException がスローされるケースを挙げてください。

はい、Hiroakiさん。

`UnsupportedOperationException` は、Javaのコレクションなどで「**その操作がサポートされていない場合にスローされる例外**」です。

**実行時例外（`RuntimeException`）**であり、通常は不変（変更不可）のオブジェクトに対して変更しようとしたときに発生します。

---

## 🔷 よくあるケース一覧

| ケース | 説明 | 例 |
| --- | --- | --- |
| `List.of(...)` に対して add/remove | Java 9以降の `List.of()` は**不変リスト** | `List<String> list = List.of("A", "B"); list.add("C");` |
| `Arrays.asList(...)` に対して add/remove | サイズ固定のリスト（add/remove非対応） | `List<String> list = Arrays.asList("A", "B"); list.add("C");` |
| `Collections.unmodifiableList()` に対して変更操作 | 明示的に不変にされたリスト | `List<String> list = Collections.unmodifiableList(new ArrayList<>()); list.add("X");` |
| `Map.of(...)` や `Set.of(...)` も同様 | Java 9以降の不変マップやセット | `Map<String, String> map = Map.of("A", "a"); map.put("B", "b");` |
| 一部のコレクションビューで remove など | `List.subList()` や `Map.entrySet()` などの一部ビューでは操作制限があることがある | `list.subList(0, 2).clear();`（場合により例外） |

---

## 🔸 実例1：`List.of(...)` の変更で例外

```java
List<String> list = List.of("a", "b", "c");
list.add("d");  // UnsupportedOperationException
```

---

## 🔸 実例2：`Arrays.asList()` のサイズ変更

```java
List<String> list = Arrays.asList("a", "b");
list.add("c");  // UnsupportedOperationException
```

- `Arrays.asList()` は固定サイズのリストを返すため、`set()` は可能でも `add()` や `remove()` は不可。

---

## ✅ まとめ：UnsupportedOperationException が発生する代表的な操作

| 操作対象 | 発生しやすい操作 |
| --- | --- |
| `List.of(...)`, `Set.of(...)` | `add`, `remove`, `clear` |
| `Arrays.asList(...)` | `add`, `remove`（`set`はOK） |
| `Collections.unmodifiableXXX(...)` | いかなる変更操作もNG |

---

さらに詳しく、どの操作が許可されているか知りたい場合は、表形式の比較一覧も作れますよ。希望があれば教えてください！