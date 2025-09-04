# interface と 抽象クラス

## 🔷 インターフェースとは？

インターフェースとは、**「メソッドの定義だけを持つ型」**です。実装は持たず、「こういう機能を提供するよ」という**契約（仕様）**を定めるものです。

### ✅ 主な目的

- クラス間の共通動作の定義
- 多態性（ポリモーフィズム）の実現
- 実装の分離と柔軟性の向上

---

## 🔷 基本構文

```java
public interface Animal {
    void eat(); // 抽象メソッド
    void sleep();
}
```

```java
public class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("Dog is eating.");
    }

    @Override
    public void sleep() {
        System.out.println("Dog is sleeping.");
    }
}
```

- `implements` キーワードを使ってクラスがインターフェースを実装します。
- 実装クラスはすべてのメソッドを **必ず** 実装しなければなりません。

---

## 🔷 インターフェースの特徴

| 特徴 | 内容 |
| --- | --- |
| フィールド | `public static final`（定数）のみ可能 |
| メソッド（Java 8以降） | `abstract`, `default`, `static`, `private` の4種が使える |
| 多重実装が可能 | クラスは複数のインターフェースを同時に `implements` できる |
| 継承 | インターフェース同士も `extends` で継承できる（多重継承可） |

---

## 🔷 メソッドの種類（Java 8以降）

| 修飾子 | 説明 |
| --- | --- |
| `abstract` | 通常の抽象メソッド。実装クラスが必ずオーバーライドする。 |
| `default` | 実装を持つ。実装クラスでオーバーライド可能。 |
| `static` | インターフェース名から直接呼び出せる静的メソッド。 |
| `private` | インターフェース内部のヘルパー用途（Java 9以降）。 |

---

## 🔷 インターフェースとクラスの違い

| 比較項目 | インターフェース | 抽象クラス |
| --- | --- | --- |
| 継承関係 | 多重実装可能 (`implements`) | 単一継承のみ (`extends`) |
| メソッド | 実装なし（defaultで可能に） | 実装ありも可能 |
| フィールド | 定数のみ (`public static final`) | 通常のフィールドもOK |
| 利用目的 | 契約の定義、実装の切り替え | 部分実装の提供、共通処理の共有 |

---

## 🔷 使用例：ポリモーフィズムとの関係

```java
public void makeItEat(Animal animal) {
    animal.eat(); // 実際の動作は、渡されたオブジェクト（DogやCat）により異なる
}
```

- このように、**共通インターフェースを引数にとることで、柔軟なメソッド設計が可能**になります。

---

## 🔷 よくある利用パターン

- 戦略パターン（Strategy Pattern）
- DI（依存性注入）と組み合わせて、切り替え可能な実装を用意する
- 複数の機能インターフェースを使って責任を分離（インターフェース分離原則）

# Q,

インターフェースにおけるprivate 修飾は、通常のアクセス修飾子と役割が異なる認識はあっていますか？

はい、**Hiroakiさんの認識は正しいです。**

インターフェースにおける `private` 修飾子は、**通常のクラスにおけるアクセス制御のためのものとは少し異なり**、**実装の共通化やカプセル化のために導入された特殊な使い方**です。

---

### ✅ 結論

> インターフェースにおける private メソッドは、実装の共通処理を隠蔽するためのものであり、外部や実装クラスからはアクセスできません。
> 
> 
> **通常のクラスでの「外部からアクセスを制限する」目的とは、少し異なる文脈で使用されます。**
> 

---

### 🔍 背景と違いのポイント

| 項目 | 通常のクラス | インターフェース（Java 9以降） |
| --- | --- | --- |
| `private` の意味 | 外部クラスや他のインスタンスからのアクセスを遮断 | **インターフェース内部の実装を隠すため**のユーティリティメソッド |
| 主な目的 | カプセル化（データ隠蔽） | **実装共有のための共通メソッドの隠蔽** |
| 使用可能な場所 | フィールド、メソッド、コンストラクタなど | **メソッドのみ（`static` または `default` 内からのみ呼び出せる）** |

---

### 🧪 例：インターフェースの private メソッド

```java
public interface MyInterface {
    
    default void greet() {
        System.out.println(getMessage());
    }

    private String getMessage() {
        return "Hello from private method!";
    }
}
```

- `getMessage()` は他のインターフェースメソッド（ここでは `greet()`）からのみ使えます。
- **実装クラスや外部からは絶対に呼び出せません。**

---

### 🎯 補足：なぜインターフェースに private が必要？

