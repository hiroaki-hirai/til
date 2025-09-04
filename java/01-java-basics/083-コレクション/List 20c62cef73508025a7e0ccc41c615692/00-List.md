# List

ここでは Java の **`List` インターフェース**について、以下の構成で詳しく解説します。

---

### ✅ 1. `List`とは？

`List`はJavaのコレクションフレームワークの一部で、**順序を保持した要素の集まり（リスト構造）**を表現するインターフェースです。重複要素を許容し、インデックス（位置）で要素にアクセスできます。

```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
System.out.println(list.get(0)); // "A"
```

---

### ✅ 2. 主な実装クラス

| 実装クラス | 特徴 |
| --- | --- |
| `ArrayList` | 可変長の配列。検索に強いが、挿入・削除はコストが高い |
| `LinkedList` | 双方向リスト。挿入・削除に強いが、ランダムアクセスは遅い |
| `Vector` | 古い実装。スレッドセーフ（同期化されている）が非推奨気味 |
| `Stack` | `Vector`を拡張。LIFO構造で非推奨気味（`Deque`が推奨） |

---

### ✅ 3. List の主なメソッド

| メソッド | 説明 |
| --- | --- |
| `add(E e)` | 要素を末尾に追加 |
| `add(int index, E e)` | 指定位置に要素を追加 |
| `get(int index)` | 指定位置の要素を取得 |
| `set(int index, E e)` | 指定位置の要素を置換 |
| `remove(int index)` | 指定位置の要素を削除 |
| `indexOf(Object o)` | 指定要素の最初のインデックスを返す（なければ-1） |
| `contains(Object o)` | 指定要素が含まれているか判定 |
| `size()` | 要素数を返す |
| `isEmpty()` | 空かどうか判定 |
| `clear()` | 全要素を削除 |

---

### ✅ 4. for文との連携

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");

// 拡張for文（for-each）
for (String fruit : list) {
    System.out.println(fruit);
}

// ラムダ + forEach
list.forEach(System.out::println);
```

---

### ✅ 5. 配列との違い

| 特徴 | 配列 (`Array`) | List (`ArrayList` など) |
| --- | --- | --- |
| サイズ | 固定 | 可変 |
| 型 | プリミティブも可 | ラッパー型や参照型のみ |
| 機能 | 最低限 | 豊富な操作メソッドがある |
| パフォーマンス | 高速（アクセス） | 挿入・削除・検索に柔軟対応 |

---

### ✅ 6. よくある使い方

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(10);
numbers.add(20);
numbers.add(30);

int sum = 0;
for (int num : numbers) {
    sum += num;
}
System.out.println("合計: " + sum);
```

---

### ✅ 7. Stream APIとの連携

```java
List<String> names = Arrays.asList("John", "Jane", "Tom");

List<String> result = names.stream()
    .filter(name -> name.startsWith("J"))
    .collect(Collectors.toList());

System.out.println(result); // [John, Jane]
```

---

### ✅ 8. `List`の注意点

- **スレッドセーフではない**：`ArrayList` は複数スレッドで安全に使えません（必要なら `Collections.synchronizedList`）。
- **`null`を許容**：リストは `null` を要素として追加できます。
- **`remove()`は型に注意**：`remove(1)` はインデックス、`remove(Integer.valueOf(1))` はオブジェクト削除です。

---

### ご希望に応じて：

- `List`の内部構造（配列 vs 連結リスト）
- `List<List<>>` やネスト構造の使い方
- `Set`や`Map`との比較
- `List.of()`や`List.copyOf()`（Java 9以降）など不変リストの扱い

なども掘り下げられます。希望があればお知らせください！

# Q,

Listの内部構造（配列 vs 連結リスト）をお願いします

了解しました。

ここでは `List` の **内部構造としての「配列(ArrayList)」と「連結リスト(LinkedList)」** の違いを、**図解・処理コスト・使用場面の観点から** 詳しく解説します。

---

## ✅ 1. 配列型 `ArrayList` の内部構造

### ◼ 概要

`ArrayList` は内部的に **可変長の配列（`Object[]`）** を保持しています。

```java
List<String> list = new ArrayList<>();
```

この `list` は、次のような配列構造を持ちます：

```java
インデックス:   0      1      2      3
内容:         "A"   "B"   "C"   null
```

配列のサイズがいっぱいになると、新しい配列を確保し、**全要素をコピーして拡張**します。

---

### ◼ 主な特徴

