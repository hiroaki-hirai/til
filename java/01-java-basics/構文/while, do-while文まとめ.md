# while, do-while文まとめ

以下に、Javaの `while` と `do-while` 文に関するパターン問題を難易度別に用意しました。ひっかけや典型パターンも含めています。

---

## 🔰 基本パターン問題（初級）

### 問題 1

```java
int i = 0;
while (i < 3) {
    System.out.print(i + " ");
    i++;
}
```

```java
0 1 2 
```

**解説：**

i は 0 から始まり、3 未満の間ループします。1 ループごとに 1 ずつ増加するので、0〜2 まで表示されます。

---

### 問題 2

```java
int i = 0;
do {
    System.out.print(i + " ");
    i++;
} while (i < 3);
```

```java
0 1 2 
```

**解説：**

`do-while` は**必ず1回は実行される**のが特徴。while と同様、0〜2 を出力します。

---

### 問題 3（ひっかけ）

```java
int i = 10;
while (i < 5) {
    System.out.print("Hello");
}
```

**解答：**

**無限ループにはなりませんが、何も出力されません。**

**解説：**

条件 `i < 5` は最初から `false` なのでループ本体は1度も実行されず、出力もなし。

ただし、逆に `do-while` にしていたら "Hello" が表示されていた点に注意！

---

## ⚠️ 動作の違いを問う問題（中級）

### 問題 4

```java
int i = 0;
while (i > 0) {
    System.out.print(i);
}

int j = 0;
do {
    System.out.print(j);
} while (j > 0);
```

```java
0
```

**解説：**

- `while (i > 0)`：最初から条件が `false` のため一度も実行されない（出力なし）
- `do-while (j > 0)`：`do` が先に1回実行されるため `0` が表示される

---

### 問題 5

```java
int i = 0;
while (++i < 3) {
    System.out.print(i + " ");
}
```

```java
1 2 
```

**解説：**

`++i` は前置インクリメントなので、`i` を 1 増やしてから条件判定されます。

- 1 → OK（出力 1）
- 2 → OK（出力 2）
- 3 → NG（ループ終了）

---

## 🧠 応用・アルゴリズム的（上級）

### 問題 6（累積和）

```java
int sum = 0, i = 1;
while (i <= 5) {
    sum += i;
    i++;
}
System.out.println(sum);
```

```java
15
```

**解説：**

1+2+3+4+5 = 15。典型的なループ加算パターン。

---

### 問題 7（do-whileでメニュー選択）

```java
int choice;
Scanner sc = new Scanner(System.in);
do {
    System.out.println("1. Start\n2. Exit");
    choice = sc.nextInt();
} while (choice != 2);
System.out.println("終了しました");
```

**解答：**

「2 が入力されるまで繰り返し、2 が入力されたら終了」と表示される。

**解説：**

メニュー選択などでは、**最初に必ずメニューを表示したい**ため `do-while` が適しています。`while` だと最初に条件確認だけで終了する可能性あり。

```java
while (choice != 2);
```

ここで空文となっていることに注意！

---

## 🌀 特殊パターン・無限ループ（挑戦）

### 問題 8

```java
int i = 0;
while (true) {
    if (i == 3) break;
    System.out.print(i + " ");
    i++;
}
```

```java
0 1 2 
```

**解説：**

`while (true)` で無限ループを作っていますが、`if (i == 3) break;` により脱出。i=3になると終了。

---

### 問題 9（カウンタの更新忘れ）

```java
int i = 0;
while (i < 5) {
    System.out.println("i is " + i);
    // i++ がない
}
```

**解答：**

**無限ループになる**

**解説：**

`i` の値が更新されないため、`i < 5` が永遠に `true`。

**修正案：**

```java
i++; // をループ内に追加
```