# Map

Javaのコレクションフレームワークにおける`Map`は、**キーと値（Key-Value）のペアを保持するデータ構造**です。リストやセットと異なり、要素に順序はなく、キーを使って値を検索・更新・削除します。

---

## 🔸 Mapの基本構造

```java
Map<Key型, Value型> map = new HashMap<>();
```

- `Key型`: 一意の識別子。重複できません。
- `Value型`: キーに対応する値。重複しても構いません。

例：

```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
scores.put("Bob", 85);
```

---

## 🔸 代表的な実装クラス

| 実装クラス | 特徴 |
| --- | --- |
| `HashMap` | 順序なし、高速アクセス（`null`キー1つ可）。最もよく使われる。 |
| `LinkedHashMap` | 挿入順を保持。アクセス順に並び替えも可能。 |
| `TreeMap` | キーの自然順序またはComparatorによる順序でソート。 |
| `Hashtable` | 古い同期付き実装。現在は`ConcurrentHashMap`を使う方が一般的。 |
| `ConcurrentHashMap` | スレッドセーフな高性能Map。並列環境に向いている。 |

---

## 🔸 主なメソッド

| メソッド | 説明 |
| --- | --- |
| `put(K key, V value)` | キーと値を追加。既存キーなら値を上書き。 |
| `get(Object key)` | 指定キーに対応する値を取得。 |
| `remove(Object key)` | 指定キーのペアを削除。 |
| `containsKey(Object key)` | キーが存在するか調べる。 |
| `containsValue(Object val)` | 値が存在するか調べる。 |
| `keySet()` | 全キーをSetとして取得。 |
| `values()` | 全値をCollectionとして取得。 |
| `entrySet()` | Map.Entry（キーと値のペア）のSetを取得。 |

---

## 🔸 簡単な使用例

```java
Map<String, String> capitalMap = new HashMap<>();
capitalMap.put("Japan", "Tokyo");
capitalMap.put("France", "Paris");

System.out.println(capitalMap.get("Japan")); // "Tokyo"
System.out.println(capitalMap.containsKey("France")); // true

for (Map.Entry<String, String> entry : capitalMap.entrySet()) {
    System.out.println(entry.getKey() + " → " + entry.getValue());
}
```

---

## 🔸 よくある注意点

| 注意点 | 内容 |
| --- | --- |
| キーの重複 | 同じキーに`put()`すると上書きされます。 |
| `null`の扱い | `HashMap`は`null`キーを1つ、`null`値を複数許可しますが、`TreeMap`はキーに`null`を許可しません。 |
| イテレーション時の変更 | ループ中に要素を追加・削除する場合は`ConcurrentHashMap`などを使う必要があります。 |

---

## 🔸 Mapが使われる代表的ケース

- ユーザIDとユーザ情報の対応付け
- 商品コードと在庫数の管理
- 設定キーとその値（構成ファイルの読み込みなど）
- キャッシュの実装

---

# Q,

Javaで宣言型を Map 型とした変数に複数要素を格納する方法として、HashMap、LinkedHasMap,TreeMap, Hashtable, ConcurrentHashMap　以外の方法はありますか？

はい、Javaで`Map`型の変数に複数要素を格納する方法として、代表的な5つの実装クラス（`HashMap`、`LinkedHashMap`、`TreeMap`、`Hashtable`、`ConcurrentHashMap`）以外にもいくつかの方法があります。以下に**追加の方法と使われるケース**を整理して説明します。

---

## ✅ 1. **匿名クラス・ラムダでの即時初期化**

Javaでは`Map`に複数の要素をまとめて初期化したい場合、**初期化ブロックを使った匿名クラス**の手法があります。

```java
Map<String, Integer> map = new HashMap<>() {{
    put("A", 1);
    put("B", 2);
}};
```

🔸ただし、この方法は**内部クラスを生成する副作用**があるため、**実務やパフォーマンス要件が高い場面では非推奨**です。

---

## ✅ 2. **Java 9以降の`Map.of()`と`Map.ofEntries()`（イミュータブル）**

Java 9から、**少数の要素を簡潔に初期化できるメソッド**が標準ライブラリに追加されました。

```java
Map<String, Integer> map1 = Map.of("A", 1, "B", 2); // 最大10組まで

Map<String, Integer> map2 = Map.ofEntries(
    Map.entry("X", 10),
    Map.entry("Y", 20)
);
```

🟡 特徴：

- **イミュータブル（不変）Map**
- `null`キー・値は**許容されない**
- 要素数は`Map.of()`で最大10組、それ以上は`Map.ofEntries()`を使用

---

## ✅ 3. **`Collectors.toMap()`を使ったStream初期化**

Stream APIで、例えば配列やリストからMapを生成できます。

