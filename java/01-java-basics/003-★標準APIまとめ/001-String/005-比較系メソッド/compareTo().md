# compareTo()

---

# ✅ `String.compareTo()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `compareTo(String anotherString)` |
| 戻り値の型 | `int`（比較結果を示す整数値） |
| 主な用途 | **文字列同士の大小比較（辞書順順序比較）** |

---

# 🔹 シグネチャ（定義）

```java
public int compareTo(String anotherString)
```

- **引数**：比較対象となる別の文字列
- **戻り値**：
    - **0**：両者が「完全一致」
    - **負の値**：呼び出し元（this）が引数より「辞書順で前」
    - **正の値**：呼び出し元（this）が引数より「辞書順で後」

---

# 🔹 使用例（基本）

```java
String a = "apple";
String b = "banana";

System.out.println(a.compareTo(b)); // 負の値（aはbより前）
System.out.println(b.compareTo(a)); // 正の値（bはaより後）
System.out.println(a.compareTo("apple")); // 0（完全一致）
```

✅ **辞書順（アルファベット順）比較**を行う！

---

# 🔹 比較のルール（細かい仕様）

- **先頭の文字から順番に**、Unicode（文字コード）を比較する。
- 最初に違いが出た時点で比較終了する。
- 途中まで一致している場合は、**長いほうが後（大きい）**と判定される。

### 🔸 例：文字コード順で比較

```java
System.out.println("abc".compareTo("abd")); // 負の値 ("c" vs "d")
System.out.println("abc".compareTo("ab"));  // 正の値 (長さで比較)
```

---

# 🔹 文字コードベースなので注意！

| 文字 | Unicodeコード |
| --- | --- |
| 'A' | 65 |
| 'B' | 66 |
| 'a' | 97 |
| 'b' | 98 |

✅ **大文字と小文字では小文字の方が後**にくる！

---

# 🔹 実務での主な使い方

| シチュエーション | 使用例 |
| --- | --- |
| ソート順を決めたい場合 | `Collections.sort(list)` の内部で利用される |
| 順位判定をしたい場合 | `compareTo()`で結果を使ってif-else分岐 |
| 自作クラスでソート順を定義したい場合（Comparable実装） | compareTo()をオーバーライドして自然順序付けをする |

---

# ✅ まとめ

| 比較結果 | 説明 |
| --- | --- |
| 0 | 同じ内容 |
| 負の整数 | 呼び出し元が引数より前（小さい） |
| 正の整数 | 呼び出し元が引数より後（大きい） |
| 比較方法 | 先頭から順にUnicodeコード値を比較（違いが出た時点で決定） |

---

# 🔥 超重要注意点まとめ

- **大文字と小文字は区別される！**
    - `"Apple"`と`"apple"`は異なる！（小文字のほうが後）
- **nullには呼び出せない！**
    - 呼び出し元または引数がnullだと**NullPointerException**が発生！

---

# ✨ 超シンプルまとめ

> compareTo()は「文字列同士を辞書順で比較する」！
0なら一致、負なら前、正なら後！
大文字小文字は区別されるので注意！
> 

✅ この感覚を押さえておけば、試験でも実務でもバッチリ使えます！

以下に、**「compareTo()専用 引っかけ問題集（大小区別・長さ違いトリック対応 全8問）」** をご用意しました！

特に、

**「大文字小文字の違い」「長さ違いの扱い」「null問題」「途中一致トリック」**

に引っかからないように練習できるセットになっています！

---

# 🔺 `compareTo()`専用 引っかけ問題集（全8問）

---

## 【問題編】

---

### ❓ 問題 1：基本的な大小比較

```java
String a = "apple";
String b = "banana";

System.out.println(a.compareTo(b));
```

### ✅ 問題 1の答え

**答え**：C. 負の値

**解説**：

- `"apple"`は`"banana"`より辞書順で前にくる！

---

### ❓ 問題 2：大文字小文字の違い①

```java
String a = "Apple";
String b = "apple";

System.out.println(a.compareTo(b));
```

### ✅ 問題 2の答え

**答え**：C. 負の値

**解説**：

- `"Apple"`と`"apple"`は違う！
- 'A'（65）と'a'（97）では'A'の方が小さいので、compareToは負の値！