Java 9 以降、インターフェースにも `default` や `static` メソッドが使えるようになりましたが、それらの中で**共通処理をまとめたい**場合があります。

従来：

```java
default void methodA() {
    System.out.println("共通処理");
}

default void methodB() {
    System.out.println("共通処理");
}
```

→ 共通処理をまとめたい！

```java
default void methodA() {
    printCommon();
}

default void methodB() {
    printCommon();
}

private void printCommon() {
    System.out.println("共通処理");
}
```

---

### ✅ まとめ

| 観点 | クラスにおける `private` | インターフェースにおける `private` |
| --- | --- | --- |
| 主目的 | アクセス制御 | 実装共有のための内部メソッドの隠蔽 |
| 使える場所 | クラス全体（メンバ、コンストラクタなど） | **メソッドのみ（Java 9以降）** |
| 呼び出し元 | クラス内部 | **default や static メソッド内のみ** |

Java 9 以降の **インターフェースにおける `private static` と `private`（インスタンスメソッド）の使い分け**について、明確に整理して解説します

---

## 🔷 インターフェースにおける private メソッドの2種類

| 種類 | 宣言形式 | 特徴 | 呼び出せる場所 |
| --- | --- | --- | --- |
| `private` | `private void method()` | **インスタンスメソッド**の共通処理用 | `default` メソッド内からのみ |
| `private static` | `private static void method()` | **クラスメソッド的な共通処理**用 | `static` メソッド内からのみ |

---

## 🔍 違いと使い分けのポイント

| 観点 | `private` | `private static` |
| --- | --- | --- |
| **目的** | 複数の `default` メソッドの共通処理に使う | 複数の `static` メソッドの共通処理に使う |
| **使える場所** | `default` メソッド内のみ | `static` メソッド内のみ |
| **インスタンス参照 (`this`)** | 可能 | 不可能（static文脈のため） |
| **外部クラス/実装クラスからのアクセス** | 不可 | 不可（どちらも完全に非公開） |
| **引数が必要かどうかの設計** | `this` にアクセスできるので少なく済む | `this` なしで完結させる必要あり（明示的に引数を渡す） |

---

## 🧪 具体例で理解する

### ✔ `private`（インスタンスメソッド）

```java
public interface Greeting {

    default void greetInEnglish() {
        log("Hello");
    }

    default void greetInJapanese() {
        log("こんにちは");
    }

    private void log(String message) {
        System.out.println("[LOG] " + message);
    }
}
```

### ✅ 特徴

- `log()` は `default` メソッド内でだけ使用可能。
- `this` を使ったり、他のインスタンスメソッドを呼び出すことも可能。

---

### ✔ `private static`（静的メソッド）

```java
public interface MathUtil {

    static int square(int x) {
        logCall("square");
        return x * x;
    }

    static int cube(int x) {
        logCall("cube");
        return x * x * x;
    }

    private static void logCall(String methodName) {
        System.out.println("Calling method: " + methodName);
    }
}
```

### ✅ 特徴

- `logCall()` は `static` メソッド内だけで使える。
- インスタンスを必要としない純粋な処理に適している。

---

## 🎯 使い分け指針

| 状況 | 使用すべき修飾子 |
| --- | --- |
| インスタンスごとの処理で共通化したい（例：ログ、検証） | `private` |
| 静的な処理を共通化したい（例：ユーティリティ的な処理） | `private static` |

---

## 📝 まとめ

- **`private`** → `default` メソッドのための共通処理。インスタンスに依存。
- **`private static`** → `static` メソッドのための共通処理。完全に静的。
- **共通点** → どちらも **外部や実装クラスからは一切アクセス不可**。
    
    → インターフェースの内部実装をスッキリ整理するための構造。
    

以下に、Java Silver試験風の「インターフェースに関するパターン問題」とその**選択肢・解説**を用意しました。実際の試験に近い構文・引っかけ・選択肢構成になっています。

---

## 🧠 インターフェース：Java Silver風パターン問題①

### 【問題1】

次のコードをコンパイルして実行したときの出力として正しいものを選びなさい。

```java
interface Flyer {
    void fly();
}

class Bird implements Flyer {
    public void fly() {
        System.out.println("Bird is flying");
    }
}

public class Main {
    public static void main(String[] args) {
        Flyer f = new Bird();
        f.fly();
    }
}
```

### 【選択肢】

A. Bird is flying

B. コンパイルエラー

C. 実行時例外

D. 何も表示されない