```java
List<String> names = List.of("Alice", "Bob", "Charlie");

Map<String, Integer> nameLengthMap = names.stream()
    .collect(Collectors.toMap(
        name -> name,         // key
        name -> name.length() // value
    ));
```

🔸動的なデータに基づいてMapを構築したいときに便利です。

---

## ✅ 4. **Guavaの`ImmutableMap`（Google製ライブラリ）**

Googleのライブラリ [Guava](https://github.com/google/guava) を使うと、簡潔で高速なイミュータブルMapが作成可能です。

```java
Map<String, Integer> map = ImmutableMap.of(
    "One", 1,
    "Two", 2
);
```

🟡 特徴：

- イミュータブル
- `null`禁止
- Java標準より高機能（特に多重Map系など）

---

## ✅ 5. **Apache Commons Collectionsの`MapUtils`**

Apache Commonsライブラリのユーティリティクラスを使えば、Mapの作成や変換が簡潔になります。

```java
Map<String, String> map = MapUtils.putAll(
    new HashMap<>(), 
    new String[][] {
        {"A", "Apple"},
        {"B", "Banana"}
    }
);
```

---

## ✅ 6. **EnumMap（列挙型のキー専用）**

列挙型をキーとして使うなら`EnumMap`が非常に高速です。

```java
enum Day { MON, TUE, WED }

Map<Day, String> schedule = new EnumMap<>(Day.class);
schedule.put(Day.MON, "Work");
schedule.put(Day.TUE, "Meeting");
```

🟢 特徴：

- キーに`enum`限定
- 高速でメモリ効率が良い

---

## ✅ 7. **Properties（Mapの一種）**

設定ファイルや環境変数などで使用される`Properties`は、`Map<Object, Object>`のサブクラスです。

```java
Properties props = new Properties();
props.setProperty("url", "https://example.com");
```

---

## ✅ まとめ表

| 方法 | 可変 | 特徴 |
| --- | --- | --- |
| `HashMap`など5種 | ✅ | 標準・高速・用途別 |
| `Map.of()` / `ofEntries()` | ❌ | イミュータブル・簡易初期化 |
| 匿名クラス初期化 | ✅ | 簡単だが非推奨 |
| `Collectors.toMap()` | ✅ | Streamとの組み合わせで柔軟 |
| `Guava ImmutableMap` | ❌ | 強力な不変Map（外部ライブラリ） |
| `EnumMap` | ✅ | enum専用で効率的 |
| `Properties` | ✅ | キー・値が文字列に限定される |

以下に Java Silver 試験を意識した **Mapに関するパターン問題（選択肢トラップあり）** を提示します。

出題傾向を踏まえ、`HashMap` を中心に `null`の扱いやキー重複、`get()`の戻り値なども含めています。

---

## ✅ 問題①：`put()` と `get()` の基本動作

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("A", "Apple");
        map.put("B", "Banana");
        map.put("A", "Apricot");

        System.out.println(map.get("A"));
    }
}
```

### Q. 実行結果は？

A. `Apple`

B. `Banana`

C. `Apricot`

D. `null`

---

### ✅ 解説：

- 同じキー `"A"` に対して `put()` を2回行っており、後の `"Apricot"` で上書きされる。
- `get("A")` は `"Apricot"` を返す。

✅ **正解：C**

---

## ✅ 問題②：`null`キー・値の扱い

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put(null, null);
        map.put("X", "x-ray");

        System.out.println(map.get(null));
    }
}
```

### Q. 実行結果は？

A. コンパイルエラー

B. 実行時例外

C. `"null"`（文字列）

D. `null`（値）

---

### ✅ 解説：

- `HashMap` は **nullキーを1つだけ許容**する。
- `null` 値も許容される。
- `System.out.println()` で `null` を出力すると、何も表示されずに `null` と表示される。

✅ **正解：D**

---

## ✅ 問題③：イテレーション処理（entrySet）

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("A", 10);
        map.put("B", 20);

        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.print(entry.getKey() + entry.getValue());
        }
    }
}
```

### Q. 出力結果の候補として**正しいものは？**

A. `A10B20`

B. `B20A10`

C. 出力順はランダムである

D. コンパイルエラーになる

---

### ✅ 解説：

- `HashMap` は **順序保証がない**。
- `entrySet()` を使うと順不同で出力される。
- AやBの順序は保証されないため、**固定出力に期待してはいけない**。

✅ **正解：C**

---

## ✅ 問題④：キーが存在しない場合の `get()` の戻り値

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("X", "x-ray");

        System.out.println(map.get("Y"));
    }
}
```

### Q. 実行結果は？

A. `"null"`（文字列）

B. `null`（値）

C. コンパイルエラー

D. 実行時例外（NullPointerException）

---

### ✅ 解説：

