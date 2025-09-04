# Set

ここでは Java のコレクションフレームワークの1つである **`Set` インターフェース** について、**構造・用途・実装クラスの違い・注意点**を整理して解説します。

---

## ✅ 1. `Set`とは？

- `Set` はコレクションの一種で、**重複を許さない要素の集まり**を表します。
- 順序やインデックスによるアクセスは保証されません（一部の実装を除く）。

```java
Set<String> set = new HashSet<>();
set.add("A");
set.add("B");
set.add("A"); // 無視される（重複）
System.out.println(set); // [A, B]（順序保証なし）
```

---

## ✅ 2. 主な特徴

| 特性 | 内容 |
| --- | --- |
| 重複不可 | 同じ値の要素を複数回追加しても1回のみ保存 |
| nullの扱い | 実装によるが、`HashSet`は1つだけ追加可能 |
| 順序保持 | 基本なし（`HashSet`）／順序保持（`LinkedHashSet`）／ソート（`TreeSet`） |
| equalsとhashCode | 重複判定に使用される（特に`HashSet`） |

---

## ✅ 3. 主な実装クラスの違い

| 実装クラス | 順序 | ソート | 特徴 |
| --- | --- | --- | --- |
| `HashSet` | ✕ | ✕ | 最も一般的で高速（内部はハッシュテーブル） |
| `LinkedHashSet` | ◯ | ✕ | 追加順を保持する `HashSet` の派生 |
| `TreeSet` | ◯ | ◯ | 自然順 or `Comparator` によるソート |

HashSet

```java
Set<String> set = new HashSet<>();
set.add("apple");
set.add("banana");
set.add("apple"); // 無視される
System.out.println(set); // 順序は保証されないｖ
```

LinkedHashSet

```java
Set<String> set = new LinkedHashSet<>();
set.add("apple");
set.add("banana");
System.out.println(set); // [apple, banana]
```

TreeSet

```java
Set<String> set = new TreeSet<>();
set.add("banana");
set.add("apple");
System.out.println(set); // [apple, banana]（ソート済）
```

---

## ✅ 4. よく使うメソッド（`Set`インターフェース）

| メソッド | 説明 |
| --- | --- |
| `add(E e)` | 要素を追加（重複は無視） |
| `remove(Object o)` | 要素を削除 |
| `contains(Object o)` | 含まれているかをチェック |
| `isEmpty()` | 空かどうか |
| `size()` | 要素数 |
| `clear()` | 全削除 |
| `forEach(...)` | 全要素に対する処理 |

---

## ✅ 5. `List`との違い（試験でも重要）

| 比較項目 | `List` | `Set` |
| --- | --- | --- |
| 重複 | 許容する | 許容しない |
| 順序 | 保持する（インデックスあり） | 保持しない（TreeSet以外） |
| null | 複数可 | 実装次第（HashSetは1つまで） |
| 検索コスト | `O(n)`（順次探索） | `HashSet`: `O(1)` に近い |

---

## ✅ 6. equalsとhashCodeの重要性（特にHashSet）

`Set`に要素を追加するとき、**`equals()`と`hashCode()`の実装が正しくないと重複判定が誤る**ことがあります。

```java
class User {
    String name;
    public User(String name) {
        this.name = name;
    }
}

Set<User> set = new HashSet<>();
set.add(new User("Alice"));
set.add(new User("Alice")); // 別オブジェクト。重複扱いされない！

System.out.println(set.size()); // 2 ←重複扱いされてない
```

➡ 対策：`equals()`と`hashCode()`を適切にオーバーライドすること！

---

## ✅ 7. Setが使われる実務シーン

| シーン | 理由 |
| --- | --- |
| IDやユニークな値の管理 | 重複を自動で排除できるため |
| タグ・カテゴリの管理 | 順序不要、重複NGなケースに最適 |
| 検索高速化（`contains()`） | `HashSet`は検索が高速 |
| データ差分の計算（交差・和） | `retainAll()`, `removeAll()` など集合演算が便利 |

---

## ✅ 8. 注意点・落とし穴

| ケース | 説明 |
| --- | --- |
| `add()`しても増えない | `equals()`と`hashCode()`の問題がないか確認 |
| `Set.of(...)`は変更不可 | `add()`や`remove()`で例外が出る（Java 9以降） |
| `TreeSet`ではnullが禁止 | `NullPointerException`が発生する（順序付けできない） |

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 主な特徴 | 順序なし、重複なし、検索が高速 |
| 代表的な実装 | `HashSet`, `LinkedHashSet`, `TreeSet` |
| 使い所 | 重複排除、検索効率化、順序不要なユニーク値の保持 |
| 実装上の注意点 | `equals()`と`hashCode()`を正しく実装する |