---

### ❓ 問題 3：長さの違い①（短いほうが先か？）

```java
String a = "abc";
String b = "abcd";

System.out.println(a.compareTo(b));
```

### ✅ 問題 3の答え

**答え**：C. 負の値

**解説**：

- 先頭が一致しても、短い方（"abc"）は辞書順で前と判定される！

---

### ❓ 問題 4：長さの違い②（途中一致）

```java
String a = "abcd";
String b = "abc";

System.out.println(a.compareTo(b));
```

### ✅ 問題 4の答え

**答え**：B. 正の値

**解説**：

- 途中までは一致するが、"abcd"は"abc"より**長い**ので大きい判定！

---

### ❓ 問題 5：途中まで一致して最後だけ違う場合

```java
String a = "abcx";
String b = "abcy";

System.out.println(a.compareTo(b));
```

### ✅ 問題 5の答え

**答え**：C. 負の値

**解説**：

- "abcx"と"abcy"は、最後の文字'x'と'y'で比較する → 'x'の方が小さい！

---

### ❓ 問題 6：完全一致の場合

```java
String a = "OpenAI";
String b = "OpenAI";

System.out.println(a.compareTo(b));
```

### ✅ 問題 6の答え

**答え**：A. 0

**解説**：

- 内容が完全一致ならcompareTo()は0！

---

### ❓ 問題 7：nullとの比較①（呼び出し元がnull）

```java
String a = null;
String b = "test";

System.out.println(a.compareTo(b));
```

### ✅ 問題 7の答え

**答え**：D. 実行時例外

**解説**：

- 呼び出し元（a）がnull → **NullPointerException**！

---

### ❓ 問題 8：nullとの比較②（引数がnull）

```java
String a = "test";
String b = null;

System.out.println(a.compareTo(b));

```

### ✅ 問題 8の答え

**答え**：D. 実行時例外

**解説**：

- 引数（b）がnullでもcompareTo内でnullチェックがあり、**NullPointerException**！

---

# ✅ compareTo()引っかけ注意ポイントまとめ

| 観点 | 内容 |
| --- | --- |
| 大文字小文字は区別する | 'A'と'a'は違う！（Aが小さい） |
| 長さが違う場合 | 先頭一致でも短い方が前、長い方が後 |
| 途中で違いが出た場合だけ比較終了 | 全体を見ずに、違いが出た文字のコード値だけで決まる |
| null比較 | 呼び出し元または引数がnullならNullPointerException発生 |

---

# ✨ 超シンプルまとめ

> compareTo()は辞書順比較！
大文字小文字を区別！
途中一致でも長さで判定あり！
nullに注意！（安全設計必須！）
> 

✅ この感覚を押さえておけば、試験・実務どちらでもバッチリ使いこなせます！

以下に、**「compareTo()を使ったリストソート演習（自然順・逆順対応・全8問）」** をご用意しました！

- *自然順（昇順）と逆順（降順）**を自在に使い分ける感覚を鍛えます！

---

# 🔺 `compareTo()`を使ったリストソート演習（自然順・逆順）

---

## 【問題編】

---

### ❓ 問題 1：基本 – 文字列リストを自然順にソート

```java
List<String> fruits = List.of("banana", "apple", "cherry");
List<String> sorted = new ArrayList<>(fruits);

Collections.sort(sorted);

System.out.println(sorted);
```

### ✅ 問題 1の答え

**答え**：A. `[apple, banana, cherry]`

**解説**：

- Collections.sort()で自然順（compareTo()基準）に並び替え！

---

### ❓ 問題 2：逆順にソートするには？

上記のリストを**降順（逆順）**に並び替えたい。正しいコードはどれ？

A.

```java
Collections.reverse(sorted);
```

B.

```java
Collections.sort(sorted, Collections.reverseOrder());
```

### ✅ 問題 2の答え

**答え**：D. AとBどちらでもOK

**解説**：

- ソート後にreverse()してもよいし、最初からreverseOrder()を渡してもよい！

---

### ❓ 問題 3：逆順ソート時の注意点

Collections.sort(list, Collections.reverseOrder())を使うとき、

何を前提にするべきか？

A. 要素がComparableを実装していること

