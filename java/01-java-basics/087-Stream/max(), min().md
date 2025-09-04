# max(), min()

# Q,

Streamクラスのmaxメソッドとminメソッドは、その処理内のルールに従って計算された数値の最大値、最小値をそれぞれ取得するので、処理のルール設定によっては出力の結果が真反対になることもあるという認識はあっていますか？

はい、その認識は**正しい**です。

---

### 🔹理由の整理：

`Stream.max(Comparator)` や `Stream.min(Comparator)` は、**引数に渡された `Comparator` のロジックに従って**要素の大小を比較して、最大または最小の要素を決定します。

そのため、

- 同じデータでも `Comparator` の内容（並び替えルール）によって
- `max()` と `min()` の結果は **逆になることがありえます**

---

### 🔸具体例：

```java
List<String> list = List.of("apple", "banana", "cherry");

// 文字列長で比較するComparator
Comparator<String> byLength = Comparator.comparingInt(String::length);

// 最長の文字列
String longest = list.stream().max(byLength).orElse("none");
// 最短の文字列
String shortest = list.stream().min(byLength).orElse("none");

System.out.println("Longest: " + longest);  // banana
System.out.println("Shortest: " + shortest); // apple
```

このように、`Comparator` によって「何を基準に最大・最小とするか」が変わるため、

**max()とmin()で逆転した結果になるのは自然なこと**です。

---

### 🔸逆転例：

たとえば、`byLength.reversed()` を使うと、`max()` と `min()` の意味がひっくり返ります：

```java
Comparator<String> reversedLength = Comparator.comparingInt(String::length).reversed();

// この場合、最短文字列が max() で取得される
String maxWithReversed = list.stream().max(reversedLength).orElse("none");

System.out.println("max with reversed: " + maxWithReversed);  // apple
```

---

### ✅まとめ：

- `max()` / `min()` の結果は、**Comparator次第で大きく変わる**
- 結果が真逆になることもある
- *「何を基準に最大・最小を決めるか」**を意識して `Comparator` を設計する必要がある

何か具体的なケースで結果が逆になって困った例があれば、それを元に詳細も解説できます。