# Q,

Javaで宣言型を Set 型（Set<T>）とした変数に複数要素を格納する方法として、HashSet、LinkedHasSet,TreeSet　以外の方法はありますか？

はい、あります。

Hiroakiさんの質問は、「**宣言型を `Set<T>` とした変数に、複数要素を格納する方法**」として、

`HashSet` / `LinkedHashSet` / `TreeSet` 以外にどんな選択肢があるか？という問いですね。

ここでは **実装クラス・ファクトリメソッド・ライブラリ系** も含めて、整理してご紹介します。

---

## ✅ 1. 結論：他にも複数あります（主に以下）

| 分類 | 実装・手段 | 特徴 |
| --- | --- | --- |
| ✅ 不変Set | `Set.of(...)`（Java 9+） | 変更不可・重複不可 |
| ✅ コピー系Set | `Set.copyOf(...)`（Java 10+） | 元のCollectionから不変Setを作る |
| ✅ 同期版Set | `Collections.synchronizedSet(...)` | スレッドセーフ |
| ✅ 並列Set（特殊用途） | `ConcurrentHashMap.newKeySet()` | 高速で並列安全なSet |
| ✅ GuavaのImmutableSet | `ImmutableSet.of(...)` | 高機能・null禁止・不変 |
| ✅ EnumSet | `EnumSet.of(...)` | 列挙型専用で高速 |

---

## 🔷 2. それぞれの使い方と特徴

---

### ◼ Set.of(...)（Java 9以降）

```java
Set<String> set = Set.of("A", "B", "C");
```

- **不変（immutable）**
- 重複要素や `null` を渡すと **例外が発生**
- 軽量で高速な読み取り専用Set

---

### ◼ Set.copyOf(...)（Java 10以降）

```java
List<String> list = List.of("A", "B", "C");
Set<String> set = Set.copyOf(list);
```

- 引数のコレクションの内容を**変更不可なSetに変換**
- `Set.of()`との違い → 元が `Collection` であることが前提

---

### ◼ Collections.synchronizedSet(...)

```java
Set<String> set = Collections.synchronizedSet(new HashSet<>());
```

- **スレッドセーフなラッパー**
- 内部は通常の `Set` だが、全メソッドに `synchronized` を付ける

---

### ◼ ConcurrentHashMap.newKeySet()

```java
Set<String> set = ConcurrentHashMap.newKeySet();
set.add("A");
```

- **高並列性能**
- Javaのマルチスレッド環境向け（Webサーバーのキャッシュ管理など）

---

### ◼ Guava: ImmutableSet.of(...)

```java
Set<String> set = ImmutableSet.of("A", "B", "C"); // Google Guava
```

- Java標準の `Set.of()` より前から存在
- **null禁止**, 高機能（ビルダーやコピーなども充実）

---

### ◼ EnumSet.of(...)（列挙型専用）

```java
enum Color { RED, GREEN, BLUE }

Set<Color> set = EnumSet.of(Color.RED, Color.BLUE);
```

- 列挙型専用で、**メモリ効率・実行速度が非常に優秀**
- すべての列挙値を持つ `EnumSet.allOf(Color.class)` もある

---

## ✅ 3. 全体比較表（用途別）

| 実装 | ミュータブル | 順序保証 | null許容 | 重複許容 | 備考 |
| --- | --- | --- | --- | --- | --- |
| `HashSet` | ✔ | ✕ | ✔(1つ) | ✕ | 最も基本的なSet |
| `LinkedHashSet` | ✔ | ✔（追加順） | ✔(1つ) | ✕ | 順序が必要な場合に |
| `TreeSet` | ✔ | ✔（ソート） | ✕ | ✕ | Comparable必須 |
| `Set.of(...)` | ✕ | 実装依存 | ✕ | ✕ | 不変、null/重複禁止 |
| `Set.copyOf(...)` | ✕ | 実装依存 | ✕ | ✕ | 不変、null/重複禁止 |
| `Collections.synchronizedSet(...)` | ✔ | 実装依存 | ✔ | ✕ | 同期ラッパー |
| `ConcurrentHashMap.newKeySet()` | ✔ | ✕ | ✔ | ✕ | 高速スレッドセーフ |
| `ImmutableSet.of(...)` (Guava) | ✕ | 実装依存 | ✕ | ✕ | 不変＋高機能（null禁止） |
| `EnumSet.of(...)` | ✔ | ソート済 | ✕ | ✕ | 列挙型専用、軽量高速 |