B. 要素のサイズが偶数であること

C. listがすでにソート済みであること

D. listのサイズが10以上であること

### ✅ 問題 3の答え

**答え**：A. 要素がComparableを実装していること

**解説**：

- reverseOrder()は内部的にcompareTo()を使うため、必須条件！

---

### ❓ 問題 4：compareTo()を使ったカスタム並び替え（昇順）

次のようなリストを昇順にソートしてください。

```java
List<String> names = List.of("Taro", "Hanako", "Jiro");
```

A.

```java
Collections.sort(names, (a, b) -> a.compareTo(b));
```

C.

```java
Collections.sort(names);
```

### ✅ 問題 4の答え

**答え**：D. AとC両方OK

**解説**：

- Collections.sort(list)も自然順ソート（compareTo使用）。
- 明示的にa.compareTo(b)でももちろんOK！

---

### ❓ 問題 5：カスタム逆順並び替え（降順）

同じリストを降順にソートしたい。正しいコードは？

A.

```java
Collections.sort(names, (a, b) -> b.compareTo(a));
```

C.

```java
Collections.reverse(names);
```

### ✅ 問題 5の答え

**答え**：D. AとC両方OK

**解説**：

- b.compareTo(a)で逆順もできるし、
- 一旦自然順で並べた後にreverse()でもできる！

---

### ❓ 問題 6：自然順とは？

「自然順」とは何を指す？

A. 要素が持つ数値の昇順

B. コレクションの挿入順

C. 辞書順や数値順（Comparableが定義する順番）

D. メモリ上の配置順

### ✅ 問題 6の答え

**答え**：C. 辞書順や数値順（Comparableが定義する順番）

**解説**：

- Comparableインタフェースが提供する「自然な並び順」！

---

### ❓ 問題 7：Listのソートでnull要素があったら？

compareTo()を使うCollections.sort()でリストにnullがあると？

A. ソートできる

B. NullPointerExceptionが発生する

C. nullは無視される

D. nullは最後に自動的に回る

### ✅ 問題 7の答え

**答え**：B. NullPointerExceptionが発生する

**解説**：

- compareTo()はnullと比較できない（NPE発生）！

---

### ❓ 問題 8：Listの自然順→逆順を変えるだけならどうする？

一番シンプルに逆順にするなら？

A. Collections.reverse(list)

B. Collections.sort(list, Collections.reverseOrder())

C. 自前でCollections.sort(list, (a, b) -> b.compareTo(a))

D. AとB両方OK（状況による）

### ✅ 問題 8の答え

**答え**：D. AとB両方OK（状況による）

**解説**：

- 既に自然順ソート済みならreverse()だけでも逆順にできる！
- ソートからやるならreverseOrder()でsort！

---

# ✅ まとめ表

| シチュエーション | 使うべきコード例 |
| --- | --- |
| 自然順（昇順） | `Collections.sort(list)` |
| 逆順（降順） | `Collections.sort(list, Collections.reverseOrder())` |
| 既に自然順済みリストを逆順にするだけ | `Collections.reverse(list)` |
| カスタム比較（明示的に書く場合） | `(a, b) -> a.compareTo(b)` や `(a, b) -> b.compareTo(a)` |

---

# ✨ 超シンプルまとめ

> compareTo()は自然順ソートの基礎！
b.compareTo(a)で逆順！
Collections.reverseOrder()も便利！
nullには要注意（NPE発生）！
> 

✅ この感覚を押さえれば、試験も実務もバッチリです！

以下に、**「ComparableとComparatorの違い・使い分け演習（応用版・全8問）」** をご用意しました！

今回は、

**「ComparableとComparatorの使いどころの見極め」「ソート基準のカスタマイズ」**

を正しく使い分けられるかにフォーカスしています！

---

# 🔺 Comparable vs Comparator 違い・使い分け演習（応用版）

---

## 【問題編】

---

### ❓ 問題 1：Comparableとは何か？

次の説明で**正しいもの**はどれ？

A. 複数のソート基準を外部から指定できるインタフェース

B. クラス自身が「自然順序」を持つために実装するインタフェース

C. 1回だけ比較できる便利メソッドを提供するクラス

D. ラムダ式専用のインタフェース

