# record まとめ

Javaの `record`（レコード）は、Java 14でプレビュー導入され、**Java 16から正式に導入**された機能です。

**主にデータの受け渡しや集約**を目的とする場合に使う、**シンプルで不変（immutable）なデータキャリア**の構文です。

---

## 🔷 基本定義

```java
public record Person(String name, int age) {}
```

これは、次のようなクラスを **自動生成したものと等価**です：

```java

public final class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String name() { return name; }
    public int age() { return age; }

    @Override public boolean equals(Object o) { ... }
    @Override public int hashCode() { ... }
    @Override public String toString() { ... }
}
```

---

## ✅ 特徴まとめ

| 特徴 | 説明 |
| --- | --- |
| `final`クラス | 暗黙的に `final`（継承不可） |
| フィールド | 暗黙的に `private final` |
| コンストラクタ | 自動で全フィールドを引数に持つものが生成される |
| アクセサ | フィールド名と同じメソッド名（`getName()`ではなく`name()`） |
| `equals`/`hashCode`/`toString` | 自動生成される |
| `Serializable` | 実装されないので、必要なら明示的に実装する必要あり |

---

## 🔶 使用例

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Hiroaki", 30);
        System.out.println(p.name()); // → Hiroaki
        System.out.println(p);        // → Person[name=Hiroaki, age=30]
    }
}
```

---

## 🔷 カスタムロジックを追加したい場合

```java
public record Person(String name, int age) {
    public Person {
        if (age < 0) throw new IllegalArgumentException("年齢は0以上");
    }

    public String greet() {
        return "こんにちは、" + name;
    }
}
```

- 上記のように「**コンパクトコンストラクタ**」でバリデーションを追加可能
- メソッド追加もできる（ただし状態は持てない）

---

## 🔷 制限事項（レコードではできないこと）

| 制限内容 | 理由 |
| --- | --- |
| 継承不可 | `record` は暗黙で `final` |
| フィールド追加不可（定義外） | 不変データキャリアの原則 |
| 可変フィールド不可 | `final` によるイミュータブル保証 |

---

## ✅ record が向いているケース

- DTO（データ転送オブジェクト）
- 値オブジェクト（Value Object）
- イミュータブルな構造体的用途
- Mapのキーに使いたい場合（equals/hashCodeが自動）

---

## 🔷 補足：record × switchパターンマッチ（Java 21）

```java
sealed interface Shape permits Circle, Rectangle {}

record Circle(double radius) implements Shape {}
record Rectangle(double width, double height) implements Shape {}

static double area(Shape shape) {
    return switch (shape) {
        case Circle c -> Math.PI * c.radius() * c.radius();
        case Rectangle r -> r.width() * r.height();
    };
}
```

---

## ✅ まとめ表

| 項目 | record | class |
| --- | --- | --- |
| イミュータブル | ✅（標準） | ❌（自分で書く） |
| フィールドの自動定義 | ✅ | ❌ |
| コンストラクタの自動生成 | ✅ | ❌ |
| equals/hashCode/toString自動生成 | ✅ | ❌ |
| 継承 | ❌（final） | ✅ |
| 主な用途 | 値オブジェクト、DTO | 汎用クラス |

以下に、**Java Silver 試験風の `record` に関するパターン問題**を5問ご用意しました。

Silverレベルでは、「構文ルール・自動生成・制限・簡単な使用例」がよく問われます。

---

## ✅ 問題1：record の基本構文

```java
record Person(String name, int age) {}
```

### Q1. 上記のコードで正しい説明はどれ？

A. `Person` は abstract class である

B. `name()` や `age()` というアクセサメソッドが自動生成される

C. `Person` は継承してサブクラスを作れる

D. `getName()` や `getAge()` というメソッドが自動生成される

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q1 | B | `record` ではフィールド名と同名のアクセサメソッドが自動生成される |
| --- | --- | --- |

## ✅ 問題2：使用方法

```java
record Book(String title, int price) {}

public class Main {
    public static void main(String[] args) {
        Book b = new Book("Java入門", 2800);
        System.out.println(b.title());
    }
}
```

### Q2. 出力結果は？

A. Java入門

B. getTitle()

C. title

D. コンパイルエラー

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q2 | A | `title()` は自動生成されたアクセサであり、"Java入門" が出力される |
| --- | --- | --- |

## ✅ 問題3：フィールドの扱い

```java
record Point(int x, int y) {}

public class Main {
    public static void main(String[] args) {
        Point p = new Point(10, 20);
        p.x = 30; // ← この行
    }
}
```

### Q3. 上記コードの挙動として正しいものは？

A. コンパイルが通り、x は 30 に更新される

B. 実行時例外が発生する

C. コンパイルエラーになる

D. x フィールドは public なので変更できる

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q3 | C | `record` のフィールドはすべて `private final` のため、書き換え不可でコンパイルエラー |
| --- | --- | --- |

## ✅ 問題4：equals/hashCode の自動生成

```java
record Item(String name, int price) {}