---

## ✅ 4. まとめ

> 「宣言型が Set<T>」であれば、どの実装・生成方法でも代入可能
> 
> 
> 重要なのは、**生成方法が何を意図しているのか（不変か／順序か／スレッドセーフか）**です。
> 

ここでは Java Silver レベルに対応した、**`Set` に関する基本パターン問題**を出題します。

Silver試験では主に `HashSet` を中心とした「重複禁止」「順序非保証」「基本APIの理解」などが問われやすいです。

---

## 🧠 Set の基本パターン問題（Java Silver風）

---

### 【問題1】重複の扱い

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("A");
        set.add("B");
        set.add("A");
        System.out.println(set.size());
    }
}
```

### 問：

出力として正しいものは？

A. 1

B. 2

C. 3

D. 実行時エラーが発生する

---

### 【問題1】正解：**B**

- `Set`は重複を許さない → `"A"` は2回追加しても1回だけ保存される
    
    → 要素数は `"A", "B"` の2つ
    
    ---
    

### 【問題2】順序保証の確認

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("B");
        set.add("A");
        set.add("C");
        System.out.println(set);
    }
}
```

### 問：

このコードの出力結果はどうなるか？

A. [B, A, C]

B. [A, B, C]

C. [C, B, A]

D. 実行するたびに順序が変わる可能性がある

---

### 【問題2】正解：**D**

- `HashSet` は順序を保証しない（追加順でも辞書順でもない）
- 実行ごとに `[C, A, B]` や `[A, C, B]` など変わる可能性あり

---

### 【問題3】nullの扱い

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add(null);
        set.add(null);
        System.out.println(set.size());
    }
}
```

### 問：

このコードの出力結果は？

A. 0

B. 1

C. 2

D. コンパイルエラーになる

---

### 【問題3】正解：**B**

- `HashSet` は `null` を1つだけ保持可能
- 複数回追加しても1つとしてカウントされる

---

### 【問題4】containsの判定基準

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("Hello");
        System.out.println(set.contains(new String("Hello")));
    }
}
```

### 問：

このコードの出力結果として正しいものは？

A. false（参照が異なるから）

B. true（値が同じなら同一と判定される）

C. コンパイルエラー

D. 実行時エラー

---

### 【問題4】正解：**B**

- `contains()` は `equals()` による内容比較
- `"Hello".equals(new String("Hello"))` は `true`

---

### 【問題5】Set操作の基本API

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<Integer> set = new HashSet<>();
        set.add(1);
        set.add(2);
        set.remove(2);
        System.out.println(set.contains(2));
    }
}
```

### 問：

このコードの出力結果は？

A. true

B. false

C. 2

D. コンパイルエラー

---

### 【問題5】正解：**B**

- `remove(2)` により `2` は削除済み
- `contains(2)` → `false`

---

## ✅ よくある Silver 引っかけポイント（Set編）

| トラップ内容 | 解説 |
| --- | --- |
| 重複を入れてもsizeが増えない | `Set`の基本動作（addしても既存なら無視） |
| 順序保証と思い込む | `HashSet`は順序非保証、`LinkedHashSet`は順序保証 |
| `contains()`は参照でなく値で判定 | `equals()` による比較 |
| `null`を複数入れても1つだけ | `Set`はnullを1つだけ保持可（HashSet） |

# Q,

Linkのremove は その引数がint だとインデックス削除でしたが、Set の場合は、要素そのものの削除になる。
これは、Setが重複を許さない要素の集まりであること、更に基本順序を考慮しないことから、そもそもインデックス管理する構造にないからという理解は合っていますか？

はい、Hiroakiさんのご認識は**完全に正しい**です。

その理解をもう少し深堀りして整理すると、以下のようになります。

---

## ✅ `List` と `Set` の `remove()` の違い

| コレクション型 | メソッド呼び出し例 | 意味 |
| --- | --- | --- |
| `List` | `list.remove(1)` | インデックス `1` の要素を削除（int型） |
| `Set` | `set.remove(1)` | 要素 `1` を削除（オブジェクトとして処理） |

---

## ✅ なぜ `Set` はインデックスで削除できないのか？

### 🔹 理由1：**重複を許さない「値ベース」の構造**

- `Set` の目的は「ある値が**一意に存在するか**」にフォーカスするため、**インデックスは無関係**
- 値そのものに意味がある ⇒ `remove(Object o)` の形式が自然

---

### 🔹 理由2：**順序保証がない（インデックスが無意味）**

- `HashSet` や `TreeSet` は **要素の位置や順番を保証しない**
- インデックスを持たせる構造がそもそも存在しないため、`remove(int index)` という概念がない

---

### 🔹 補足：`List` には両方ある

- `list.remove(int index)` ← インデックス削除
- `list.remove(Object o)` ← 要素削除

この2つが**オーバーロード**されているため、`Integer` 型のリテラルを渡すと意図しない呼び分けが起こることもあります（Silverでよく出るトラップ）。

```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.remove(1); // インデックス削除（IndexOutOfBoundsになるかも）

