# switch文 まとめ

以下に、Javaの`switch`文に関する**基本〜トリック含む応用パターン問題集（選択式）**を作成しました。Silver試験や実務でつまずきやすいポイントを押さえています。

---

## 🎯 `switch`文 パターン問題集（基本～応用10問）

---

### 🔸 Q1：出力は？

```java
int x = 2;
switch (x) {
    case 1:
        System.out.println("A");
    case 2:
        System.out.println("B");
    case 3:
        System.out.println("C");
}
```

✅ **正解：B. BC**

💡 **ポイント：** `break`がないと**後続のcaseも実行**される（= fall-through）。

---

### 🔸 Q2

```java
int x = 4;
switch (x) {
    case 1:
        System.out.println("One");
        break;
    case 2:
        System.out.println("Two");
    default:
        System.out.println("Default");
}
```

✅ **正解：A. Default**

💡 **ポイント：** `default`は**どこに書いてあっても最終的に実行対象**になる。

`case 2`の`break`がないのも注意点。

---

### 🔸 Q3

```java
char grade = 'B';
switch (grade) {
    case 'A':
    case 'B':
    case 'C':
        System.out.println("Pass");
        break;
    case 'D':
        System.out.println("Fail");
}
```

✅ **正解：A. Pass**

💡 **ポイント：** 連続caseは**共通処理**に使える（A/B/C → Pass）。

---

### 🔸 Q4

```java
String day = "MON";
switch (day) {
    case "MON":
        System.out.println("Start");
        break;
    case "FRI":
        System.out.println("Finish");
        break;
    default:
        System.out.println("Midweek");
}
```

✅ **正解：A. Start**

💡 **ポイント：** Java 7以降、`String`型も`switch`可能。

---

### 🔸 Q5

```java
int x = 3;
switch (x) {
    case 1 -> System.out.println("A");
    case 2 -> System.out.println("B");
    case 3 -> System.out.println("C");
    default -> System.out.println("D");
}
```

✅ **正解：A. C**

💡 **ポイント：** Java 14以降の「**switch式構文**（アロー構文）」

ブロックなし、`break`不要で簡潔！

---

### 🔸 Q6

```java
int x = 5;
switch (x) {
    default:
        System.out.println("Default");
    case 1:
        System.out.println("One");
    case 2:
        System.out.println("Two");
}
```

✅ **正解：B. DefaultOneTwo**

💡 **ポイント：** `default`が先にあっても**fall-throughは発生**する。

---

### 🔸 Q7

```java
final int A = 1;
int x = 1;
switch (x) {
    case A:
        System.out.println("OK");
}
```

✅ **正解：A. OK**

💡 **ポイント：** `switch-case`では`case`の定数は`final`ならOK（定数式）。

---

### 🔸 Q8

```java
byte b = 10;
switch (b) {
    case 10:
        System.out.println("Ten");
        break;
    case 128:
        System.out.println("Too big");
        break;
}
```

✅ **正解：C. コンパイルエラー**

```java
byte型の範囲： -128 ～ 127
→ case 128 は範囲外 → エラー
```

💡 **ポイント：** `case`に使う値は**変数の型に収まる必要がある**。

---

### 🔸 Q9

```java
Integer x = 1;
switch (x) {
    case 1:
        System.out.println("One");
        break;
    default:
        System.out.println("Other");
}
```

✅ **正解：A. One**

💡 **ポイント：** `switch`は`Integer`などのラッパー型も**オートアンボクシング対応**。

---

### 🔸 Q10

```java
String color = null;
switch (color) {
    case "RED":
        System.out.println("Red");
        break;
    default:
        System.out.println("None");
}
```

✅ **正解：D. 実行時エラー**

```java
color == null → switch(null) → NullPointerException 発生
```

💡 **ポイント：** `null`を`switch`に渡すと**実行時エラー（NPE）**になる。

→ 対策：事前に`null`チェックを！

---

## 🧩 総まとめ：switch文の注意点一覧

| 項目 | 内容 |
| --- | --- |
| `break`の省略 | fall-throughが発生し、後続caseも実行される |
| `default`の位置 | どこに書いてもOK（条件不一致時に実行） |
| 許可される型 | `byte`, `short`, `char`, `int`, `enum`, `String`（Java 7以降）、一部ラッパークラス |
| `final`定数 | `case`では`final`な定数式なら使用可能 |
| Java 14+ | `switch`式（アロー構文）でシンプルに書ける |
| `null`の扱い | `switch(null)` は **実行時例外（NPE）** になるので要注意 |

