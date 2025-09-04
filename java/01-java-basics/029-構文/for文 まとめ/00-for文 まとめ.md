# for文 まとめ

---

# ✅ Java Silver風 `for` 文パターン問題：解答＆解説

---

### 【問題 1】基本構文の理解

```java
for (int i = 0; i < 3; i++) {
    System.out.print(i + " ");
}
```

**✅ 解答：A. 0 1 2**

**🧠 解説：**

- 初期値：`i = 0`
- 条件：`i < 3`（true の間繰り返す）
- 出力：`0`, `1`, `2` → 条件により `3` では止まる
- 標準的な `for` 文の構造問題。

---

### 【問題 2】セミコロンの位置

```java
for (int i = 0; i < 3; i++);
System.out.print("done");
```

**✅ 解答：B. done は1回表示される**

**🧠 解説：**

- `for (...) ;` により、**空のループ**が回るだけ。
- `System.out.print("done");` はループの外 → **1回だけ実行**される。
- セミコロンの位置による「空文トラップ」。

---

### 【問題 3】初期化と更新の省略

```java
int i = 0;
for (; i < 3; ) {
    System.out.print(i + " ");
    i++;
}
```

**✅ 解答：A. 0 1 2**

**🧠 解説：**

- `for` 文の構文は「省略可能」。`i++` を本体に移して正常に動作。
- `while (i < 3)` と本質的には同じ動き。

---

### 【問題 4】拡張for文の理解

```java
int[] nums = {1, 2, 3};
for (int n : nums) {
    System.out.print(n + " ");
}
```

**✅ 解答：A. 1 2 3**

**🧠 解説：**

- 拡張 `for` 文は「配列やコレクションの各要素を順に処理」。
- 配列 `nums` の中身 `1, 2, 3` をそのまま出力。
- `n` はコピーされた値（要素自体を操作しているわけではない）。

---

### 【問題 5】複数変数の使用

```java
for (int i = 0, j = 3; i < j; i++, j--) {
    System.out.print(i + ":" + j + " ");
}
```

**✅ 解答：A. 0:3 1:2**

**🧠 解説：**

- i=0, j=3 → 条件 i<j 成立 → 0:3
- i=1, j=2 → 成立 → 1:2
- i=2, j=1 → 条件不成立 → ループ終了
- **2変数制御のパターン**はSilverで出やすい。

---

### 【問題 6】範囲外アクセス（拡張for文）

```java
String[] list = {"A", "B", "C"};
for (String s : list) {
    s = s.toLowerCase();
}
System.out.println(Arrays.toString(list));
```

**✅ 解答：B. [A, B, C]**

**🧠 解説：**

- `s` は各要素の**コピー**。元配列は変更されない。
- `s.toLowerCase()` は `s` 変数のみに適用され、**listの中身は変化しない**。

---

### 【問題 7】for文の変数スコープ

```java
for (int i = 0; i < 2; i++) {
    System.out.print(i);
}
System.out.print(i); // ← ここ
```

# ✅ Java `for`文 応用問題【解答 & 解説付き】

---

### 【応用 1】ネストされた `for` のトラップ

```java
for (int i = 0; i < 2; i++) {
    for (int j = 2; j > 0; j--) {
        if (i == j) break;
        System.out.print(i + ":" + j + " ");
    }
}
```

**✅ 解答：A. 0:2 0:1 1:2**

**🧠 解説：**

- i = 0 のとき：j = 2, 1 → i ≠ j → 出力 → j = 0 → 終了
- i = 1 のとき：j = 2 → i ≠ j → 出力 → j = 1 → i == j → break（内側ループのみ終了）
- **break は内側ループを抜けるだけで、外側は継続**

---

### 【応用 2】`continue` の作用

```java
for (int i = 0; i < 5; i++) {
    if (i % 2 == 0) continue;
    System.out.print(i + " ");
}
```

**✅ 解答：B. 1 3**

**🧠 解説：**

- `i % 2 == 0` のとき `continue` → 0, 2, 4 はスキップ
- 出力されるのは **奇数：1 と 3**

---

### 【応用 3】`拡張for`＋配列書き換え

```java
int[] arr = {1, 2, 3};
for (int x : arr) {
    x *= 2;
}
System.out.println(Arrays.toString(arr));
```

**✅ 解答：B. [1, 2, 3]**

**🧠 解説：**

- `x` は配列の要素の**コピー**（値渡し）であり、`x *= 2;` は `arr` には影響しない
- 配列本体を変更するには、**インデックス付きの通常の for 文が必要**

---

### 【応用 4】ループ変数のスコープと再利用

```java
for (int i = 0; i < 3; i++) {
    int count = i;
}
System.out.println(count);
```

**✅ 解答：D. コンパイルエラー**

**🧠 解説：**

- `count` は `for` 文の**ブロック内ローカル変数** → **外では存在しない**
- スコープ外での変数使用は**コンパイルエラー**

---

### 【応用 5】複雑な更新式と副作用

```java
int sum = 0;
for (int i = 0; i < 5; sum += i, i++);
System.out.println(sum);
```

**✅ 解答：A. 10**

**🧠 解説：**

- `sum += i, i++` → i=0〜4 まで `sum` に加算、i は各ループで+1
- 実行の流れ：
    - i=0 → sum=0 → i++
    - i=1 → sum=1 → i++
    - i=2 → sum=3 → i++
    - i=3 → sum=6 → i++
    - i=4 → sum=10 → i++
    - i=5 → 終了
- **複数式（,）を使った更新パターン**

---

### 【応用 6】2重ループ＋breakラベル付き

```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i + j > 3) break outer;
        System.out.print(i + "" + j + " ");
    }
}
```

**✅ 解答：B. 00 01 02 10 11**

**🧠 解説：**

- `i+j > 3` になったら **outerループごと抜ける**（ラベル付きbreak）
- 実行の流れ：
    - i=0 → j=0~2 → OK → 出力：00 01 02
    - i=1 → j=0~1 → OK → 出力：10 11
    - i=1, j=2 → i+j=3 → OK
    - i=2, j=2 → i+j=4 → break outer → 終了

---

[()内の省略](for%E6%96%87%20%E3%81%BE%E3%81%A8%E3%82%81%201e962cef735080cdb92ad5e5b09f211a/()%E5%86%85%E3%81%AE%E7%9C%81%E7%95%A5%2022e62cef735080c6a8bacede7eb2d6bb.md)