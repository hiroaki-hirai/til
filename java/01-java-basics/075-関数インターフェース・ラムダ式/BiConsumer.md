# BiConsumer

`BiConsumer` は Java の関数型インタフェースの1つで、**2つの引数を受け取って処理を行うが、戻り値を返さない関数**を表現するために使います。以下、詳しく解説します。

---

## 🔷 `BiConsumer` の基本構文

```java
@FunctionalInterface
public interface BiConsumer<T, U> {
    void accept(T t, U u);
}
```

- `T`：第1引数の型
- `U`：第2引数の型
- `accept(T t, U u)`：2つの引数を受け取って処理を行うメソッド

---

## 🔷 使用例（簡単な出力）

```java
import java.util.function.BiConsumer;

public class Main {
    public static void main(String[] args) {
        BiConsumer<String, Integer> printer = (name, age) -> {
            System.out.println(name + " is " + age + " years old.");
        };

        printer.accept("Hiroaki", 30);
    }
}
```

🟢 実行結果：

```java
Hiroaki is 30 years old.
```

---

## 🔷 主な利用用途

- **Mapのループ処理**
- **2つのパラメータを受けて処理するようなイベントハンドラ**
- **Builderや初期化処理などのカスタマイズ**

---

### 🔶 Mapと組み合わせた例

```java
Map<String, Integer> map = Map.of("apple", 2, "banana", 5);

map.forEach((key, value) -> System.out.println(key + " = " + value));
```

※ `Map#forEach()` は `BiConsumer<? super K, ? super V>` を引数に取ります。

---

### 🔶 カスタムロジックの注入

```java
public class Processor {
    public static <T, U> void executeWith(BiConsumer<T, U> bi, T t, U u) {
        bi.accept(t, u);
    }

    public static void main(String[] args) {
        executeWith((x, y) -> System.out.println(x + " & " + y), "Java", "Lambda");
    }
}
```

---

## 🔷 `andThen()` メソッドによる合成

`BiConsumer` はデフォルトメソッド `andThen()` を持っており、処理を合成できます。

```java
BiConsumer<String, Integer> first = (name, score) -> System.out.println(name + ": " + score);
BiConsumer<String, Integer> second = (name, score) -> {
    if (score >= 60) System.out.println("Passed");
    else System.out.println("Failed");
};

BiConsumer<String, Integer> combined = first.andThen(second);
combined.accept("Hiroaki", 75);
```

🟢 実行結果：

```java
Hiroaki: 75
Passed
```

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| インタフェース名 | `BiConsumer<T, U>` |
| メインメソッド | `void accept(T t, U u)` |
| 戻り値 | なし（`void`） |
| 主な使いどころ | 2引数の処理、MapのforEach、イベント処理、初期化処理 |
| 合成可能 | `andThen()` による後続処理の合成が可能 |

以下は **Java Gold 試験風の BiConsumer に関するパターン問題** です。

Lambda や Map、`andThen()` などの応用も含めています。

---

## 🔶【問題1】MapとBiConsumer

次のコードを実行した結果として**正しいもの**を1つ選びなさい。

```java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("Java", 90);
        map.put("Python", 85);

        BiConsumer<String, Integer> printer = (k, v) -> {
            if (v > 80) {
                System.out.println(k + ": High score");
            }
        };

        map.forEach(printer);
    }
}
```

### 選択肢：

A. 実行時エラーになる

B. 何も出力されない

C. `Java: High score` のみが出力される

D. `Java: High score` と `Python: High score` の2行が出力される

---

### ✅ 解答：**D**

**解説**：

- `map.forEach()` は `BiConsumer<String, Integer>` を受け取る
- `v > 80` の条件は Java=90, Python=85 の両方にマッチ
- よって、両方出力される

---

## 🔶【問題2】andThen() の合成

次のコードの実行結果として**正しいもの**を1つ選びなさい。

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        BiConsumer<String, Integer> log = (s, i) -> System.out.print(s + ":" + i + " ");
        BiConsumer<String, Integer> checker = (s, i) -> {
            if (i >= 50) System.out.print("Pass ");
            else System.out.print("Fail ");
        };

        BiConsumer<String, Integer> composed = log.andThen(checker);
        composed.accept("Math", 45);
    }
}
```

### 選択肢：

A. Math:45 Pass

B. Math:45 Fail

C. Pass Math:45

D. Fail Math:45

---

### ✅ 解答：**B**

**解説**：

- `andThen()` は2つの `BiConsumer` を順に呼び出す
- 最初に `log` が出力 → `Math:45`
- 次に `checker` が出力 → `Fail`
- 出力順に注意

---

## 🔶【問題3】BiConsumerの参照型としての使用

以下のコードについて**コンパイルエラーになる箇所はどこか**。

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        BiConsumer<String, Integer> c = Main::process;
        c.accept("Java", 100);
    }

    public static void process(String s, int i) {
        System.out.println(s + " scored " + i);
    }
}
```

### 選択肢：

A. `BiConsumer<String, Integer> c = Main::process;`

B. `process` メソッドの定義

C. `c.accept("Java", 100);`

D. すべて正しくコンパイル・実行できる

---

### ✅ 解答：**D**

**解説**：

- `BiConsumer<T, U>` は `(T, U)` を受け取る関数参照に使える
- `Main::process` は `(String, int)` なので OK（int は Integer にボクシング）

---

## 🔶【問題4】BiConsumer の null 安全性

