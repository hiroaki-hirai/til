# 同名の default()

# Q,

インターフェースの多重継承、多重実装の場合に、インターフェース側にそれぞれ同名にdefaultメソッドがある場合、子クラス及び子インターフェース側で、A.super.test() のようにどの親インターフェースのdefaultメソッドを引き継ぐのか宣言しないとコンパイルエラーになる認識はあっていますか・

はい、**その認識は正確です。**

---

## ✅ 結論（要点）

> 複数のインターフェースが同名の default メソッドを持っており、それらを継承・実装するクラス／インターフェースでは、
> 
> 
> **`A.super.test()` のように「どのインターフェースの `default` を使うか」明示的に選ばなければコンパイルエラーになります。**
> 

これは Java の **「default メソッドの競合解決」**ルールに基づく仕様です。

---

## 🔸 例：インターフェースの多重継承で default メソッドが競合

```java
interface A {
    default void test() {
        System.out.println("A");
    }
}

interface B {
    default void test() {
        System.out.println("B");
    }
}

class C implements A, B {
    @Override
    public void test() {
        A.super.test(); // ← 明示的に選択しないとコンパイルエラーになる
    }
}
```

### 🔍 解説：

- `C` は `A` と `B` の両方を `implements` している
- 両方に `default void test()` が存在
- Java はどちらの `default` を使えばよいか **自動で解決しない**
- よって **明示的に `A.super.test()` または `B.super.test()` を書く必要がある**
- 書かないとコンパイルエラーになります

---

## ✅ 補足：子インターフェースで多重継承する場合も同様

```java
interface A {
    default void test() {
        System.out.println("A");
    }
}

interface B {
    default void test() {
        System.out.println("B");
    }
}

interface C extends A, B {
    // ❌ コンパイルエラー：test() の衝突があるため、C でどちらかをオーバーライドして解決する必要がある
}
```

```java
interface C extends A, B {
    @Override
    default void test() {
        A.super.test(); // ✅ 明示的に選択すればOK
    }
}
```

### **✅ 選択しないとどうなるか？**

```java
Error: class C inherits unrelated defaults for test() from types A and B
```

➡ Java コンパイラは「どちらの default を採用すべきかわからない」としてエラーを出します。

---

## ✅ 補足：クラスに継承されたメソッドがある場合は、クラス優先

```java
interface A {
    default void test() {
        System.out.println("A");
    }
}

class Super {
    public void test() {
        System.out.println("Super");
    }
}

class C extends Super implements A {
    // ✅ OK：Super クラスのメソッドが優先されるため、競合なし
}
```

- このように **クラスに同名メソッドがあれば、インターフェースの default は無視されます**
- Java のルール：「クラス > インターフェース」

---

## ✅ まとめ表

| 構成 | 結果 | 解決方法 |
| --- | --- | --- |
| `class C implements A, B`（両方に同名 `default`） | ❌ コンパイルエラー | `A.super.test()` などで明示する |
| `interface C extends A, B`（両方に同名 `default`） | ❌ コンパイルエラー | `default` メソッドを override して選ぶ |
| クラスに継承元メソッドがある場合 | ✅ クラスが優先 | override 不要 |

---

## ✅ 結論（再掲）

> インターフェースの多重継承／実装で default メソッドが競合する場合は、明示的に A.super.method() で選ばなければコンパイルエラーになります。
> 
> 
> これは **クラスと異なり、インターフェースは優先順位がつかない**ため、Java が自動解決しない仕様です。
> 

---

必要であれば、競合回避のパターン別（default削除／抽象化／ユーティリティ委譲）もご案内可能です。