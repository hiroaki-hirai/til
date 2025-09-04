# List Set の相互変換、順序扱い

ここでは、**SetとListの相互変換**と、それに伴う**順序の扱い**の違いについて、

**構造の違い・実装例・順序保証の有無**を明確に整理して解説します。

---

## ✅ 1. 基本方針：Set ⇔ List の相互変換

| 変換方向 | 使用例 | 意図 |
| --- | --- | --- |
| List → Set | `new HashSet<>(list)` | 重複削除 |
| Set → List | `new ArrayList<>(set)` | 順序操作を可能にする |

---

## ✅ 2. List → Set の変換（重複の除去）

### ◼ 実装例

```java
List<String> list = Arrays.asList("A", "B", "A", "C");
Set<String> set = new HashSet<>(list);
System.out.println(set); // [A, B, C]（順序保証なし）
```

### ◼ 注意点

- **重複要素は自動的に除去**される
- **順序は保証されない**（`HashSet`の場合）

---

## ✅ 3. Set → List の変換（順序を扱えるようにする）

### ◼ 実装例

```java
Set<String> set = new HashSet<>();
set.add("B");
set.add("A");
set.add("C");

List<String> list = new ArrayList<>(set);
System.out.println(list); // [B, A, C] など（Setの順序次第）
```

### ◼ 注意点

- `List`に変換しても、元が`HashSet`なら**順序はバラバラ**
- 順序が必要な場合は、`LinkedHashSet` や `TreeSet` を使う

---

## ✅ 4. 順序保持したい場合の選択肢

### ◼ 元のListの順序を保持したい（→Set化したあとも）

```java
List<String> list = Arrays.asList("A", "B", "A", "C");
Set<String> set = new LinkedHashSet<>(list);
System.out.println(set); // [A, B, C]（追加順を保持）

List<String> list2 = new ArrayList<>(set); // 再List化も順序保持される
```

---

### ◼ 自然順（ソート済）で扱いたい

```java
Set<String> set = new TreeSet<>();
set.add("B");
set.add("A");
set.add("C");

List<String> list = new ArrayList<>(set);
System.out.println(list); // [A, B, C]（昇順ソート済）
```

---

## ✅ 5. まとめ：順序の扱いに関する違い

| 種類 | 順序保持 | ソート | 主な目的 |
| --- | --- | --- | --- |
| `HashSet` | ❌ | ❌ | 高速な重複排除 |
| `LinkedHashSet` | ✅ | ❌ | 順序を保った重複排除 |
| `TreeSet` | ✅ | ✅ | 自然順 or Comparator順の重複排除 |
| `ArrayList` | ✅ | ❌ | インデックス順・順序操作 |

---

## ✅ 補足：相互変換を行う際の落とし穴

| 落とし穴 | 説明 |
| --- | --- |
| `Set → List` の順序に期待すると崩れる | 元が `HashSet` だと順序が不安定 |
| `List → Set → List` で順序が消える | 中間が `HashSet` の場合順序は崩れる |
| `null` を `TreeSet` に渡すと例外 | ソートできないため `NullPointerException` |

---

## ✅ 実務応用シーン例

| シーン | 適した構造 |
| --- | --- |
| ユーザーの入力リストから重複除去 | `new LinkedHashSet<>(list)` |
| タグ一覧をアルファベット順で表示 | `new TreeSet<>(list)` |
| 重複なしで順不同なチェック用 | `new HashSet<>(list)` |
| 特定順序を操作したい（先頭挿入など） | `List` |

---

## ✅ まとめのポイント

- **Listは順序操作・重複OK**
- **Setは重複NG・順序は実装依存**
- 順序が重要な場合は `LinkedHashSet` または `TreeSet` を使う
- 相互変換の際は、**順序・重複の意図を明確にして使い分ける**