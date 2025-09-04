# Comparable と Comparator

# Q,

Comparableインターフェイスには compareToメソッドしか定義されていない一方で、Comparatorインターフェイスには compareメソッド以外に多くのメソッドが定義されています。
この違いは、なぜでしょうか？
歴史的にComparatorが後から追加されたような背景がありますか？

とても本質的なご質問です、Hiroakiさん。

この疑問は「**Javaにおける比較機構の進化**」に深く関わるテーマで、`Comparable` と `Comparator` の役割の違いとその進化の背景を知ると、全体像が明確になります。

---

## ✅ 結論：

`Comparable` は **Javaの初期（JDK 1.2）からある基本インターフェース**で、

`Comparator` は後から拡張され、特に **Java 8 で「関数型インターフェース」として機能強化された**という背景があります。

---

## ✅ 1. `Comparable` と `Comparator` の設計上の違い

| 観点 | `Comparable<T>` | `Comparator<T>` |
| --- | --- | --- |
| 実装場所 | **クラス自身**（内在的順序） | **外部定義**（柔軟な順序） |
| メソッド | `int compareTo(T o)` のみ | `int compare(T o1, T o2)` ほか多数（Java 8以降） |
| 役割 | そのクラスの「**自然順序**」を定義 | さまざまな順序付けを**柔軟に定義・注入**できる |
| 実装例 | `String`, `Integer`, `LocalDate` | `Comparator.comparing()`, `reversed()` など |

---

## ✅ 2. なぜ `Comparable` は `compareTo()` しか持たないのか？

- `Comparable` は **古い（JDK 1.2）**インターフェースであり、
    
    シンプルに「**このクラスが自分で順序を知っている**」という設計思想に基づいています。
    

```java
public interface Comparable<T> {
    int compareTo(T o);
}
```

- 1つの「自然順序」しか定義できない → 拡張性が低いが、**単純な並び替えには十分**

---

## ✅ 3. なぜ `Comparator` はメソッドが多いのか？（Java 8以降）

- `Comparator` は **Java 8 で関数型インターフェース化**され、
    
    `ラムダ式と一緒に使うためのユーティリティメソッド` が多数追加されました。
    

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);

    // Java 8で追加されたデフォルト・静的メソッド
    default Comparator<T> reversed()
    default Comparator<T> thenComparing(...)
    static <T, U> Comparator<T> comparing(Function<T, U> keyExtractor)
}
```

### ▶ 機能拡張の背景：

- **ラムダ式とStream APIの普及**によって、比較操作の柔軟性が求められるようになった
- `Comparator` は**比較ロジックをオブジェクトとして扱える**ので、**関数合成に適している**

---

## ✅ 4. 時系列で見る進化（歴史的背景）

| Javaバージョン | 追加・強化された内容 |
| --- | --- |
| JDK 1.2 | `Comparable<T>`, `Comparator<T>` の基本定義 |
| JDK 8 | `Comparator` に `default` メソッド追加（reversed, thenComparing, comparing） |
| JDK 14+ | `record` により、簡易に `Comparable` 実装可 |

---

## ✅ 5. 構文面からの比較（用途に応じた選択）

### ◼ Comparable（クラス自身に自然順序を定義）

```java
class User implements Comparable<User> {
    String name;
    public int compareTo(User other) {
        return this.name.compareTo(other.name);
    }
}
```

### ◼ Comparator（外部から注入）

```java
Comparator<User> byName = Comparator.comparing(u -> u.name);
List<User> list = ...
list.sort(byName);
```

➡ 複数の順序を切り替えるなら **Comparator の方が柔軟で実務向き**

---

## ✅ まとめ

| 項目 | `Comparable` | `Comparator` |
| --- | --- | --- |
| 歴史 | 古い（JDK 1.2） | 後発、Java 8で大きく拡張 |
| 主な用途 | 自然順序の定義 | 外部からの柔軟な順序付け |
| 拡張性 | 低い（1通りのみ） | 高い（複数定義、合成、逆順、null対応） |
| メソッド構成 | 1つだけ（compareTo） | compareに加え、多数のユーティリティあり |
| モダンな使用（Stream） | ✕ | ◎（Stream.sortedなどと好相性） |