### ✅ 問題 1の答え

**答え**：B. クラス自身が「自然順序」を持つために実装するインタフェース

---

### ❓ 問題 2：Comparatorとは何か？

次の説明で**正しいもの**はどれ？

A. クラス自身に直接compareToメソッドを持たせるインタフェース

B. ソート基準を外から提供できるインタフェース

C. 自動的に自然順ソートが適用される仕組み

D. null比較を強制的に禁止するインタフェース

### ✅ 問題 2の答え

**答え**：B. ソート基準を外から提供できるインタフェース

---

### ❓ 問題 3：Comparableを使うべきケース

次のうち、**Comparableを使う方が自然なケース**は？

A. 商品リストを「価格順」に並べる

B. 人リストを「年齢順」「名前順」で切り替える

C. 日付リストを「日付順」に並べる

D. ログメッセージを「エラー優先」で並べる

### ✅ 問題 3の答え

**答え**：C. 日付リストを「日付順」に並べる

**解説**：

- 日付のように**自然な1つの順序があるもの**はComparableで自然順を持たせるのが適切！

---

### ❓ 問題 4：Comparatorを使うべきケース

次のうち、**Comparatorを使う方が適切なケース**は？

A. 文字列リストをアルファベット順に並べる

B. 顧客リストを「年齢→購入金額→名前順」に並べたい

C. 単純に日付リストを日付順に並べたい

D. 数値リストを小さい順に並べたい

### ✅ 問題 4の答え

**答え**：B. 顧客リストを「年齢→購入金額→名前順」に並べたい

**解説**：

- ソート基準を切り替えたり複雑な順序を作りたいならComparator！

---

### ❓ 問題 5：Comparableの実装例で正しいもの

```java
public class Person implements Comparable<Person> {
    private String name;
    private int age;

    @Override
    public int compareTo(Person other) {
        return this.age - other.age;
    }
}
```

### ✅ 問題 5の答え

**答え**：B. 年齢順（昇順）

**解説**：

- this.age - other.age は昇順になる！（小さい順）

---

### ❓ 問題 6：Comparatorを使ったカスタム比較

年齢→名前順に並べたい場合、正しいコードは？

**答え**：C.

```java
list.sort(Comparator.comparing(Person::getAge).thenComparing(Person::getName));
```

**解説**：

- 年齢優先、同年齢なら名前順にする複数条件ソート！

---

### ❓ 問題 7：Comparable実装クラスのソート方法

次のリストを自然順に並べたいとき、正しい方法は？

A.

```java
Collections.sort(list);
```

B.

```java
Collections.sort(list, Comparator.naturalOrder());
```

### ✅ 問題 7の答え

**答え**：C. どちらもOK

**解説**：

- Comparableを実装していれば、Collections.sort(list)もComparator.naturalOrder()も使える！

---

### ❓ 問題 8：Comparatorを使うメリット

Comparatorを使うことで得られるメリットはどれ？

A. 複数の異なる順序で簡単に並べ替えができる

B. ソート処理が高速化される

C. compareToを使うよりコードが短くなる

D. 自動的にnull安全になる

### ✅ 問題 8の答え

**答え**：A. 複数の異なる順序で簡単に並べ替えができる

**解説**：

- Comparatorはその場その場で**異なるソート基準**を自由に作れる！

---

# ✅ まとめ表

| 比較観点 | Comparable | Comparator |
| --- | --- | --- |
| 目的 | 自然順をクラス自体に持たせる | 外部から柔軟に順序を指定する |
| 実装場所 | ソート対象クラス自身 | ソートロジックを外に分離 |
| 基本メソッド | `compareTo(T o)` | `compare(T o1, T o2)` |
| 複数条件切り替えできるか | ❌ できない | ✅ 簡単にできる（thenComparingなど） |
| nullに自動対応するか | ❌ しない | ❌ デフォルトではしない（ただしnullsFirst/lastを使えば対応可） |

---

# ✨ 超シンプルまとめ

> Comparable＝自然順（クラス自身で固定順序）
Comparator＝カスタム順（外から柔軟に指定）
特に複数条件や動的切り替えはComparator一択！
> 

✅ この感覚を押さえておけば、試験・実務両方でプロレベルに使い分けできます！