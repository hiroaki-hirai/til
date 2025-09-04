# return 到達判定

以下に、Javaで「`return` 文の後に記述されたコード」が**到達可能かどうかを判定する代表的なパターン例**を整理してお見せします。

---

## ✅ パターン別：到達可能かどうか一覧

| パターン | 到達可能か？ | 説明 |
| --- | --- | --- |
| `return` 直後に文を書く | ❌ 到達不能 | コンパイルエラー |
| `if` 文で分岐 | ✅ 到達可能 | 条件により実行される可能性がある |
| `if (true)` の後に return | ❌ 到達不能 | 実質的に return しかない |
| `while(true)` で return | ❌ 到達不能 | return でループが必ず終わる |
| `for` 文で return に至らない可能性あり | ✅ 到達可能 | 条件によって return に至らない |
| `try-catch-finally` 構造内の return | ❌ / ✅ | finally の後は原則到達不能 |

---

## 🔍 各パターンのコード例

### ❌ パターン①：returnのあとに文を書く（到達不能）

```java
public int test1() {
    return 1;
    System.out.println("これはエラー"); // 到達不能 → コンパイルエラー
}
```

---

### ✅ パターン②：if文による分岐（到達可能）

```java
public int test2(boolean flag) {
    if (flag) {
        return 1;
    }
    System.out.println("flagがfalseのときはここに到達可能"); // OK
    return 2;
}
```

---

### ❌ パターン③：if (true) の return（到達不能）

```java
public int test3() {
    if (true) {
        return 1;
    }
    System.out.println("これは到達不能"); // エラー
}
```

---

### ❌ パターン④：while(true) で必ず return（到達不能）

```java
public int test4() {
    while (true) {
        return 1;
    }
    System.out.println("これは到達不能"); // エラー
}
```

---

### ✅ パターン⑤：for文で条件によっては return されない場合（到達可能）

```java
public int test5() {
    for (int i = 0; i < 5; i++) {
        if (i == 2) return 2;
    }
    System.out.println("ループがreturnしなければ到達可能"); // OK
    return 0;
}
```

---

### ❌ パターン⑥：try-finally内のreturn後（到達不能）

```java
public int test6() {
    try {
        return 1;
    } finally {
        System.out.println("finally内の文"); // OK
    }
    System.out.println("これは到達不能"); // エラー
}
```

---

## ✅ チェックポイント（補足）

- Javaでは**到達不能コードを静的に検出し、コンパイルエラーにします**（`unreachable statement`）。
- `return` / `throw` / 無限ループ（`while(true)`）+ `break` なし などが直前にあると、その後のコードはエラーになる可能性が高いです。

では次に、「**`switch文` や `ラムダ式` の return に関する到達不能コードのパターン**」について、代表例を交えて整理します。

---

## 🧭 Part 2: `switch文` と `ラムダ式` における `return` の到達性パターン

---

### 🔷 `switch` 文における `return` の到達性

### ✅ パターン①：各 `case` に `return`、`default` なし → 到達可能

```java
public int testSwitch1(int x) {
    switch (x) {
        case 1: return 10;
        case 2: return 20;
    }
    System.out.println("switchがどのcaseにも当てはまらなければ実行される"); // OK
    return 0;
}
```

❌ パターン②：全 `case` と `default` に `return` があり、その後に文を書く

```java
public int testSwitch3(int x) {
    switch (x) {
        case 1:
            return 100;
        case 2:
            break;
        default:
            System.out.println("default節実行");
    }
    return 0; // OK（全てのパスが return ではない）
}
```

✅ パターン③：`break` と `return` 混在 → `return` 後のコード注意

```java
public int testSwitch3(int x) {
    switch (x) {
        case 1:
            return 100;
        case 2:
            break;
        default:
            System.out.println("default節実行");
    }
    return 0; // OK（全てのパスが return ではない）
}
```

---

### 🔷 ラムダ式における `return` の扱い

### ✅ パターン①：式形式（戻り値がある）→ `return` 不要

```java
Function<Integer, Integer> func1 = x -> x + 1; // OK（式のみ）
```

✅ パターン②：ブロック形式で `return` 使用 → OK

```java
Function<Integer, Integer> func2 = x -> {
    return x + 1;
}
```

