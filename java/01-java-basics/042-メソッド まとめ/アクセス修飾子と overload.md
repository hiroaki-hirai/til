# アクセス修飾子と overload

# Q,

アクセス修飾子で制限されているメソッドの場合、オーバーライドは不可ですが、オーバーロードはどうでしょうか？

とても鋭い質問です。

Hiroakiさんの問いに対する答えは：

---

## ✅ 結論：

> アクセス修飾子で制限されているメソッドは、オーバーライドは不可ですが、
> 
> 
> **オーバーロードは可能です。**
> 

---

## 🔹 オーバーライドとオーバーロードの違い（前提整理）

| 項目 | オーバーライド（override） | オーバーロード（overload） |
| --- | --- | --- |
| 意味 | 親クラスのメソッドを上書き | 同じクラス内で名前が同じで引数が異なるメソッドを複数定義 |
| 関係する要素 | 継承／アクセス修飾子／戻り値／例外など | 引数の個数・型のみ |
| アクセス修飾子の影響 | **受ける（親のメソッドが見えなければ上書き不可）** | **受けない（親を見ずに定義できる）** |

### **🔸 例で確認：**

```java
class Parent {
    private void greet(String name) {
        System.out.println("Hello, " + name);
    }
}

class Child extends Parent {
    // オーバーライドではない（見えてないので）
    public void greet(String name, int times) {
        for (int i = 0; i < times; i++) {
            System.out.println("Hi, " + name);
        }
    }
}
```

### 解説：

- `Parent#greet(String)` は `private` → **`Child` からは見えない**
- よって **オーバーライドは不可能（そもそも見えない＝継承されない扱い）**
- 一方で、`Child` 内に同名の別メソッド `greet(String, int)` を定義 → **これはオーバーロードとして有効**

---

## ✅ ポイントまとめ

| 条件 | オーバーライド可否 | オーバーロード可否 | 理由 |
| --- | --- | --- | --- |
| 親メソッドが `private` | ❌ | ✅ | 子クラスから見えないため override 不可／同名メソッドは overload 可能 |
| 親メソッドが `default`（パッケージ）でパッケージが異なる | ❌ | ✅ | 同様に子クラスから見えないため |
| 親メソッドが `protected` または `public` | ✅ | ✅ | override も overload も可能 |

### **✅ 注意点：混同しやすいケース**

```java
class Parent {
    private void test() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    public void test() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.test(); // → "Child"
        // ((Parent)c).test(); // ❌ コンパイルエラー：Parent.test() は private
    }
}
```

- 上記の `Child.test()` は、**Parent の `test()` をオーバーライドしているように見えるが、実際は完全に別メソッド（オーバーロードではなく「単なる定義」）**
- `@Override` をつけると **コンパイルエラーになる**（＝正しく override していない）

---

## ✅ 補足：`@Override` を使って防げるミス

```java
@Override
public void test() {} // ← Parent の test() が private の場合 → ❌ コンパイルエラー
```

- `@Override` は「本当に親のメソッドをオーバーライドしているか」をチェックしてくれる重要なツールです。

---

## 🔚 結論（再掲）

> アクセス修飾子により親クラスのメソッドが見えない場合、オーバーライドはできません。
> 
> 
> ただし、同名メソッドを別引数で定義することは **オーバーロードとして可能です。**
> 

必要であれば、JVM バイトコードの挙動や `javap` の出力例でも確認できます。お気軽にどうぞ。