| 特徴 | 説明 |
| --- | --- |
| ランダムアクセス | `get(index)` が高速（O(1）） |
| 挿入／削除 | 中間位置で遅い（O(n)） → 要素の**ずらし操作**が発生 |
| メモリ効率 | 要素が詰まっているため効率的 |
| 拡張処理 | 拡張時は **2倍サイズの新配列を生成 + コピー**（コスト大） |

---

## ✅ 2. 連結リスト型 `LinkedList` の内部構造

### ◼ 概要

`LinkedList` は、各要素をノード（`Node`）として保持し、それぞれが **前後のノードを参照**します。

```java
null ← [A] ↔ [B] ↔ [C] → null
```

各ノードは次のような構造を持ちます：

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

---

### ◼ 主な特徴

| 特徴 | 説明 |
| --- | --- |
| ランダムアクセス | 遅い（O(n)） → **前から順にたどる必要あり** |
| 挿入／削除 | 高速（O(1)） → ノードの参照先を付け替えるだけ |
| メモリ効率 | 各要素が `prev` / `next` を保持するので**余分なメモリ**が必要 |
| 双方向移動 | `prev` / `next` による双方向アクセス可能 |

---

## ✅ 3. 処理コスト比較まとめ

| 操作内容 | `ArrayList` | `LinkedList` |
| --- | --- | --- |
| `get(int index)` | O(1)（高速） | O(n)（遅い） |
| `add(E e)` | 平均 O(1)（末尾） | O(1)（末尾） |
| `add(int index, E)` | O(n)（中間移動） | O(n) + O(1)（移動＋追加） |
| `remove(int index)` | O(n)（要素ずらし） | O(n) + O(1)（移動＋切断） |
| メモリ使用量 | 少ない | 多い（Node分） |

---

## ✅ 4. 使用場面の違い（選び方）

| シチュエーション | 適した実装 | 理由 |
| --- | --- | --- |
| ランダムアクセス（indexでアクセスが多い） | `ArrayList` | 高速な`get()`が使える |
| データの追加・削除が頻繁 | `LinkedList` | 要素の挿入／削除が効率的 |
| メモリ効率重視 | `ArrayList` | ノード構造がないので軽量 |
| 要素数が少なくパフォーマンス差が出にくい場合 | どちらでもよい | 微差のため用途に応じて |

---

## ✅ 5. 図による違い（簡易）

### 🔷 ArrayList の構造

```java
[ "A" ][ "B" ][ "C" ][ null ][ null ] ...
↑      ↑      ↑
index 0  1      2
```

### 🔷 LinkedList の構造

```java
null ← [ "A" ] ↔ [ "B" ] ↔ [ "C" ] → null
        ↑         ↑         ↑
      Node0     Node1     Node2
```

---

## ✅ 6. 実装選択の判断例（実務）

```java
// ランダムアクセスが多い：ArrayListが適
List<String> list = new ArrayList<>();
list.get(500);

// 挿入・削除が多い：LinkedListが適
List<String> list = new LinkedList<>();
list.add(0, "new"); // 頻繁な先頭追加
```

---

## 🔚 補足：不変リスト（Java 9以降）

```java
List<String> fixedList = List.of("A", "B", "C"); // 変更不可（不変）
```

配列 or 連結リストではなく、**内部的に最適な構造**を使っているケースもあります（実装依存）。**読み取り専用**ならこちらが有効。

# Q,

宣言型をList型にした変数へ複数データを格納する方法は、ArrayList、LinkedList、List.ofの３つ以外にありますか？

とても良い質問です、Hiroakiさん。

Javaで**宣言型を `List` 型（`List<T>`）とした変数に複数要素を格納する方法**として、`ArrayList` / `LinkedList` / `List.of(...)` 以外にもいくつか方法があります。

---

## ✅ 宣言型：`List<String> list = ...` の文脈における他の主な代入方法

### ① `Arrays.asList(...)`（固定サイズリスト）

```java
List<String> list = Arrays.asList("A", "B", "C");
```

- **可変要素**に見えるが、内部は固定サイズ（add/remove不可）
- 配列をラップしたリスト
- 既存配列をリストとして扱いたいときに便利

```java
String[] arr = {"A", "B", "C"};
List<String> list = Arrays.asList(arr);
```

---

### ② `Stream` を使った構築（柔軟）

```java
List<String> list = Stream.of("A", "B", "C").collect(Collectors.toList());
```

- `List.of()`に似ているが、Streamの操作をかませることが可能
- フィルタ、マッピング後にList化できるので柔軟性が高い

---

### ③ `Collections.unmodifiableList(...)`（変更不可リスト）

```java
List<String> modifiable = new ArrayList<>(List.of("A", "B", "C"));
List<String> list = Collections.unmodifiableList(modifiable);
```

- ミューテーション防止のためのラッパー
- `ArrayList`などで作成したものを安全に公開したいときに使用

---

### ④ `List.copyOf(...)`（Java 10以降の不変コピー）

```java
List<String> list = List.copyOf(Arrays.asList("A", "B", "C"));
```

- 元リストの内容を基に**変更不可なコピー**を返す
- `List.of(...)`と似ているが、「元リストから不変コピーを作る」目的

---

### ⑤ `Guava`（Google提供の拡張ライブラリ）

```java
List<String> list = ImmutableList.of("A", "B", "C"); // Guavaライブラリ
```

- **ImmutableList** は Guava による**高機能な不変リスト**
- 公式Javaの `List.of()` よりも前から存在し、`null` の検査などが厳格

※使用には依存追加が必要：

```java
<!-- Maven例 -->
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>31.1-jre</version>
</dependency>
```

---

## ✅ まとめ：List変数に複数データを渡す方法

| 方法 | 可変 | 不変 | 特徴 |
| --- | --- | --- | --- |
| `new ArrayList<>(...)` | ◯ | ✕ | 最も柔軟。自由に追加/削除できる |
| `new LinkedList<>(...)` | ◯ | ✕ | 頻繁な挿入・削除に最適 |
| `List.of(...)` | ✕ | ◯ | Java 9以降。高速・安全 |
| `Arrays.asList(...)` | △ | △ | サイズ固定。内部は配列（古い形式） |
| `Stream.of(...).collect()` | ◯ | ✕ | 柔軟な構築が可能 |
| `Collections.unmodifiableList(...)` | ✕ | ◯ | ミューテーション防止のラッパー |
| `List.copyOf(...)` | ✕ | ◯ | Java 10以降。Listの不変コピー |
| `ImmutableList.of(...)`（Guava） | ✕ | ◯ | 高機能な不変リスト。null非許容などが特徴 |

---

### ✳補足：Listインターフェースの変数であるメリット

宣言型を `List<String>` とすることで：

```java
List<String> list = new ArrayList<>();
// ↓
list = new LinkedList<>();
// ↓
list = List.of("A", "B", "C");
```

のように、**具体的な実装を後から自由に切り替えられる柔軟性**があります（DIの基本思想にも通じます）。

ここでは **Java Silver レベル**に対応した「`List` に関するパターン問題（引っかけ要素あり）」を出題します。

Silver試験では `ArrayList` を中心とした `List` の基本操作が主に問われるため、**add/get/removeの基本API**や**参照の動き**を中心に出題します。

---

## 🔰 パターン問題（Java Silver風）

### 【問題1】基本操作とインデックスの理解

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add(1, "C");
        list.remove(0);
        System.out.println(list.get(1));
    }
}
```

### 問：

このコードを実行したときの出力はどれか？

A. A

B. B

C. C

D. 例外が発生する

---

【問題1】正解：**B**

```java
list.add("A");          // [A]
list.add("B");          // [A, B]
list.add(1, "C");       // [A, C, B]
list.remove(0);         // [C, B]
System.out.println(list.get(1)); // "B"
```

### 【問題2】参照の共有による影響

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<StringBuilder> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder("Hello");
        list.add(sb);
        sb.append(" World");
        System.out.println(list.get(0));
    }
}
```