public class Main {
    public static void main(String[] args) {
        Item a = new Item("Pen", 100);
        Item b = new Item("Pen", 100);
        System.out.println(a.equals(b));
    }
}
```

### Q4. 出力結果は？

A. true

B. false

C. コンパイルエラー

D. 実行時例外

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q4 | A | `equals()` は内容に基づいて自動生成されるので、値が同じなら true |
| --- | --- | --- |

## ✅ 問題5：レコードの制限

```java
record User(String name) {
    void print() {
        System.out.println("名前: " + name);
    }
}
```

### Q5. 上記について正しい記述はどれ？

A. `name` は setter メソッドで書き換え可能

B. `User` クラスはサブクラスから継承可能

C. `print()` のようなメソッド追加は許可されている

D. `name` フィールドは private ではない

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q5 | C | `record` にメソッドの追加は可能。ただし状態（フィールド）の追加は不可 |
| --- | --- | --- |

---

## ✅ Silverレベルで押さえるべきポイントまとめ

| 観点 | 試験で問われやすい点 |
| --- | --- |
| 構文 | `record` の宣言形式、暗黙的 `final` |
| アクセサ | `name()` のようなメソッド名になる（getterではない） |
| フィールド | 自動的に `private final`、変更不可 |
| 自動生成 | `equals` / `hashCode` / `toString` の生成あり |
| 制限 | 継承不可・フィールド追加不可・可変不可 |

お待たせしました。以下に、**Java Goldレベルの `record` に関するパターン問題**を5問ご用意しました。

Goldレベルでは、構造の理解だけでなく、**バリデーション、型安全性、分解パターンとの組み合わせ**などが問われます。

---

## ✅ 問題1：コンパクトコンストラクタの動作

```java
record Product(String name, int price) {
    public Product {
        if (price < 0) {
            throw new IllegalArgumentException("価格は0以上でなければなりません");
        }
    }
}
```

### Q1. 上記コードの説明として正しいものはどれ？

A. コンストラクタでフィールド値を変更しているためコンパイルエラーになる

B. `price` が負の値で渡されたときに例外がスローされる

C. `record` にコンストラクタを定義することはできない

D. `name` というアクセサメソッドは自動生成されない

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q1 | B | コンパクトコンストラクタはフィールドの代入前にバリデーション可能。負値なら例外がスローされる。 |
| --- | --- | --- |

## ✅ 問題2：toString の自動生成と出力内容

```java
record User(String name, int level) {}

public class Main {
    public static void main(String[] args) {
        User u = new User("Hiroaki", 5);
        System.out.println(u);
    }
}
```

### Q2. 出力される文字列の形式として正しいものは？

A. name=Hiroaki, level=5

B. User[name=Hiroaki, level=5]

C. User(name=Hiroaki, level=5)

D. クラス名は出力されず、フィールドだけ出力される

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q2 | B | `record` の `toString()` は `クラス名[フィールド=値,...]` の形式で自動生成される。 |
| --- | --- | --- |

## ✅ 問題3：継承とインターフェースの扱い

```java
interface Printable {
    void print();
}

record Report(String title) implements Printable {
    public void print() {
        System.out.println("タイトル: " + title);
    }
}
```

### Q3. 上記の説明として正しいものはどれ？

A. `record` はインターフェースを実装できない

B. `print()` は自動生成される

C. `Report` は `Printable` を実装している有効なレコード宣言である

D. `title` というフィールドは private ではない

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q3 | C | `record` はインターフェースを自由に実装できる。メソッドも自前で定義可能。 |
| --- | --- | --- |

## ✅ 問題4：分解パターン（deconstruction）と switch

```java
record Point(int x, int y) {}

static void printPoint(Object obj) {
    switch (obj) {
        case Point(int x, int y) -> System.out.println("X=" + x + ", Y=" + y);
        default -> System.out.println("Other");
    }
}
```

### Q4. このコードにおいて正しいものはどれ？

A. `Point(int x, int y)` は構文エラーになる

B. switch文でレコードの中身を直接取り出している

C. `x` と `y` は `Point` の private フィールドなので取り出せない

D. `default` は不要である

---

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q4 | B | Java 21以降で導入された「レコードの分解パターン」により、直接フィールドを取り出して処理できる。 |
| --- | --- | --- |

## ✅ 問題5：不変性と設計の誤り

```java
record Counter(int count) {
    public void increment() {
        count++;
    }
}
```

### Q5. 上記コードの説明として正しいものはどれ？

A. コンパイルエラーにはならない

B. `count` は `final` なので `++` は使えない

C. `record` のフィールドはデフォルトで可変である

D. `increment()` を定義できないため構文エラーとなる

| 問題 | 解答 | 解説 |
| --- | --- | --- |

| Q5 | B | `record` のフィールドはすべて暗黙的に `private final`。`count++` はコンパイルエラーになる。 |
| --- | --- | --- |

---

## ✅ Goldレベルで押さえるべき観点

| 項目 | 説明 |
| --- | --- |
| コンパクトコンストラクタ | フィールド代入前にチェックが可能 |
| フィールドの不変性 | 自動的に `final` で再代入不可 |
| 自動生成メソッド | `equals` / `hashCode` / `toString` の正確な形式 |
| 分解パターン | Java 21以降の重要構文（switchとの連携） |
| インターフェース実装 | `record` でも可能、状態保持は禁止 |