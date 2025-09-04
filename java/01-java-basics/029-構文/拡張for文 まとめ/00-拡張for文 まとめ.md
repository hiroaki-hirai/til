# 拡張for文 まとめ

[２次元配列の順次取り出し](%E6%8B%A1%E5%BC%B5for%E6%96%87%20%E3%81%BE%E3%81%A8%E3%82%81%201eb62cef735080a38a44fbfe1e3574c8/%EF%BC%92%E6%AC%A1%E5%85%83%E9%85%8D%E5%88%97%E3%81%AE%E9%A0%86%E6%AC%A1%E5%8F%96%E3%82%8A%E5%87%BA%E3%81%97%2021f62cef7350803ab030e2dfffc8d02b.md)

ここでは Java Silver 試験風に、**拡張for文（enhanced for-loop）に関する典型的なパターン問題**を5問ご用意しました。

引っかけ要素や注意すべき仕様も盛り込んでいます。

---

## ✅ 拡張for文 パターン問題（Java Silver 風）

### 【問1】次のコードの出力はどれか？

```java
int[] nums = {1, 2, 3, 4};

for (int num : nums) {
    num *= 2;
    System.out.print(num + " ");
}
System.out.println();
System.out.print(Arrays.toString(nums));
```

### 選択肢:

A. `2 4 6 8`

`[2, 4, 6, 8]`

B. `2 4 6 8`

`[1, 2, 3, 4]`

C. `1 2 3 4`

`[2, 4, 6, 8]`

D. コンパイルエラー

▶ 解答・解説</summary>

**正解：B**

- `num` は **要素のコピー**であり、元配列`nums`は変更されない。
- `System.out.print(num)` は変更後を出力するが、`nums`自体は変化しない。

---

### 【問2】次のコードはコンパイルされるか？

```java
List<String> list = List.of("A", "B", "C");
for (String str : list) {
    if (str.equals("B")) {
        list.remove(str);
    }
}
```

### 選択肢:

A. 正常に実行される

B. `str`のスコープが不正でコンパイルエラー

C. 実行時に `ConcurrentModificationException` が発生する

D. `List.of()` の仕様で `remove` メソッドが呼び出せずコンパイルエラー

▶ 解答・解説</summary>

**正解：C**

- `拡張for文`中にコレクションを**構造的に変更**すると `ConcurrentModificationException` が発生する。
- また、`List.of()` は**不変リスト**なので remove 自体も問題。
    
    → **Dも近いが、コンパイルは通るためCが正解**
    

---

### 【問3】拡張for文で使えないのはどれか？

### 選択肢:

A. `int[]`

B. `List<String>`

C. `Map<String, String>`

D. `ArrayList<Integer>`

▶ 解答・解説</summary>

**正解：C**

- `Map`は直接拡張for文では使えない（`entrySet()`などを使えば可能）。
- 拡張for文の対象は **配列 or Iterable の実装クラス**。

---

### 【問4】次のコードの出力は？

```java
String[][] data = {
    {"A", "B"},
    {"C", "D"}
};

for (String[] row : data) {
    for (String s : row) {
        System.out.print(s + " ");
    }
}
```

### 選択肢:

A. `ABCD`

B. `A B C D`

C. `A C B D`

D. コンパイルエラー

▶ 解答・解説</summary>

**正解：B**

- 外側の配列から`String[]`（行）を、内側で`String`（各要素）を順に出力している。
- 空白も含めて正確に確認。

---

### 【問5】次のコードの実行結果として正しいものはどれか？

```java
String[] items = {"pen", "book", "bag"};
for (String item : items) {
    item = item.toUpperCase();
}
System.out.println(Arrays.toString(items));
```

### 選択肢:

A. `[pen, book, bag]`

B. `[PEN, BOOK, BAG]`

C. `penbookbag`

D. コンパイルエラー

▶ 解答・解説</summary>

**正解：A**

- `item = item.toUpperCase();` は**新しい文字列をitemに代入しているだけ**で、配列自体は変更されない。

String[] items = new String[3];

items[0] = “pen”;

items[1] = “book”;

items[2] = “bag”;

拡張for文は基本的には「読み取り専用・順次処理」がメインですが、**応用問題としては次のようなパターン**がJava Silverや実務理解レベルで出題されます。

---

## ✅ 拡張for文 応用問題パターン（Java Silver 応用）

---

### 🔸【応用1】入れ子構造（多次元配列 / List<List<>>）

```java
String[][] data = {
    {"a", "b"},
    {"c", "d"}
};

for (String[] row : data) {
    for (String s : row) {
        System.out.print(s.toUpperCase());
    }
}
```

### ❓出力は？

→ `ABCD`

**ポイント**：

- ネストされた拡張for文を使いこなせるか
- `row` が `String[]` 型であることに気づけるか

---

### 🔸【応用2】更新が反映されない罠

```java
int[] nums = {1, 2, 3};
for (int n : nums) {
    n *= 10;
    
}
System.out.println(Arrays.toString(nums));
```