それでは、Javaの**従来の `switch文`（break/fall-through使用）に限定した**「応用パターン問題集（トリック・実務寄り含む）」をご用意しました。

---

## 🧠 `switch文` 応用パターン問題集（全7問）

---

### 🔸 Q1：出力は？

```java
int value = 3;
switch (value) {
    case 1:
    case 2:
        System.out.println("Low");
        break;
    case 3:
    case 4:
    case 5:
        System.out.println("Mid");
        break;
    default:
        System.out.println("Other");
}
```

✅ **正解：B. Mid**

💡 **ポイント：**

複数`case`をひとまとめにして、**1つの処理を共有**可能。

---

### 🔸 Q2

```java
int value = 4;
switch (value) {
    case 3:
        System.out.println("Three");
    case 4:
        System.out.println("Four");
    case 5:
        System.out.println("Five");
        break;
    default:
        System.out.println("Other");
}
```

✅ **正解：B. FourFive**

💡 **ポイント：**

`case 3`に一致しなくても、**case 4以降が実行される**。

`break`がないと**連続実行**になる。

---

### 🔸 Q3

```java
int x = 10;
switch (x) {
    default:
        System.out.println("Default");
        break;
    case 10:
        System.out.println("Ten");
        break;
}
```

✅ **正解：A. Ten**

💡 **ポイント：**

`default`が先にあっても、**一致したcaseが優先される**。

---

### 🔸 Q4

```java
int x = 2;
switch (x) {
    case 1:
        System.out.println("One");
    case 2:
        System.out.println("Two");
    case 3:
        System.out.println("Three");
        break;
}
```

✅ **正解：A. TwoThree**

💡 **ポイント：**

`break`がないと**次のcase文も実行**される（fall-throughトリック）。

---

### 🔸 Q5

```java
int y = 0;
switch (y + 2) {
    case 1:
        System.out.println("One");
        break;
    case 2:
        System.out.println("Two");
    case 3:
        System.out.println("Three");
        break;
}
```

✅ **正解：A. TwoThree**

💡 **ポイント：**

`switch(式)`は計算可能。

`break`の有無により**流れが変化**。

---

### 🔸 Q6

```java
final int A = 1;
final int B = 2;
int num = 2;
switch (num) {
    case A:
        System.out.println("A");
        break;
    case B:
        System.out.println("B");
        break;
}
```

✅ **正解：B. B**

💡 **ポイント：**

`case`には**コンパイル時定数（final）**を使える。

（`非final`ではコンパイルエラーになる）

---

### 🔸 Q7

```java
int a = 10;
int b = 20;
switch (a) {
    case 10:
        System.out.println("A");
        b = 30;
    case 20:
        System.out.println("B");
}
System.out.println(b);
```

✅ **正解：A. AB30**

📊 **図解：**

```java
a == 10 → case 10 に一致
↓
System.out.println("A");
b = 30;
↓（fall-through）
case 20 → 条件一致しなくても実行される
↓
System.out.println("B");
System.out.println(b); → 30
```

💡 **ポイント：**

caseに一致後、**breakがないと全て実行**。

変数`b`は変更後の値を出力。

---

## 🔍 応用的注意点まとめ

| トリック | 説明 |
| --- | --- |
| `break`の省略 | fall-throughで下のcaseも実行される |
| `case`の計算式 | `switch(式)`に加算・変数使用可（ただし定数扱い） |
| `default`の位置 | どこでも良い。優先順位は一致するcaseが上 |
| `final`の使用 | `case`に変数を使うには`final`である必要あり |
| 再代入の影響 | `switch`内で変数変更後に、外部出力にも影響する |

こちらは **Java 14以降で導入された「switch式（アロー構文）」に特化したパターン問題集** です。従来のswitch文との違い（戻り値、break不要、複数ラベルなど）を活かした内容になっています。

---

## ⚡ `switch式` パターン問題集（Java 14+対応）

※ すべて `switch (変数) -> 処理;` または `yield` を使用する構文です。

---

### 🔸 Q1：出力は？