次のコードの出力結果として**正しいもの**を選びなさい。

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        BiConsumer<String, String> printer = (k, v) -> System.out.println(k + "=" + v);

        Map<String, String> map = new HashMap<>();
        map.put("lang", "Java");
        map.put("version", null);

        map.forEach(printer);
    }
}
```

### 選択肢：

A. 実行時例外が発生する

B. lang=Java のみが出力される

C. version=null も出力される

D. コンパイルエラーになる

---

### ✅ 解答：**C**

**解説**：

- `Map#forEach(BiConsumer)` は `null` 値を許容する
- `version=null` のように出力される

---

## ✨ 総まとめ：Goldで問われやすい観点

| 出題テーマ | ポイント例 |
| --- | --- |
| 型推論 | `BiConsumer<String, Integer>` などの型パラメータの一致 |
| メソッド参照 | `Main::methodName` が `BiConsumer` と一致するか |
| andThenの合成 | 合成順序や例外のスロー順序にも注意 |
| null安全性 | `Map#forEach()` は null 値に寛容 |
| 参照 vs ラムダ | メソッド参照・匿名クラスとの相互変換の理解 |

以下に、**Java Goldレベルの BiConsumer に関する引っかけ問題（ひっかけポイント＋解説付き）を用意しました。
構文や仕様に対する表面的な理解では解けない**ように作っています。

---

## 🔶【引っかけ問題1】型推論ミス誘導

以下のコードを見て、**正しいものを選びなさい**。

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        BiConsumer printer = (String a, Integer b) -> System.out.println(a + ":" + b);
        printer.accept("Java", 17);
    }
}
```

### 選択肢：

A. コンパイルエラーになる

B. 実行時例外が発生する

C. `Java:17` と出力される

D. BiConsumer はラムダ式では使えない

---

### ❌ ひっかけポイント：

- ラムダ式に**明示的な型を書く場合は、両方の型を正しく一致させる必要がある**
- **raw型の BiConsumer** を使っている点に注意！

---

### ✅ 正解：**A**

**解説**：

- `BiConsumer` が **型指定されていない raw型** のため、ラムダ式に **型推論ができない**
- 明示的な型 `(String a, Integer b)` を指定しているが、raw型にラムダ式で型付き引数を使うのは **コンパイルエラー**
- 修正案：

```java
BiConsumer<String, Integer> printer = (a, b) -> System.out.println(a + ":" + b);
```

---

## 🔶【引っかけ問題2】メソッド参照の誤使用

以下のコードについて、**正しいものを選びなさい**。

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        BiConsumer<String, Integer> consumer = Main::show;
        consumer.accept("Lambda", 100);
    }

    public static void show(String name) {
        System.out.println(name);
    }
}
```

### 選択肢：

A. コンパイルエラーになる

B. 実行時に `NullPointerException` が出る

C. `Lambda` と出力される

D. `Main::show` は BiConsumer として有効な参照

---

### ❌ ひっかけポイント：

- `BiConsumer<T, U>` には **引数が2つ必要**
- メソッド参照は **メソッドシグネチャと完全一致** が必要

---

### ✅ 正解：**A**

**解説**：

- `BiConsumer<String, Integer>` は `(String, Integer)` を受け取る関数
- しかし `show(String)` は **引数が1つだけ**
- 引数数が一致しないので `Main::show` は使えない ⇒ コンパイルエラー

---

## 🔶【引っかけ問題3】andThen() の例外伝播

次のコードの動作について、**正しいものを選びなさい**。

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        BiConsumer<String, Integer> a = (x, y) -> System.out.println("A");
        BiConsumer<String, Integer> b = (x, y) -> {
            throw new RuntimeException("Error in B");
        };

        BiConsumer<String, Integer> ab = a.andThen(b);
        ab.accept("Test", 10);
    }
}
```

### 選択肢：

A. 何も出力されず例外がスローされる

B. "A" と出力された後、例外がスローされる

C. "Error in B" と出力されるだけ

D. andThen() により b は呼び出されない

---

### ❌ ひっかけポイント：

- `andThen()` は **前段が成功したら後段を呼ぶ**
- **後段で例外が発生するパターンの例外伝播確認**

---

### ✅ 正解：**B**

**解説**：

- `a.accept()` が実行 → `"A"` 出力
- `b.accept()` が続けて実行 → 例外発生
- よって `"A"` の出力の**後に例外**

---

## 🔶【引っかけ問題4】nullキー・null値処理

以下のコードの出力結果として**正しいものを選びなさい**。

```java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put(null, "NullKey");
        map.put("Java", null);

        BiConsumer<String, String> printer = (k, v) -> System.out.println(k + "=" + v);
        map.forEach(printer);
    }
}
```

### 選択肢：

A. コンパイルエラーになる

B. 実行時に NullPointerException が出る

C. `null=NullKey` と `Java=null` の両方が出力される

D. `Map#forEach()` は nullキーを許容しない

---

### ❌ ひっかけポイント：

- `null` キーと `null` 値の扱い
- `HashMap` は許容、`TreeMap` は許容しない点に注意

---

### ✅ 正解：**C**

**解説**：

- `HashMap` は `null` キー・値を1つまで許容
- `forEach(BiConsumer)` は `null` をそのまま渡す（処理側で NPE を投げない限り安全）

---

## ✅ まとめ：よくある Gold 的ひっかけポイント

| テーマ | よくある誤答例・注意点 |
| --- | --- |
| 型推論と raw型 | ラムダ式で明示型を使うなら、ジェネリック型の指定が必要 |
| メソッド参照の不一致 | 引数の数・型が完全一致していないと使えない |
| andThen の合成順序と例外 | 後段で例外 → 前段の処理は実行済、合成順にも注意 |
| null の扱い | `Map#forEach()` は nullキー/値を許容する（HashMap） |
| コンパイル vs 実行時の違い | コンパイル時の型チェックと実行時例外の違いを明確に理解 |