### ❓出力は？

→ `[1, 2, 3]`

**ポイント**：

- `n` は**値のコピー**であり、`nums`は変更されない
- 配列要素を変更したい場合は**普通のfor文を使う**

---

### 🔸【応用3】拡張for文内で remove しようとして例外

```java
List<String> list = new ArrayList<>(List.of("A", "B", "C"));
for (String s : list) {
    if (s.equals("B")) list.remove(s);
}
```

### ❓実行結果は？

→ `ConcurrentModificationException` が発生

**ポイント**：

- 拡張for文で **構造的変更（removeなど）**は禁止

---

### 🔸【応用4】参照型を通じて配列の内容を変更できるか

```java
StringBuilder[] builders = {
    new StringBuilder("a"),
    new StringBuilder("b")
};

for (StringBuilder sb : builders) {
    sb.append("X");
}
for (StringBuilder sb : builders) {
    System.out.print(sb.toString() + " ");
}
```

### ❓出力は？

→ `aX bX`

**ポイント**：

- `StringBuilder` は**参照型**なので、**内部の状態は変更される**
- `拡張for文＝読み取り専用`という印象に引っかからないこと

---

### 🔸【応用5】配列＋nullチェック＋continue

```java
String[] words = {"Java", null, "Silver"};
for (String w : words) {
    if (w == null) continue;
    System.out.print(w.length() + " ");
}
```

### ❓出力は？

→ `4 6`

**ポイント**：

- `null` 要素を飛ばす`continue`の使い方
- `.length()` 呼び出しによる `NullPointerException` を防げるか

---

## ✅ 拡張for文 応用パターンまとめ

| パターン | 要点 |
| --- | --- |
| ネスト構造 | 多次元配列やList<List<>>への対応 |
| 値の更新 | プリミティブ型は変更不可（値のコピー） |
| 例外処理 | `remove` は `ConcurrentModificationException` に注意 |
| 参照型 | 内部状態の変更は可能（例：StringBuilder） |
| null処理 | `null` 判定と `continue` を併用する実務的対策 |

# Q,