### ✅ 正解：**A**

### ✅ 解説：

- `Bird` クラスは `Flyer` インターフェースを `implements` しており、`fly()` メソッドを正しくオーバーライド。
- `Flyer f = new Bird();` により、**インターフェース型の変数に実装クラスのインスタンスが代入**され、ポリモーフィズムが成立。
- `f.fly();` によって `Bird` クラスの `fly()` が呼ばれる。

---

## 🧠 インターフェース：Java Silver風パターン問題②

### 【問題2】

次のコードについて正しい記述を選びなさい。

```java
interface A {
    void methodA();
}

interface B extends A {
    void methodB();
}

class C implements B {
    public void methodA() {
        System.out.println("A");
    }

    public void methodB() {
        System.out.println("B");
    }
}
```

### 【選択肢】

A. インターフェースは他のインターフェースを継承できない

B. クラスCはmethodAだけ実装すればよい

C. インターフェースBを実装する場合、Aのメソッドも実装しなければならない

D. クラスCはインターフェースを継承している

### ✅ 正解：**C**

### ✅ 解説：

- インターフェースは他のインターフェースを `extends` できる（**Aは誤り**）。
- `interface B extends A` によって、Bを実装するということは、**Aのメソッドも実装しなければならない**。
- `implements` はインターフェースの実装であり、継承ではない（**Dは誤り**）。

---

## 🧠 インターフェース：Java Silver風パターン問題③（defaultメソッド）

### 【問題3】

次のコードを実行した場合の出力として正しいものを選びなさい。

```java
interface Greeter {
    default void greet() {
        System.out.println("Hello from interface");
    }
}

class MyGreeter implements Greeter {
    public void greet() {
        System.out.println("Hello from class");
    }
}

public class Main {
    public static void main(String[] args) {
        Greeter g = new MyGreeter();
        g.greet();
    }
}
```

### 【選択肢】

A. Hello from interface

B. Hello from class

C. コンパイルエラー

D. 実行時例外

### ✅ 正解：**B**

### ✅ 解説：

- `interface` 内に `default` メソッドを定義している → Java 8以降の文法。
- 実装クラスが `greet()` を**オーバーライド**しているため、`MyGreeter` の実装が呼び出される。

---

## 🧠 インターフェース：Java Silver風パターン問題④（staticメソッド）

### 【問題4】

次のコードをコンパイルして実行したときの結果として正しいものを選びなさい。

```java
interface Util {
    static void print() {
        System.out.println("Static method");
    }
}

public class Main {
    public static void main(String[] args) {
        Util.print();
    }
}
```

### 【選択肢】

A. Static method

B. コンパイルエラー：staticメソッドは呼び出せない

C. コンパイルエラー：インスタンスから呼び出す必要がある

D. 実行時例外

### ✅ 正解：**A**

### ✅ 解説：

- `interface` の `static` メソッドは、**インターフェース名から直接呼び出す**。
- `Util.print();` という呼び方は正しく、出力は `"Static method"`。

---

## 📌 総括：出題ポイントまとめ

| ポイント | 出題の狙い例 |
| --- | --- |
| implements必須メソッド実装 | 抽象メソッドが残っているとコンパイルエラー |
| インターフェースの継承 | extendsキーワードで多重継承可能 |
| defaultメソッド | 実装を省略可能、上書きも可能 |
| staticメソッド | インスタンスからではなくインターフェース名で呼び出す |

## 🧠 インターフェース：パターン問題⑤（フィールドの扱い）

### 【問題5】

次のコードをコンパイルして実行するとどうなりますか？

```java
interface Constants {
    int VALUE = 100;
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Constants.VALUE);
        // Constants.VALUE = 200; // ★この行があるとき
    }
}
```

### 【選択肢】

A. 100 と表示される

B. コンパイルエラー：定数に再代入しようとしている

C. 実行時エラー：初期化されていない

D. 200 と表示される

### ✅ 正解：**A**（※★の行がコメントアウトされている場合）

### ✅ 正解：B（※★の行がコメントアウトされていない場合）

### ✅ 解説：

- `interface` 内のフィールドは自動的に `public static final` → **定数扱い**。
- 再代入しようとすると**Bのようにコンパイルエラー**になる。
- 表示自体は、`Constants.VALUE` でOK。

---

## 🧠 インターフェース：パターン問題⑥（多重実装）

### 【問題6】

次のコードを見て、正しく動作する組み合わせを選びなさい。

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

