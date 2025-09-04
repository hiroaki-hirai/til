# defaultメソッド 呼び出し

# Q,

```java
public interface A {
    
    default void sample() {
        System.out.println("Hello");
    }
}

public interface B extends A {

}

public class C implements B {
    
    @Override
    public void sample() {
        A.super.sample();
        System.out.println("Java");
    }

}

public class Main {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        A a = new C();
        a.sample();

    }

}
```

上記コードを実行すると以下のエラーが発生しました。

```java
Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
	Illegal reference to super type A, cannot bypass the more specific direct super type test06.B

	at test06.C.sample(C.java:7)
	at test06.Main.main(Main.java:8)
```

Hiroakiさん、これは非常に良い質問です。

ご提示のエラーメッセージ：

```java
Illegal reference to super type A, cannot bypass the more specific direct super type test06.B
```

は、**Javaの default メソッドと `super` 呼び出しにおけるルール違反が原因**です。

---

## ✅ 結論：

**インターフェース A の `default` メソッドは、C から `A.super.sample()` の形では呼び出せません。**

> C は B を実装しており、B が A を継承しているため、C から見ると A は間接的な親。
> 
> 
> Javaでは、**`super` で呼び出せるのは「直接実装しているインターフェース」の default メソッドのみ** という制約があります。
> 

---

## 🔍 なぜエラーになるのか？

```java
public class C implements B {
    @Override
    public void sample() {
        A.super.sample(); // ❌ NG：C は A を直接実装していない
    }
}
```

- `C` は **`B` を直接実装**していて、`B` は `A` を継承している。
- つまり、`A` は C にとって「間接的な super type」。
- Java の仕様では、**`super` は「直接の superinterface」しか指定できません**。

---

## ✅ 修正方法（正しい呼び出し方法）

### 方法①：`B`に default メソッドを移す

```java
public interface B extends A {
    @Override
    default void sample() {
        A.super.sample(); // ✅ OK：ここでは A は直接の superinterface
    }
}
```

そして `C` ではオーバーライドする場合、`super.sample()`（= B.super）で呼び出せます。

---

### 方法②：`C` が A を**直接**実装すれば OK

```java
public class C implements A, B {
    @Override
    public void sample() {
        A.super.sample(); // ✅ OK：A は直接実装されている
        System.out.println("Java");
    }
}
```

---

## ✅ まとめ表

| 構文 | 使用可能条件 | 結果 |
| --- | --- | --- |
| `X.super.method()` | クラスが `X` を**直接** `implements` している | ✅ OK |
| `X.super.method()` | クラスが `X` を**間接**的に継承しているだけ | ❌ コンパイルエラー |

---

## ✅ 最終的な修正例（動くコード）

```java
public interface A {
    default void sample() {
        System.out.println("Hello");
    }
}

public interface B extends A {
    @Override
    default void sample() {
        A.super.sample(); // OK
    }
}

public class C implements B {
    @Override
    public void sample() {
        B.super.sample(); // 呼び出し可能！
        System.out.println("Java");
    }
}

public class Main {
    public static void main(String[] args) {
        A a = new C();
        a.sample();
    }
}
```

---

## ✅ 出力結果

```java
Hello
Java
```

---

ご希望があれば、「defaultメソッドの継承と競合解決ルール」や「複数インターフェースのダイヤモンド継承」などの関連トピックも整理できます！