```java
int day = 2;
String result = switch (day) {
    case 1 -> "Mon";
    case 2 -> "Tue";
    case 3 -> "Wed";
    default -> "Other";
};
System.out.println(result);
```

✅ **正解：B. Tue**

🔍 **構文ポイント：**

- `>` は「switch式」のアロー構文
- 値をそのまま返す（break不要）
- `switch式`は**戻り値を変数へ代入**できるのが特徴

---

### 🔸 Q2

```java
int score = 85;
String grade = switch (score / 10) {
    case 10, 9 -> "A";
    case 8 -> "B";
    case 7 -> "C";
    default -> "D";
};
System.out.println(grade);
```

✅ **正解：B. B**

🔍 **構文ポイント：**

- `case 10, 9 ->` のように **複数ラベルをカンマで並べられる**
- 式として評価されるので `score / 10 = 8` に一致

---

### 🔸 Q3

```java
String status = "READY";
String msg = switch (status) {
    case "READY" -> {
        System.out.println("Preparing...");
        yield "Ready to go";
    }
    case "RUNNING" -> "Working";
    default -> "Unknown";
};
System.out.println(msg);
```

✅ **正解：A. Preparing... Ready to go**

🔍 **構文ポイント：**

- `switch式`で複数文を実行したい場合は**`{}` + `yield`*を使う
- `System.out.println`は副作用、`yield`は**値の返却**

---

### 🔸 Q4

```java
int x = 3;
String result = switch (x) {
    case 1 -> "One";
    case 2 -> {
        break "Two"; // ← これが問題
    }
    default -> "Other";
};
```

✅ **正解：B. コンパイルエラー**

🔍 **構文ポイント：**

- `switch式`では**`break`は使えない**
- ブロック内で値を返すには必ず `yield` を使用する必要がある

---

### 🔸 Q5

```java
int n = 5;
String result = switch (n % 2) {
    case 0 -> "Even";
    case 1 -> "Odd";
    default -> throw new IllegalStateException("Unexpected");
};
System.out.println(result);
```

✅ **正解：B. Odd**

🔍 **構文ポイント：**

- `switch式`では **`default -> throw Exception`** も使用可能
- `n % 2 == 1` → case 1 に一致 → "Odd" が返る

---

### 🔸 Q6

```java
int age = 20;
String msg = switch (age) {
    case 20 -> {
        System.out.print("Age is 20. ");
        yield "Adult";
    }
    default -> "Unknown";
};
System.out.println(msg);
```

✅ **正解：A. Age is 20. Adult**

🔍 **構文ポイント：**

- ブロック内で複数の処理を書くときは **必ず`yield`で値を返す**
- `System.out.print()` は**副作用としてその場で出力**

---

## 🧠 `switch式` vs 従来の`switch文` 比較まとめ

| 比較項目 | switch文（従来） | switch式（Java 14+） |
| --- | --- | --- |
| 戻り値 | 返せない（void） | 返せる（代入できる） |
| break | 必須（しないとfall-through） | 不要（fall-throughしない） |
| 複数文処理 | 普通に書ける | `{}`と`yield`が必要 |
| ラベル複数 | `case A:`複数記述 | `case A, B ->`と簡潔 |
| default | 同じ | 同じ（`->`または`throw`も可） |

以下にご紹介するのは、**Java 14以降の「switch式」の応用パターン問題集**です。以下のようなシチュエーションを含めた設問構成になっています：

- `yield`を使った複数文処理
- `default -> throw`の例外制御
- 入れ子の`switch式`
- `enum`との組み合わせ
- 値を返す`switch式`の活用（条件分岐による変数代入）

---

## 🚀 `switch式` 応用パターン問題集（5問）

---

### 🔸 Q1

```java
int hour = 15;
String period = switch (hour) {
    case 6, 7, 8, 9, 10, 11 -> "Morning";
    case 12, 13, 14, 15, 16 -> "Afternoon";
    case 17, 18, 19 -> "Evening";
    default -> "Night";
};
System.out.println(period);
```

✅ **正解：B. Afternoon**

🔍 **構文ポイント：**

- `case`に複数の値をカンマで記述可
- `hour == 15` → `case 12〜16` にマッチ → "Afternoon"が返る

---

### 🔸 Q2