list.remove(Integer.valueOf(1)); // 値の削除
```

---

## ✅ `Set` にインデックスがない構造的根拠

`Set`（特に `HashSet`）の内部構造は：

```java
内部的には HashMap の keySet を使用
→ key のハッシュ値により格納場所を決定
→ インデックスではなくハッシュベースで管理
```

つまり、「位置」という概念がないため、インデックス操作は論理的にも実装的にも意味を持ちません。

---

## ✅ まとめ

| 観点 | Hiroakiさんの理解 | 補足・評価 |
| --- | --- | --- |
| `Set.remove(int)`がない理由 | ✅ 正解 | `Set`は値ベースの集合構造で、順序を持たずインデックス管理されていないため |
| `Set.remove(Object)`になる理由 | ✅ 正解 | 「どの位置にあるか」ではなく「その値が存在するか」で判断するため |

ここでは Java Gold レベルで頻出する **`Set` + `equals()`/`hashCode()` に関するパターン問題**を出題します。

---

## 🎯 Set Goldレベル問題：equals/hashCode 編

### ✅ 試験の観点

Java Goldでは、`Set` の「重複判定」が `equals()` と `hashCode()` によって行われることを前提に、

**・オブジェクトの追加結果
・Setのサイズ
・toString出力** などを通じて
その理解を問う問題が出されます。

---

### 【問題1】equals/hashCode未定義の影響

```java
import java.util.*;

class User {
    String name;
    User(String name) {
        this.name = name;
    }
}

public class Main {
    public static void main(String[] args) {
        Set<User> users = new HashSet<>();
        users.add(new User("Hiroaki"));
        users.add(new User("Hiroaki"));
        System.out.println(users.size());
    }
}
```

### 問：

このコードの出力は？

A. 1

B. 2

C. コンパイルエラー

D. 実行時例外

---

### 【問題1】正解：**B**

- `User` は `equals()` も `hashCode()` もオーバーライドしていない
- **異なるインスタンスは異なると判定され、両方追加される**

---

### 【問題2】equalsだけオーバーライド

```java
import java.util.*;

class User {
    String name;
    User(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof User)) return false;
        return this.name.equals(((User)obj).name);
    }
}

public class Main {
    public static void main(String[] args) {
        Set<User> users = new HashSet<>();
        users.add(new User("Hiroaki"));
        users.add(new User("Hiroaki"));
        System.out.println(users.size());
    }
}
```

### 問：

このコードの出力は？

A. 1

B. 2

C. コンパイルエラー

D. 実行時例外

---

### 【問題2】正解：**B**

- `equals()` だけをオーバーライドしても、`hashCode()` が異なると別物として扱われる
- **HashSetの内部構造は、まず hashCode() でバケットを振り分けてから equals() で比較する**

---

### 【問題3】equalsとhashCodeの正しいセット

```java
import java.util.*;

class User {
    String name;
    User(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof User)) return false;
        return this.name.equals(((User)obj).name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}

public class Main {
    public static void main(String[] args) {
        Set<User> users = new HashSet<>();
        users.add(new User("Hiroaki"));
        users.add(new User("Hiroaki"));
        System.out.println(users.size());
    }
}
```

### 問：

このコードの出力は？

A. 1

B. 2

C. コンパイルエラー

D. 実行時例外

---

### 【問題3】正解：**A**

- `equals()` と `hashCode()` を適切にオーバーライド → 値が同一と判定され、**2つ目の要素は追加されない**

---

### 【問題4】TreeSetとComparable未実装

```java
import java.util.*;

class User {
    String name;
    User(String name) {
        this.name = name;
    }
}