### 問：

このコードを実行したときの出力はどれか？

A. Hello

B. Hello World

C. null

D. コンパイルエラー

---

### 【問題2】正解：**B**

- `StringBuilder` は**ミュータブル（可変）オブジェクト**
- `sb.append(" World")` は、`list` 内の同じ参照にも影響する

```java
list.get(0) → sb → "Hello World"
```

### 【問題3】固定サイズリストの操作

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("A", "B", "C");
        list.set(1, "X");
        list.add("D");
        System.out.println(list);
    }
}
```

### 問：

このコードを実行したとき、どうなるか？

A. [A, X, C, D]

B. [A, B, C, D]

C. 実行時例外が発生する

D. コンパイルエラーになる

---

### 【問題3】正解：**C**

- `Arrays.asList()` で作られたリストは**固定サイズ**
- `list.set()` はOK（内容の置換はできる）
- `list.add()` はUnsupportedOperationExceptionを投げる

---

### 【問題4】removeの型誤認

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.remove(1);
        System.out.println(list);
    }
}
```

### 問：

このコードの出力は？

A. [1, 2]

B. [2]

C. [1]

D. 実行時例外が発生する

---

### 【問題4】正解：**C**

- `list.remove(1)` は **インデックス1の要素（値2）を削除**
- 残るのは `[1]`