System.out.println(Arrays.toString(builders));
に書き換えるても、sbと同じ出力になるということは、拡張for文は自身の内部の状態変化を外部と共有しているようですが、これは、for (StringBuilder sb : builders) { の buildersの部分でその参照を使用しているからですか？

```java
StringBuilder[] builders = {
    new StringBuilder("a"),
    new StringBuilder("b")
};

for (StringBuilder sb : builders) {
    sb.append("X");
}
for (StringBuilder sb : builders) {
    System.out.print(sb.toString() + " ");
}
```

はい、**まさにその通りです！**

拡張for文が外部と状態を共有できているのは、`for (StringBuilder sb : builders)` の **`builders` から受け取っているのが「オブジェクトの参照」だから**です。

---

## ✅ 結論：

> sb は builders 配列内の各 StringBuilder オブジェクトの参照を受け取っているので、
> 
> 
> `sb.append("X")` のように **内部状態を変更すると、それが `builders` 側にも反映される**。
> 

---

## 🔍 詳しく解説：拡張for文の動作イメージ

### 実行時には次のような動作になります：

```java
for (StringBuilder sb : builders) {
    // sb = builders[0]（1回目）
    // sb = builders[1]（2回目）
    sb.append("X");
}
```

つまり、各ループで **builders[i] の参照（アドレス）を変数 `sb` に渡している**んです。

> 🔁 sb は builders[i] の別名（エイリアス）みたいなもの
> 

---

### 🧱 イメージ（メモリの参照構造）：

```java
builders[0] → 🧱 "a" （← sb がこの参照を受け取る）
builders[1] → 🧱 "b" （← 次のループで sb に入る）

sb.append("X") → 🧱 の内部が "aX" や "bX" に変化
```

---

## ✅ 重要ポイント：**参照のコピー**

| 何が起きてる？ | 結果 |
| --- | --- |
| `sb` に `builders[i]` の参照がコピーされる | `sb` を通じて同じオブジェクトを操作できる |
| `sb.append("X")` を実行 | builders[i] の中身が変わる |
| `Arrays.toString(builders)` で表示 | append後の `"aX"`、`"bX"` が出力される |

---

## ✅ 参考比較：プリミティブ型との違い

```java
int[] nums = {1, 2, 3};
for (int n : nums) {
    n *= 10; // 値を変えても nums[i] には影響なし
}
```

- `int` は値のコピー → 外部とは**共有していない**

---

## ✅ まとめ

| 比較項目 | 結果 |
| --- | --- |
| `sb` の正体 | `builders[i]` の参照を受け取ったローカル変数 |
| 状態が共有される理由 | 同じオブジェクトの**参照**を使っているから |
| 状態の変更効果 | builders 配列側にも影響する（同じオブジェクト） |

# Q,

参照値が、sb = builders[] で同じになり共有するようになるのは理解しました。
では、２回目拡張for文の一次変数sb は１回目のsbとは別の変数名にしても問題無い認識で合っていますか？
sb1など。

はい、**その認識は完全に正しいです。**

---

## ✅ 結論：

> 2回目の拡張for文の一時変数は sb とは別の変数名（例：sb1）にしてもまったく問題ありません。
> 
> 
> なぜなら、**拡張for文の変数はそれぞれのforブロック内だけで有効なローカル変数**だからです。
> 

---

## 🔍 具体例：

```java
StringBuilder[] builders = {
    new StringBuilder("a"),
    new StringBuilder("b")
};

for (StringBuilder sb : builders) {
    sb.append("X"); // ① 最初のループ：状態を変更
}

for (StringBuilder sb1 : builders) {
    System.out.print(sb1.toString() + " "); // ② 別の名前でも同じオブジェクトを参照
}
```

✅ 出力：

```java
aX bX 
```

---

## 🧠 なぜこれでOKなのか？

### 🔸 `for (StringBuilder sb : builders)`

- これは `sb` という**ローカル変数（参照）**が `builders[i]` を1つずつ受け取っているだけ

### 🔸 `for (StringBuilder sb1 : builders)`

- `sb1` は**まったく新しいローカル変数**として宣言される
- 同じ `builders` 配列の要素を順番に参照するので、**中身は同じオブジェクト**

> 🚩 名前が違っても、中身（参照先）が同じなら、状態は共有される
> 

---

## ✅ スコープの視点からも正しい

- `sb` は最初のfor文内でしか有効
- `sb1` は2回目のfor文内でしか有効
- 両者は**完全に独立した変数スコープ**

---

## ✅ まとめ

| 比較項目 | sb（1回目） | sb1（2回目） |
| --- | --- | --- |
| スコープ | 最初のfor文内だけ | 2つ目のfor文内だけ |
| 参照しているもの | builders[0]〜builders[1] | 同じbuilders[0]〜builders[1] |
| 名前を変えてもOK？ | ✅ 問題なし | ✅ 問題なし |

# Q,

```java
StringBuilder[] builders = {
    new StringBuilder("a"),
    new StringBuilder("b")
};

for (StringBuilder sb : builders) {
    sb = new StringBuilder("X");
}
for (StringBuilder sb : builders) {
    System.out.print(sb.toString() + " ");
}
```

上記の場合の出力はどうなりますか？

とても良い質問です！

このコードの挙動を正確に理解するには、**参照の扱い（値渡し vs 参照）**についての理解が重要です。

---

## ✅ 結論：出力は

```java
a b 
```

---

## 🔍 なぜそうなるのか？詳しく解説します

### 前提の配列定義：

```java
StringBuilder[] builders = {
    new StringBuilder("a"),
    new StringBuilder("b")
};
```

- 配列 `builders` は、2つの `StringBuilder` インスタンスを参照しています。
- `builders[0]` → "a"、`builders[1]` → "b"

---

### 最初の `for` 文：

```java
for (StringBuilder sb : builders) {
    sb = new StringBuilder("X");
}
```

- この `for-each` ループは、**配列の要素を `sb` というローカル変数に代入**します。
- `sb = new StringBuilder("X");` は、**ローカル変数 `sb` に新しい参照を代入しているだけ**。
- **配列 `builders` の中身自体は一切変更されていません！**

つまりこのループは、**何の効果もありません**（副作用なし）。

---

### 2つ目の `for` 文：

```java
for (StringBuilder sb : builders) {
    System.out.print(sb.toString() + " ");
}
```

- これは `builders[0]` と `builders[1]` をそのまま取り出して表示しているだけ。
- それぞれ `"a"`、`"b"` が格納された `StringBuilder` インスタンスです。

---

## ✅ まとめ：出力とその理由

| コード | 結果 | 理由 |
| --- | --- | --- |
| `sb = new StringBuilder("X")` | 効果なし | `sb` はローカル変数。配列の中身は変わらない。 |
| `System.out.print(...)` | `a b` | 元の `builders[0]` と `builders[1]` を表示している。 |

---

## ✅ 補足：実際に配列の中身を変えたいなら？

以下のように **インデックスを使って直接変更する**必要があります：

```java
for (int i = 0; i < builders.length; i++) {
    builders[i] = new StringBuilder("X");
}
```

これなら `a b` ではなく `X X` が出力されます。

---

ご希望があれば、「参照型変数の扱い」や「for-eachとインデックスforの違い」についてさらに深堀できます！

[次の要素が取り出せない場合](%E6%8B%A1%E5%BC%B5for%E6%96%87%20%E3%81%BE%E3%81%A8%E3%82%81%201eb62cef735080a38a44fbfe1e3574c8/%E6%AC%A1%E3%81%AE%E8%A6%81%E7%B4%A0%E3%81%8C%E5%8F%96%E3%82%8A%E5%87%BA%E3%81%9B%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88%2023262cef73508068a4e9e795313fa37d.md)