```java
String level = "high";
int priority = switch (level) {
    case "low" -> 1;
    case "medium" -> 2;
    case "high" -> {
        System.out.print("Alert! ");
        yield 3;
    }
    default -> throw new IllegalArgumentException("Unknown level");
};
System.out.println(priority);
```

✅ **正解：B. Alert! 3**

🔍 **構文ポイント：**

- `yield`はブロックからの返却に使用
- `System.out.print()` は副作用で先に出力される

---

### 🔸 Q3

```java
enum Status { READY, RUNNING, FAILED }

Status s = Status.FAILED;
String result = switch (s) {
    case READY -> "Standby";
    case RUNNING -> "Working";
    case FAILED -> "Retrying";
};
System.out.println(result);
```

✅ **正解：C. Retrying**

🔍 **構文ポイント：**

- `enum`型は`switch式`と**非常に相性が良い**
- 必ず全ての列挙値をカバーするか、`default`を書く

---

### 🔸 Q4

```java
int x = 2, y = 3;
String result = switch (x) {
    case 1 -> "One";
    case 2 -> switch (y) {
        case 3 -> "Two-Three";
        default -> "Two-Other";
    };
    default -> "Other";
};
System.out.println(result);
```

✅ **正解：B. Two-Three**

🔍 **構文ポイント：**

- `switch式`は**入れ子（ネスト）も可能**
- 外側の`switch(x)`で case 2 に一致 → 内側の switch(y) へ移行

---

### 🔸 Q5

```java
int score = 75;
char grade = switch (score / 10) {
    case 10, 9 -> 'A';
    case 8 -> 'B';
    case 7 -> 'C';
    case 6 -> 'D';
    default -> 'F';
};

String message = switch (grade) {
    case 'A', 'B' -> "Great job!";
    case 'C' -> "Good effort.";
    case 'D' -> "Needs improvement.";
    case 'F' -> "Failed.";
    default -> "Invalid.";
};
System.out.println(message);
```

✅ **正解：B. Good effort.**

🔍 **構文ポイント：**

- `switch式`を**連携して使うパターン**（値を中間で受け渡す）
- `score = 75 → grade = 'C' → message = "Good effort."`

---

## 🧩 応用でよく使われるswitch式パターン一覧

| パターン | 説明例 |
| --- | --- |
| 値のグルーピング | `case 1, 2, 3 ->` のように一括処理 |
| yieldによる返却 | 複数文を処理し、`yield`で返却 |
| 入れ子switch式 | 条件分岐の中にさらにswitch式（ifの代替） |
| enumとの連携 | `enum`値を安全に分岐処理 |
| 例外スロー | `default -> throw new Exception()`で明示的なエラー制御 |

Javaの `switch文`（従来構文）において、**条件式に使用できる型／使用できない型**を以下にまとめます。

---

## ✅ `switch文`で使える／使えない型一覧

### ✔ 使用できる型（条件式に入れられる）

| 種類 | 型 |
| --- | --- |
| 整数系プリミティブ型 | `byte`, `short`, `char`, `int` |
| ラッパークラス（Java 7以降） | `Byte`, `Short`, `Character`, `Integer` |
| 文字列（Java 7以降） | `String` |
| 列挙型（enum） | `Enum`型（例：`Day.MONDAY` など） |

---

### ❌ 使用できない型（条件式に**入れられない**）

| 種類 | 型 | 理由 |
| --- | --- | --- |
| 浮動小数点型 | `float`, `double` | 精度の問題により定数比較が不安定 |
| 論理型 | `boolean` | `true` / `false` は2値しかないため `if` で十分 |
| long型 | `long` | `switch`は内部的にintに変換して処理するため不可 |
| 参照型（例） | `List`, `Map` など | 比較できない、`equals()`では動かない |

---

## 🔍 まとめ表

| 型名 | 使用可否 | 備考 |
| --- | --- | --- |
| `int` | ✅ | 基本型 |
| `char` | ✅ | 文字コードで比較 |
| `String` | ✅ | Java 7以降 |
| `enum` | ✅ | 実務でも多用される |
| `Integer` | ✅ | オートアンボクシングされる |
| `float`, `double` | ❌ | 計算誤差があるため |
| `boolean` | ❌ | `if-else`で代替 |
| `long` | ❌ | 範囲外で処理困難 |