※ 注意：`list.remove(Integer.valueOf(1))` であれば値1の削除になる（これはSilverのひっかけパターン）

---

## ✅ 総まとめ：よく出るSilver風トラップ集（List編）

| トラップ内容 | 説明 |
| --- | --- |
| `remove(int)` vs `remove(Object)` | 引数が `int` だと**インデックス削除**になる |
| `Arrays.asList()` のサイズ固定 | `add()` / `remove()` 不可。`set()`のみ可能 |
| ミュータブルなオブジェクトの共有 | `List`に追加後に元オブジェクトを変更すると中身に影響 |
| 拡張for文は要素の変更に不向き | `remove()` などは `Iterator` を使うべき（ConcurrentModificationExceptionの原因） |

ここでは Java Gold レベルに対応した、**より深い知識や注意点を突いた `List` に関するパターン問題**を出題します。

Goldレベルでは以下のような視点がよく問われます：

- **型消去（ジェネリクス）**
- **null許容**
- **不変リストの特性**
- **Streamとの連携**
- **ミュータブル vs イミュータブル**
- **Comparatorやequalsの挙動**

---

## 🧠 List パターン問題（Java Gold風）

---

### 【問題1】nullとソートの罠

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("C");
        list.add(null);
        list.add("A");

        Collections.sort(list);
        System.out.println(list);
    }
}
```

### 問：

このコードを実行した結果として**正しいもの**はどれか。

A. [null, A, C]

B. [A, C, null]

C. 例外が発生する

D. コンパイルエラーになる

---

### 【問題1】正解：**C**

- `Collections.sort()` は **null非許容**
- `null` を含んだリストをソートしようとすると `NullPointerException` が発生する
    
    （→ Comparatorを使えば回避可能）
    

### 【問題2】不変リストとミュータブルオブジェクト

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<StringBuilder> list = List.of(new StringBuilder("A"), new StringBuilder("B"));
        list.get(0).append("X");
        System.out.println(list);
    }
```

### 問：

このコードの出力として**正しいもの**はどれか。

A. [AX, B]

B. [A, B]

C. 例外が発生する

D. コンパイルエラーになる

---

### 【問題2】正解：**A**

- `List.of(...)` は **不変リスト（構造の変更不可）**
- ただし、**中身のオブジェクトがミュータブルであれば変更可能**

```java
list.get(0).append("X"); // OK（リスト構造は変更していない）
```

### 【問題3】ジェネリクスと型消去の影響

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List raw = new ArrayList();     // raw型
        raw.add("A");
        raw.add(100);

        for (Object obj : raw) {
            System.out.println(obj.toString().length());
        }
    }
}
```

### 問：

このコードを実行した結果として**正しいもの**はどれか。

A. 1, 3

B. 1, 100

C. 1, 2

D. 例外が発生する

---

### 【問題3】正解：**A**

- raw型 `List` は**型安全でない**
- `"A".toString().length()` → 1
- `100.toString().length()` → `"100"` → 3

### 【問題4】stream + removeIf の副作用

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(List.of("apple", "banana", "apricot"));

        list.stream().filter(s -> s.startsWith("a")).forEach(s -> list.remove(s));
        System.out.println(list);
    }
}
```

### 問：

このコードを実行したときの動作として**正しいもの**はどれか。

A. [banana]

B. [banana, apricot]

C. ConcurrentModificationExceptionが発生する

D. コンパイルエラーになる

### 【問題4】正解：**C**

- `stream().forEach()` の中で `list.remove()` を呼ぶと、**同じリストに対して同時に変更を加えることになり例外**
- `removeIf()` や `iterator.remove()` を使うべき

---

## 🎯 Goldレベルの理解ポイント（頻出）

| 観点 | 説明 |
| --- | --- |
| null許容性 | `sort()`や`Stream.sorted()`はnullで例外になる |
| 不変 vs ミュータブル | `List.of()`は構造だけ不変、中身の参照先は変更可能 |
| raw型と型消去 | コンパイルOKでも、実行時に型の差異が現れることがある |
| Stream APIと副作用 | stream処理中に元のコレクションを変更してはいけない（破壊的） |

[of()](List%2020c62cef73508025a7e0ccc41c615692/of()%2021d62cef7350802a8beeedefc47a2025.md)