# Supplier

はい、Hiroakiさん。Javaの `Supplier<T>` インタフェースは、関数型インタフェースの1つで、特に「値を供給する（提供する）もの」として使われます。

---

### ✅ 基本情報

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

- **型引数 `T`** は供給する値の型
- **メソッド `get()`** は引数を取らず、型 `T` の値を返す

---

### ✅ 特徴と目的

- 引数は**受け取らず**、値だけを**返す**
- 主に「**遅延評価**」「**キャッシュ**」「**再計算**」などで使われる
- 無引数コンストラクタや任意の計算処理の**ラッパー**として使える

---

### ✅ 使い方の例

### ① 簡単な使用例

```java
Supplier<String> supplier = () -> "Hello, World!";
System.out.println(supplier.get());  // → Hello, World!
```

### ② 遅延評価（Lazy Initialization）

```java
public class Config {
    private Supplier<String> tokenSupplier = () -> {
        System.out.println("Heavy computation...");
        return UUID.randomUUID().toString();
    };

    public void printToken() {
        System.out.println(tokenSupplier.get());
    }
}
```

このように `Supplier` を使うことで、**必要なときだけ初期化**するような処理も書けます。

---

### ✅ 関数型インタフェースとの比較

| インタフェース名 | 引数 | 戻り値 | 主な用途 |
| --- | --- | --- | --- |
| `Supplier<T>` | なし | T | 値を返す |
| `Consumer<T>` | T | void | 値を受け取って使う |
| `Function<T,R>` | T | R | 値を受け取って変換 |
| `Predicate<T>` | T | boolean | 値を判定する（true/false） |

---

### ✅ 応用例（Stream API）

```java
Stream.generate(() -> Math.random())
      .limit(3)
      .forEach(System.out::println);
```

- `Stream.generate(Supplier<T>)` は `Supplier` を使って値を繰り返し生成します。

---

### ✅ 補足：Java Silver/Gold 試験では？

- `Supplier` はJava 8から導入された関数型インタフェース群の一部で、**試験では `Function`, `Predicate` の方が頻出**
- ただし、**Streamとの併用やラムダの型付け**で出題される可能性があります。

---

## ✅ Supplier パターン問題集（Java Silver/Gold風）

---

### 🔹 問題 1：基本構文の理解（Silver寄り）

次のコードの出力として正しいものはどれか。

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<String> s = () -> "Hello Java";
        System.out.println(s.get());
    }
}
```

### A. Hello Java

### B. Supplier

### C. null

### D. コンパイルエラー

✅ 解説と答え

**答え：A**

- `Supplier` は引数を取らず、値を返す関数型インタフェース
- `s.get()` により `"Hello Java"` を返す
- 構文的にも問題なし

---

### 🔹 問題 2：遅延評価の理解（Gold風）

以下のコードを実行すると出力はどうなるか。

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<String> lazy = () -> {
            System.out.println("Computing...");
            return "Result";
        };

        System.out.println("Before get");
        System.out.println(lazy.get());
        System.out.println("After get");
    }
}
```

```java
Before get
Computing...
Result
After get
```

✅ 解説と答え

**答え：B**

- `Supplier#get()` を呼ぶタイミングで `Computing...` が出力される
- それまでは評価されない（＝遅延評価）

---

### 🔹 問題 3：ラムダと匿名クラスの違い（Gold風）