❌ パターン③：`return` のあとに文を置く → 到達不能

```java
Function<Integer, Integer> func3 = x -> {
    return x + 1;
    // System.out.println("これは到達不能"); // コンパイルエラー
};
```

---

## 📌 補足：ラムダ式のスコープと `return`

- `return` は**ラムダ式内の処理ブロックからの脱出**を意味します。
- **`return` 後に書いた文は絶対に実行されないため、同様に到達不能としてエラーになります。**

---

## 🔚 まとめ：`switch`・`ラムダ` 到達性チェックポイント

| 文脈 | `return` 後に記述可能？ | 備考 |
| --- | --- | --- |
| switch文全体が全て return で閉じられている | ❌ NG | defaultあり return完結型 |
| switch文に未網羅のパスがある場合（e.g., case漏れ） | ✅ OK | return後に文があっても良い |
| ラムダ式のブロック内で return した後に文を書く | ❌ NG | 到達不能になる |

では続けて、**Java 14以降の `switch式`** や、**`コンストラクタ` の中での `return`** における「到達不能コード」の扱いを解説します。

---

## 🧭 Part 3: `switch式` と `コンストラクタ` における `return` の到達性パターン

---

### 🔷 `switch式`（Java 14以降）

Java 14 から導入された `switch` は **式** として使用できます。

この形式では、**各分岐が `return` ではなく `→` または `yield` を使って値を返す**ため、少し性質が異なります。

---

### ✅ パターン①：switch式の値を返す（すべて網羅されていない）→ 到達可能

```java
public int testSwitchExpr1(String color) {
    int result = switch (color) {
        case "red" -> 1;
        case "blue" -> 2;
        default -> 0;
    };
    System.out.println("switch式の後は到達可能"); // OK
    return result;
}
```

---

### ❌ パターン②：switch式が全ての分岐で `yield` or `→` で値を返して return する形、後続に文を書く → 到達不能

```java
public int testSwitchExpr2(String color) {
    return switch (color) {
        case "red" -> 1;
        case "blue" -> 2;
        default -> 0;
    };
    System.out.println("これは到達不能"); // ❌ コンパイルエラー
}
```

---

### 🧩 補足：`yield` と `→` の違い（簡略）

```java
// 複数文を記述したいときは yield を使う
int result = switch (value) {
    case 1 -> 10;
    case 2 -> {
        System.out.println("複数文");
        yield 20; // 値を返す
    }
    default -> 0;
};
```

---

### 🔷 コンストラクタ内の `return`

Javaの**コンストラクタでは、戻り値を返す `return` は書けません**。

ただし、**`return;`（値なしの return）を使って早期リターンすることは可能です**。

---

### ✅ パターン①：コンストラクタ内で条件によって `return;`（早期終了）

```java
public class Person {
    public Person(String name) {
        if (name == null) {
            System.out.println("名前がnullのため中断");
            return; // OK
        }
        System.out.println("名前：" + name);
    }
}
```

---

### ❌ パターン②：`return` 後にコードを書く → 到達不能

```java
public class Person {
    public Person(String name) {
        return;
        // System.out.println("これは到達不能"); // ❌ エラー
    }
}
```

---

### ❌ パターン③：値を返す `return` を書く → 文法エラー

```java
public class Person {
    public Person(String name) {
        // return name; // ❌ エラー：コンストラクタは値を返せない
    }
}
```

---

## 🧩 補足：メソッド vs コンストラクタの `return`

| 区分 | `return 値あり` | `return 値なし` | 備考 |
| --- | --- | --- | --- |
| メソッド | ✅ OK | ✅ OK | 戻り値型による制御 |
| コンストラクタ | ❌ エラー | ✅ OK | 戻り値は持てない |

---

## ✅ まとめ（Part 3）

| 文脈 | 到達不能コードの可能性 | 備考 |
| --- | --- | --- |
| switch式（Java14以降）で return 直後に文を書く | ❌ エラー | switch式がreturnされると後は到達不能 |
| switch式の評価後の文 | ✅ OK | return しなければ問題なし |
| コンストラクタ内で `return;` 後に文を書く | ❌ エラー | 通常のメソッドと同様に扱われる |
| コンストラクタで `return 値;` を書く | ❌ エラー | 文法的に禁止 |