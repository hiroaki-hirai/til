# null 許容

# Q,

配列やコレクションにて、nullを許容できるものと許容できないものを整理できますか？

はい、Javaにおける**配列や各種コレクション**で「`null`を許容するかどうか」は、使うクラス・APIごとに挙動が異なります。以下に代表的なものを**「nullの許容可否」で分類・整理**します。

---

### ✅ nullを許容する

| 種類 | クラス / API | 備考 |
| --- | --- | --- |
| 配列 | `Object[] arr = new Object[3];` | 初期値はすべて`null`。`arr[0] = null;`もOK |
| List | `ArrayList` | 明示的に`add(null)`可能 |
| List | `LinkedList` | `null`の挿入・検索・削除が可能 |
| Set | `HashSet` | `null`を1つだけ格納可（Setなので重複不可） |
| Map | `HashMap` | `null`キー：1つ、`null`値：複数可 |
| Map | `LinkedHashMap` | `HashMap`と同様、`null`キーと値を許容 |
| Map | `TreeMap`（※） | `null`値はOK、キーは`Comparator`次第で例外になる場合あり |
| Queue | `LinkedList`（Queueとして） | `null`の挿入は可能（ただし使い方に注意） |

---

### ❌ nullを許容しない

| 種類 | クラス / API | 理由・備考 |
| --- | --- | --- |
| List | `CopyOnWriteArrayList` | `NullPointerException`が発生 |
| Set | `TreeSet` | `null`を要素として追加すると`NullPointerException` |
| Set | `ConcurrentSkipListSet` | `null`非対応（`Comparable`比較に失敗） |
| Map | `Hashtable` | `null`キー・値ともに不可（古い設計） |
| Map | `ConcurrentHashMap` | `null`キー・値ともに不可（非同期処理上の安全性のため） |
| Queue | `PriorityQueue` | `null`の挿入は`NullPointerException` |
| Queue | `ArrayDeque` | `null`の挿入で例外（`null`を特別な意味に使う設計のため） |
| List | `List.of(...)`（Java 9～） | 不変リスト。`null`を渡すと例外 |

---

### 補足：不変コレクション (`List.of`, `Set.of`, `Map.of`)

```java
List<String> list = List.of("a", null); // → NullPointerException
```

- Java 9以降の`of`メソッド系はすべて**`null`を許容しない**
- これは**不変性（immutable）**を保証するためと、予期しない`NullPointerException`の予防のためです

---

### まとめ：null許容の目安

| 判定基準 | null許容の傾向 |
| --- | --- |
| `new`で生成する通常のコレクション | 多くはnullを許容（ArrayList, HashMapなど） |
| 並行（concurrent）コレクション | null非許容が多い（ConcurrentHashMapなど） |
| ソートを伴う（Tree～） | `null`が比較できないためキー/値で例外の可能性 |
| `of()`などの不変API | `null`は原則許容しない |

---

必要であれば、各ケースの**コード例付き一覧**も作成できます。ご希望ですか？

# Q,

ArrayListなどのリスト生成でnullを扱えないものは何ですか？

なるほど、いい視点ですね 👍

Java の **コレクション（List系）** で「nullを要素として扱えるかどうか」について整理します。

---

## ✅ null を扱えるリスト

- **`ArrayList`**
    - 要素として `null` を追加可能。
    - `add(null)` も `set(i, null)` もOK。
    - `contains(null)` も正しく動作する。

```java
List<String> list = new ArrayList<>();
list.add(null);          // OK
System.out.println(list); // [null]
```

- **`LinkedList`**
    - `ArrayList` 同様に `null` を格納できる。
- **`Vector`**
    - 古いクラスだが、`null` を格納可能。
- **`CopyOnWriteArrayList`**
    - スレッドセーフなリスト。これも `null` を格納可能。

---

## ❌ null を扱えないリスト

- **`Stack` の一部メソッド**
    - `push(null)` はOKだが、`search(null)` は動作しない。
    - ただし「禁止」ではなく「結果が常に -1」。
- **`Arrays.asList(...)`**
    - `null` 自体は格納可能だが、`null` 配列を渡すと `NullPointerException`。

```java
List<String> list = Arrays.asList(null); // サイズ1の [null] ができる
String[] arr = null;
List<String> list2 = Arrays.asList(arr); // NPE!!
```

- **`List.of(...)` (Java 9以降)**
    - **null を要素にできない**
    - `List.of("a", null, "b");` → `NullPointerException`
    - **不変リスト**のため、`null` を禁止している。
- **`Collections.emptyList()` / `Collections.singletonList(x)`**
    - `singletonList(null)` はOK（[null] の不変リストが作れる）。
    - ただし `singletonList` 生成後に要素を `null` に差し替えることは不可（不変だから）。

---

## まとめ表

| クラス / メソッド | null要素 |
| --- | --- |
| `ArrayList` | ✅ 可 |
| `LinkedList` | ✅ 可 |
| `Vector` | ✅ 可 |
| `CopyOnWriteArrayList` | ✅ 可 |
| `Arrays.asList(...)` | ⚠️ 要注意（引数が null の場合は NPE） |
| `List.of(...)` (Java 9〜) | ❌ 不可（NPE） |
| `Collections.singletonList(null)` | ✅ 可（不変リスト [null]） |
| `Collections.emptyList()` | ❌ そもそも要素追加できない |

👉 Hiroakiさんの質問意図は「`ArrayList` はOKだけど、`List.of` はNG」という整理を確認したい感じでしょうか？