次のコードの出力として正しいものを選べ。

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<Integer> s1 = () -> 42;
        Supplier<Integer> s2 = new Supplier<Integer>() {
            public Integer get() {
                return 21 + 21;
            }
        };

        System.out.println(s1.get() + s2.get());
    }
}
```

### A. 42

### B. 84

### C. 63

### D. 0

✅ 解説と答え

**答え：B**

- `s1.get()` は 42 を返す
- `s2.get()` も 21 + 21 = 42 を返す
- 合計 84

---

### 🔹 問題 4：Stream.generate の使い方（Gold風）

以下のコードを実行したとき、出力される行数として正しいものはどれか。

```java
import java.util.function.Supplier;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        Supplier<String> supplier = () -> "Java";
        Stream.generate(supplier)
              .limit(5)
              .forEach(System.out::println);
    }
}
```

### A. 0 行

### B. 1 行

### C. 5 行

### D. コンパイルエラー

✅ 解説と答え

**答え：C**

- `Stream.generate(supplier)` は無限ストリームを作るが、
- `.limit(5)` により 5 回 `"Java"` を出力する

---

### 🔹 問題 5：Optionalとの組み合わせ（応用）

次のコードの出力として正しいものはどれか。

```java
import java.util.Optional;
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<String> supplier = () -> "Default";
        String result = Optional.ofNullable(null).orElseGet(supplier);
        System.out.println(result);
    }
}
```

### A. null

### B. supplier

### C. Default

### D. コンパイルエラー

✅ 解説と答え

**答え：C**

- `Optional.ofNullable(null)` → 空のOptional
- `orElseGet(supplier)` により `supplier.get()` が呼ばれ `"Default"` を返す

</details>

---

### 🔚 補足

- `Supplier<T>` は出題数は多くありませんが、**ラムダ式、関数型インタフェースの応用**として Gold で登場する可能性あり
- `Optional.orElseGet()` や `Stream.generate()` との組み合わせを見ておくと実務でも役立ちます

# Q,

Supplierを利用するとfactoryクラスがなくても、具象クラスへ依存しない構造にできるという認識はあっていますか？

はい、Hiroakiさんの認識は **概ね正しいです**。

ただし、もう少し**正確に表現すると**：

> ✅ Supplierを使うことで、「具象クラスを直接newしない構造」にできるため、Factoryクラスのような役割を担える
> 
> 
> 🔁 しかし「具象クラスへの**依存そのものがゼロになる」わけではない**（背後で依存関係は存在）
> 

という形になります。

---

## ✅ Supplierを使った構造の本質

### 🔹 通常のFactoryパターン（例）：

```java
class UserServiceFactory {
    public static UserService create() {
        return new UserServiceImpl();  // ← 具象クラスに依存
    }
}
```

- 呼び出し側は `UserServiceFactory.create()` を使えばよく、
- **UserServiceImpl の知識は不要**

---

### 🔹 `Supplier` を使った場合：

```java
Supplier<UserService> supplier = UserServiceImpl::new;

UserService service = supplier.get();
```

- `Supplier<T>` によって「インスタンス供給手段（new処理）」を抽象化
- 呼び出し側は `get()` しか知らなくてよい
- **ラムダ or メソッド参照で柔軟に差し替え可能**

---

## ✅ 比較：Factory vs Supplier

| 項目 | 通常のFactoryクラス | Supplierによる代替 |
| --- | --- | --- |
| クラス構造 | `XXXFactory.create()` の静的メソッド | `Supplier<T>` 型の変数 or ラムダ |
| 呼び出し時 | `XXXFactory.create()` | `supplier.get()` |
| 依存の表現 | `UserServiceImpl` を明示的に書く | `UserServiceImpl::new` などの間接参照 |
| 柔軟性 | 固定的になりやすい | DIやテストに柔軟に対応可 |
| 意味的用途 | **「明示的な設計」**としての役割 | **「関数型的な依存抽象」**としての簡略化手段 |

---

## ✅ DIやテストと組み合わせると強力

### 例：依存注入で差し替えたいとき

```java
class ServiceManager {
    private final Supplier<UserService> serviceSupplier;

    public ServiceManager(Supplier<UserService> serviceSupplier) {
        this.serviceSupplier = serviceSupplier;
    }

    public void run() {
        UserService service = serviceSupplier.get();
        service.doSomething();
    }
}
```

- この構造では、`UserServiceImpl` に **直接依存していない**。
- モックやテストダブルも簡単に差し込める：

```java
new ServiceManager(() -> new MockUserService())
```

---

## ✅ 結論

> Supplierを利用するとfactoryクラスがなくても、具象クラスへ依存しない構造にできるという認識はあっていますか？
> 

| 要素 | 評価 |
| --- | --- |
| ✅ new処理を抽象化して疎結合にする | **Yes** |
| ✅ Factoryクラスの代替手段になる | **Yes** |
| ❌ 具象クラスへの依存が完全にゼロになる | **No（依存の“隠蔽”に近い）** |

---

**要点まとめ**：

- `Supplier<T>` は **簡易ファクトリのように使える関数型手段**
- 「いつ・どのようにnewするか」を **戦略的に外部化**できる
- Java SE 8以降の設計において、Factoryパターンの代替・簡略化手法としてよく使われる
- ただし、具象クラスへの「**間接的な依存は残る**」ので、完全な依存逆転とは言い切れない