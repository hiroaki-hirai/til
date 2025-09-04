# if文　まとめ

もちろんです！Javaの`if`文の理解を深めるための**パターン問題集（初級～中級向け）**を作成しました。トリックや注意点も混ぜています。

---

## 🔹 if文 パターン問題集（全10問）

### 【基本確認編】

### 🔸 Q1：出力は？

```java
int x = 10;
if (x > 5)
    System.out.println("A");
    System.out.println("B");
```

✅ **正解：B. AB**

🔍 **解説：** `x > 5` は真なので"A"が出力される。

その後の`System.out.println("B");`はif文の外にあるので、必ず実行される。

---

### 🔸 Q2

```java
int x = 3;
if (x < 5)
    System.out.println("A");
    System.out.println("B");
```

✅ **正解：B. AB**

🔍 **解説：** インデントに惑わされないこと。

`if`の対象は直後の1文（`System.out.println("A");`）だけ。

`System.out.println("B");`は**if文の外**なので常に実行される。

---

### 🔸 Q3

```java
int x = 7;
if (x < 5)
    System.out.println("A");
else
    System.out.println("B");
    System.out.println("C");
```

✅ **正解：B. BC**

🔍 **解説：**

`x < 5`は偽 → `else`ブロックに入って「B」が出力される。

その後はif-elseの外なので「C」も出力。

---

### 🔸 Q4

```java
int x = 10;
if (x > 5)
    if (x < 20)
        System.out.println("A");
    else
        System.out.println("B");
```

✅ **正解：A. A**

🔍 **解説：**

ネストされた`if`文。

`x > 5` → 真 → 内側の`if(x < 20)`も真 → `A`出力。

`else`は内側の`if(x < 20)`に対応している。

---

### 🔸 Q5

```java
int x = 5;
if (x == 5)
    System.out.println("A");
else if (x = 10)
    System.out.println("B");
```

✅ **正解：C. コンパイルエラー**

🔍 **解説：**

`else if (x = 10)` は**代入**になっていて、booleanではないため**エラー**。

比較には `==` を使う必要がある。

---

### 🔸 Q6

```java
boolean flag = true;
if (flag = false)
    System.out.println("A");
else
    System.out.println("B");
```

✅ **正解：B. B**

🔍 **解説：**

`flag = false` は**代入式**で、評価結果は `false`。

したがって `if` 文には入らず、`else` が実行される。

---

### 🔸 Q7

```java
int x = 10, y = 20;
if (x < y)
    if (x == 10)
        System.out.println("A");
    else
        System.out.println("B");
else
    System.out.println("C");
```

✅ **正解：A. A**

🔍 **解説：**

`x < y` → 真

→ 次の`if(x == 10)`も真

→ `A`が出力される。`else`は内側の`if`に対応。

---

### 🔸 Q8

```java
int x = 0;
if (x > 0)
    System.out.println("A");
else if (x >= 0)
    System.out.println("B");
else
    System.out.println("C");
```

✅ **正解：B. B**

🔍 **解説：**

`x > 0` → 偽

`x >= 0` → 真 → `B`出力

`else`までは到達しない。

---

### 🔸 Q9

```java
String s = null;
if (s != null && s.length() > 0)
    System.out.println("A");
else
    System.out.println("B");
```

✅ **正解：B. B**

🔍 **解説：**

`&&`は**短絡評価（ショートサーキット）**される。

`s != null` が偽なので、`s.length()`は評価されず `B` が出力される。

→ NPEを防げる安全な書き方。

---

### 🔸 Q10

```java
int x = 3;
if (x > 2)
    System.out.println("A");
    System.out.println("B");
```

✅ **正解：B. AB**

🔍 **解説：**

Q2と同様、ブロック `{}` を使っていないので `if`の対象は1行目だけ。

`B`は無条件で出力される。

以下に「**if文の応用編：条件分岐＋入れ子（ネスト）＋論理演算（&&, ||）**」をすべて組み合わせた問題を用意しました。すべて**選択肢付き＋意図的なトリック含み**で、Silver〜Gold試験や実務想定にも対応しています。

---

## 🧠 if文 応用パターン問題（条件分岐 + 入れ子 + 論理演算）

---

### 🔸 Q1：出力は？

```java
int a = 5, b = 10;
if (a > 0 && b > 5)
    if (a + b < 20 || b % 2 == 0)
        System.out.println("A");
    else
        System.out.println("B");
else
    System.out.println("C");
```

✅ **正解：A**

💡 **ポイント：**

・`&&` と `||` の**短絡評価**（trueなら右は評価しない）に注意。

・入れ子if文でも正しく対応する`else`を見極める。

---

### 🔸 Q2

```java
int x = 3, y = 8;
if (x < 5)                              // true
    if (y < 10)                         // true
        if (x + y == 11)                // 11 == 11 → true
            System.out.println("OK");
        else
            System.out.println("NG");
else
    System.out.println("End");
```

✅ **正解：A. OK**

💡 **ポイント：**

・`else`は**直前のifにだけ**対応する。

・インデントに惑わされず、**構文構造**で判断！

---

### 🔸 Q3

```java
boolean flag = true;
int num = 7;

if (flag || num++ > 10)   // flag == true → || の右は評価されない
    System.out.println(num);
else
    System.out.println("Else");
```

✅ **正解：A. 7**

💡 **ポイント：**

・`||`は**左がtrueなら右は評価されない（副作用も発生しない）**

・副作用（`num++`）に依存するコードに注意！

---

### 🔸 Q4

```java
int score = 85;
if (score >= 90)                 // false
    System.out.println("A");
else if (score >= 80 && score < 90)  // true && true → true
    if (score % 2 == 0)          // 85 % 2 == 1 → false
        System.out.println("B");
    else
        System.out.println("C");
else
    System.out.println("D");
```

✅ **正解：C. C**

💡 **ポイント：**

・条件を**連続的に分岐する場合**、中の if にも注意が必要。

・`else if` で複数のスコア帯を分ける典型パターン。

---

### 🔸 Q5

```java
int a = 0;
int b = 1;

if (!(a > 0) && (b == 1 || a + b > 2))   // !false && (true || ...) → true && true → true
    System.out.println("YES");
else
    System.out.println("NO");
```

✅ **正解：A. YES**

💡 **ポイント：**

・`!`（否定演算子）と `||`（論理和）の組み合わせ

・括弧を使って**演算の順序**を明確にすることが大切

---

## 🔍 補足：if文ネストと論理演算の見分けポイント

| 特徴 | 説明 |
| --- | --- |
| `if`と`else`の対応 | 「直前の`if`にだけ」対応する（波かっこ `{}` を使わないとバグを生みやすい） |
| `&&`（AND） | 左が`false`なら右は評価しない（短絡評価） |
| ` |  |
| `!`（NOT） | 複雑な条件を単純にしたいときに使う（例：`!(a > 0)` は `a <= 0` と同じ） |
| ネストの深さ | インデントに惑わされず、**構文ツリーを意識**することが重要 |