public class Main {
    public static void main(String[] args) {
        Set<User> users = new TreeSet<>();
        users.add(new User("Hiroaki"));
    }
}
```

### 問：

このコードを実行するとどうなるか？

A. 実行時にClassCastExceptionが発生する

B. 正常終了して要素が追加される

C. コンパイルエラーになる

D. 実行時にNullPointerExceptionが発生する

---

### 【問題4】正解：**A**

- `TreeSet` はソートのために `Comparable` 実装 or `Comparator` 提供が必要
- ソートできないオブジェクトを追加すると **実行時に `ClassCastException` が発生**

---

## ✅ ゴールドで押さえるべきSet × equals/hashCodeポイント

| 観点 | 解説 |
| --- | --- |
| HashSet の重複判定基準 | ① hashCode() ② equals() の順 |
| equals() だけ定義は危険 | hashCode() が異なれば別バケットに入るため意味がない |
| TreeSet の重複判定 | compareTo() または Comparator の比較結果が 0 かどうか |
| equals/hashCode を定義しない場合 | デフォルト（Object同等：参照同一）になるため常に重複扱いされない |

ここでは Java Gold レベルに対応した「**Comparatorを使った TreeSet の応用パターン問題**」を出題します。

---

## 🎯 TreeSet × Comparator 応用パターン問題（Java Gold風）

### ✅ テーマ：

- `TreeSet` は要素をソートするために `Comparable` もしくは `Comparator` を要求
- 重複排除の判定も **equalsではなく compareTo / compare の戻り値が 0 かどうか** によって行われる
- よって、「`equals` は異なるが `compare()` は0」なケースが引っかけポイントになります

---

### 【問題1】Comparatorによる重複判定のすり替え

```java
import java.util.*;

class User {
    String name;
    int age;
    User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
        return name + ":" + age;
    }
}

public class Main {
    public static void main(String[] args) {
        Set<User> set = new TreeSet<>(Comparator.comparing(user -> user.name));
        set.add(new User("Hiroaki", 30));
        set.add(new User("Hiroaki", 35));
        System.out.println(set);
    }
}
```

### 問：

このコードを実行したとき、出力として正しいものは？

A. [Hiroaki:30, Hiroaki:35]

B. [Hiroaki:35]

C. [Hiroaki:30]

D. 例外が発生する

---

### 【問題1】正解：**C**

- `Comparator.comparing(user -> user.name)` により、`name` が等しい場合は **比較結果が0**
- つまり、2つのUserは重複と見なされ、後から入れた方は **無視される**
- 残るのは `"Hiroaki:30"`（先に追加された方）

---

### 【問題2】Comparatorの多段比較

```java
import java.util.*;

class User {
    String name;
    int age;
    User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
        return name + ":" + age;
    }
}

public class Main {
    public static void main(String[] args) {
        Comparator<User> cmp = Comparator
            .comparing((User u) -> u.name)
            .thenComparingInt(u -> u.age);

        Set<User> set = new TreeSet<>(cmp);
        set.add(new User("Hiroaki", 30));
        set.add(new User("Hiroaki", 35));
        System.out.println(set);
    }
}
```

### 問：

このコードを実行したとき、出力として正しいものは？

A. [Hiroaki:35]

B. [Hiroaki:30]

C. [Hiroaki:30, Hiroaki:35]

D. コンパイルエラーになる

---

### 【問題2】正解：**C**

- 比較順序：
    1. `name` が等しいなら、
    2. `age` でさらに比較
- `name` は同じでも `age` が異なるため、**重複扱いされず両方格納**

---

### 【問題3】null許容とComparatorの工夫

```java
import java.util.*;

class User {
    String name;
    User(String name) {
        this.name = name;
    }

    public String toString() {
        return String.valueOf(name);
    }
}

public class Main {
    public static void main(String[] args) {
        Comparator<User> cmp = Comparator.comparing(
            (User u) -> u.name,
            Comparator.nullsLast(String::compareTo)
        );

        Set<User> set = new TreeSet<>(cmp);
        set.add(new User("Alice"));
        set.add(new User(null));
        System.out.println(set);
    }
}
```

### 問：

このコードを実行するとどうなるか？

A. 例外が発生する

B. [null, Alice]

C. [Alice, null]

D. コンパイルエラーになる

---

### 【問題3】正解：**C**

- `Comparator.nullsLast(...)` により、`null` は最後にソートされる
- `name == null` なオブジェクトも許容され、正しく比較される

---

## ✅ Goldで重要なポイント：TreeSet + Comparator

| 観点 | 内容 |
| --- | --- |
| TreeSetの重複判定 | `compare()` の結果が `0` なら「重複」とみなす（equals() は無関係） |
| Comparatorの thenComparing | 多段比較に使える。優先度を明確化できる |
| nullの扱い | `Comparator.nullsFirst/Last()` で制御できる |
| toStringは出力用 | 判定には使われない（補助的に出力確認） |