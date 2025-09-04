# Map メソッド

# Q,

Mapインタフェースの主要メソッドについてまとめてください。

了解しました、Hiroakiさん！

以下に、Javaの `Map` インタフェース（`java.util.Map`）の**主要メソッド**を、**よく使う順＋用途別にわかりやすくまとめ**ます。

---

## ✅ Java `Map` インタフェースの主要メソッド一覧

| メソッド名 | 戻り値 | 概要 |
| --- | --- | --- |
| `V put(K key, V value)` | `V` | 指定したキーに値を追加（上書き） |
| `V get(Object key)` | `V` | 指定したキーに対応する値を取得（なければ `null`） |
| `V remove(Object key)` | `V` | 指定したキーのエントリを削除 |
| `boolean containsKey(Object key)` | `boolean` | 指定キーが存在するか判定 |
| `boolean containsValue(Object value)` | `boolean` | 指定値が存在するか判定 |
| `Set<K> keySet()` | `Set<K>` | 全キーの集合を取得 |
| `Collection<V> values()` | `Collection<V>` | 全値の集合を取得 |
| `Set<Map.Entry<K, V>> entrySet()` | `Set<Map.Entry>` | 全エントリ（キーと値の組）を取得 |
| `int size()` | `int` | 要素数（エントリ数）を取得 |
| `boolean isEmpty()` | `boolean` | 空かどうかを判定 |
| `void clear()` | `void` | 全エントリを削除 |
| `V putIfAbsent(K key, V value)` | `V` | 指定キーが未登録なら追加（存在すれば何もしない） |
| `V getOrDefault(Object key, V defaultValue)` | `V` | 指定キーの値がなければデフォルト値を返す |
| `void forEach(BiConsumer<K,V> action)` | `void` | 各キーと値に対して処理を実行（ラムダ式対応） |
| `void replaceAll(BiFunction<K,V,V> function)` | `void` | 各エントリの値を一括で更新 |

---

## 📌 よく使うコード例

```java
Map<String, Integer> map = new HashMap<>();

map.put("apple", 100);
map.put("banana", 80);

System.out.println(map.get("apple")); // → 100
System.out.println(map.containsKey("banana")); // → true

for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

---

## 🧠 Java 8 以降の便利メソッド（ラムダ対応）

| メソッド | 使い方例 |
| --- | --- |
| `forEach` | `map.forEach((k, v) -> System.out.println(k + "=" + v));` |
| `getOrDefault` | `map.getOrDefault("key", 0);` |
| `putIfAbsent` | `map.putIfAbsent("key", 1);` |
| `replaceAll` | `map.replaceAll((k, v) -> v * 2);` |

---

## ⚠️ 注意ポイント

- `null` キー・値は `HashMap` は許容しますが、`TreeMap` や `Hashtable` はNG（または制限あり）。
- キー重複は禁止（`put` で上書きされる）。
- `Map` はインタフェースであり、主な実装は次のようになります：

---

## ✅ 主な `Map` 実装クラス

| 実装クラス | 特徴 |
| --- | --- |
| `HashMap` | 最も基本、順序保証なし、高速 |
| `LinkedHashMap` | 挿入順を保持 |
| `TreeMap` | キー順（ソート順）に保持（要 `Comparable` or `Comparator`） |
| `ConcurrentHashMap` | スレッドセーフなマップ |
| `EnumMap` | 列挙型をキーに使う軽量マップ |

---

必要であれば、これを「試験対策用チェックリスト」「実装クラス別比較表」や「使い方チートシート」として再構成することも可能です。ご希望ありますか？

[Collection<V> values()](Map%20%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%2023362cef7350804cbe8af2a54ba901b8/Collection%20V%20values()%2023362cef73508017b4fddf247cb23311.md)