- 存在しないキー `"Y"` を指定すると、戻り値は `null`
- `null` は表示されるが文字列ではなく値としての `null`

✅ **正解：B**

---

## ✅ 問題⑤：`containsKey()` の使い方

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("A", "Apple");

        if (map.containsKey("A")) {
            System.out.println("Found");
        } else {
            System.out.println("Not Found");
        }
    }
}
```

### Q. 実行結果は？

A. `Found`

B. `Not Found`

C. コンパイルエラー

D. 実行時例外

---

### ✅ 解説：

- `containsKey("A")` は `"A"` が存在するかどうかを調べる
- 登録済みなので `"Found"` と表示される

✅ **正解：A**

---

## ご希望に応じて：

- Goldレベルの応用問題（`TreeMap`, `Map.of`, `groupingBy` など）
- `Map`を使ったラムダ＋Stream演習問題
- 実務的なMapの使い分け問題（LinkedHashMap vs TreeMap）

以下は Java Gold レベルを意識した **Map の応用パターン問題集**です。

特徴として、**順序性・null制約・イミュータブル・Streamとの連携・Comparatorとの併用**など、**実務的かつ出題トラップが多い観点**を含めています。

---

## ✅ 問題①：`Map.of()` の制約

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = Map.of("A", "Apple", "B", null);
        System.out.println(map.get("B"));
    }
}
```

### Q. 実行結果として正しいものは？

A. `null` が出力される

B. `null` と表示されるが実は文字列

C. `NullPointerException` がスローされる

D. コンパイルエラー

---

### ✅ 解説：

- `Map.of()` は Java 9 で導入された **イミュータブルな Map** を生成するファクトリーメソッド。
- **`null` のキーも値も禁止**。
- `"B", null` により `NullPointerException` が発生。

✅ **正解：C**

---

## ✅ 問題②：`TreeMap` と `Comparator` の併用

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new TreeMap<>(Comparator.reverseOrder());
        map.put("B", 2);
        map.put("A", 1);
        map.put("C", 3);

        map.keySet().forEach(System.out::print);
    }
}
```

### Q. 出力結果として正しいものは？

A. ABC

B. ACB

C. CBA

D. 出力順はランダムである

---

### ✅ 解説：

- `TreeMap` はキーの **順序付きMap**。
- `Comparator.reverseOrder()` によってキーが降順にソートされる。
- C → B → A の順に出力される。

✅ **正解：C**

---

## ✅ 問題③：`Collectors.toMap()` と重複キー

```java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "apricot");

        Map<Character, String> map = list.stream()
            .collect(Collectors.toMap(
                s -> s.charAt(0),
                s -> s
            ));

        System.out.println(map);
    }
}
```

### Q. 実行時に発生する可能性がある問題は？

A. すべてのキーが異なるため正常動作する

B. 同一キーが存在しないため問題ない

C. 重複キーによって `IllegalStateException` が発生する

D. `NullPointerException` が発生する

---

### ✅ 解説：

- `"apple"` と `"apricot"` は両方 `charAt(0) == 'a'` → 同じキー `a` になる。
- `toMap()` はデフォルトでキーの重複を許容しない。
- 明示的にマージ関数を指定しないと `IllegalStateException` がスローされる。

✅ **正解：C**

---

## ✅ 問題④：`Map.entrySet()` の操作と同時削除

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("A", 1);
        map.put("B", 2);

        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            if (entry.getKey().equals("A")) {
                map.remove("A");
            }
        }

        System.out.println(map);
    }
}
```

### Q. 上記のコードの実行結果は？

A. `{B=2}` と出力される

B. 実行時例外がスローされる

C. `{A=1, B=2}` とそのまま出力される

D. コンパイルエラーになる

---

### ✅ 解説：

- `HashMap` の `entrySet()` に対するループ中に `map.remove()` で要素を削除すると、**ConcurrentModificationException** が発生する可能性がある。
- 安全な削除は **`Iterator.remove()`** を使うべき。

✅ **正解：B**

---

## ✅ 問題⑤：`LinkedHashMap` と挿入順

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new LinkedHashMap<>();
        map.put("X", 100);
        map.put("Y", 200);
        map.put("Z", 300);

        map.forEach((k, v) -> System.out.print(k));
    }
}
```

### Q. 出力結果として正しいものは？

A. XYZ

B. ZYX

C. XZY

D. 順序は保証されない

---

### ✅ 解説：

- `LinkedHashMap` は **挿入順を保持するMap**。
- 登録順に `"X", "Y", "Z"` → `XYZ` が出力される。

✅ **正解：A**

---

## ご希望に応じて…

- `groupingBy`, `partitioningBy`, `merge()` などの実務応用編
- Map の代替設計（List<Map<>>, Map<String, List<>>）パターン
- イミュータブル vs ミュータブルの挙動比較問題

Q,