class MyClass implements A, B {
    public void methodA() {
        System.out.println("A");
    }
    public void methodB() {
        System.out.println("B");
    }
}
```

### 【選択肢】

A. Javaではクラスは複数のインターフェースを実装できない

B. MyClassのように複数のインターフェースを `implements` するのはOK

C. `class MyClass implements A extends B` と書き換えられる

D. インターフェースを継承するには `implements` を使う

### ✅ 正解：**B**

### ✅ 解説：

- Javaでは**クラスの多重継承は禁止**されているが、**インターフェースの多重実装は可能**。
- `implements A, B` は正しい構文。
- `interface` の継承は `extends`、`implements` はクラスがインターフェースを実装する場合に使用（Dは誤り）。

---

## 🧠 インターフェース：パターン問題⑦（defaultメソッドの衝突）

### 【問題7】

次のコードはコンパイルされますか？

```java
interface X {
    default void hello() {
        System.out.println("Hello from X");
    }
}

interface Y {
    default void hello() {
        System.out.println("Hello from Y");
    }
}

class Z implements X, Y {
    // ?
}
```

### 【選択肢】

A. コンパイルされる（ZはそのままでOK）

B. コンパイルエラーになる

C. Zクラスは `hello()` を `super.hello()` で呼べばOK

D. インターフェースの default メソッドは衝突してもエラーにならない

### ✅ 正解：**B**

### ✅ 解説：

- 複数のインターフェースから **同じシグネチャの default メソッド** を継承すると**衝突**とみなされる。
- コンパイルエラーとなるため、Zクラス側で明示的にどちらを使うか選択する必要あり：

```java
public void hello() {
    X.super.hello(); // 明示的にどちらかを選ぶ
}
```

---

## 🧠 インターフェース：パターン問題⑧（匿名クラス）

### 【問題8】

次のコードの出力として正しいものを選びなさい。

```java
interface Runnable {
    void run();
}

public class Main {
    public static void main(String[] args) {
        Runnable r = new Runnable() {
            public void run() {
                System.out.println("Running anonymously");
            }
        };
        r.run();
    }
}
```

### 【選択肢】

A. コンパイルエラー：インターフェースはインスタンス化できない

B. Running anonymously

C. 実行時エラー

D. 何も表示されない

### ✅ 正解：**B**

### ✅ 解説：

- `new Runnable() { ... }` は**匿名クラス（anonymous class）**の記法。
- 実際にはインターフェースを実装した**無名の内部クラスのインスタンス**を生成している。
- 実行すると `run()` の中身が出力される。

---

## 📌 補足：Java Silverで出題されやすい構文ポイント

| 出題パターン | 注目点 |
| --- | --- |
| インターフェースのフィールド | 自動で `public static final` |
| 抽象メソッドの実装義務 | すべてオーバーライド必須 |
| default/static/private の扱い | Java 8 以降の特徴 |
| 複数のインターフェース | `implements` でOK |
| 衝突や曖昧さの明示的解決 | `X.super.method()` の使用 |

[interface](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/interface%2022062cef735080ad8fb0de794fe53b85.md)

[default メソッド](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/default%20%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%2020c62cef73508031be4df7e8f4787650.md)

[抽象メソッドのオーバーライド](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/%E6%8A%BD%E8%B1%A1%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%AE%E3%82%AA%E3%83%BC%E3%83%90%E3%83%BC%E3%83%A9%E3%82%A4%E3%83%89%2020c62cef735080c8bcc1c1610e399be9.md)

[抽象メソッド インスタンス化](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/%E6%8A%BD%E8%B1%A1%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%20%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E5%8C%96%2025262cef735080068e92f1ab29efe877.md)

[意味的契約](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/%E6%84%8F%E5%91%B3%E7%9A%84%E5%A5%91%E7%B4%84%2020c62cef735080abbab0eaa5a5023a71.md)

[interface 抽象クラス 比較](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/interface%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%20%E6%AF%94%E8%BC%83%2021362cef735080e59888c77628ac0534.md)

[テンプレートメソッド＋コンストラクタ初期化](interface%20%E3%81%A8%20%E6%8A%BD%E8%B1%A1%E3%82%AF%E3%83%A9%E3%82%B9%201fc62cef735080e1b891d56d3e1b7494/%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%EF%BC%8B%E3%82%B3%E3%83%B3%E3%82%B9%E3%83%88%E3%83%A9%E3%82%AF%E3%82%BF%E5%88%9D%E6%9C%9F%E5%8C%96%2021362cef73508020912af69a0383285c.md)