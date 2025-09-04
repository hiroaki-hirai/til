# 自己返却（this）型チェーン

# Q,

メソッドチェーンを使用する際、メソッドの戻り値をクラス型で返す場合はメソッドチェーン可能。
フィールドに代入といった副作用のみで戻り値がなくても、thisキーワードにてクラス型の参照を返せばこちらもメソッドチェーン可能。
という認識は合っていますか？

はい、Hiroakiさんの認識は**完全に正しいです**。

メソッドチェーンの仕組みと `this` の活用について、以下に整理・補足します。

---

## ✅ あなたの認識まとめ

> メソッドの戻り値をクラス型で返す場合はメソッドチェーン可能。
> 

👉 **その通りです。**

```java
public class Person {
    private String name;
    
    public Person setName(String name) {
        this.name = name;
        return this;  // ← 戻り値が自身（クラス型）なのでチェーン可能
    }
}
```

---

> フィールドに代入といった副作用のみで戻り値がなくても、this キーワードにてクラス型の参照を返せばこちらもメソッドチェーン可能。
> 

👉 **まさにそのとおりです。**

- 通常、副作用だけを行うメソッド（例：`void setName(...)`）は**チェーンできません**。
- しかし、戻り値を `this` にすれば、**「副作用＋チェーン」が両立できます。**

---

## ✅ 具体例：メソッドチェーン可能なクラス

```java
public class User {
    private String name;
    private int age;

    public User setName(String name) {
        this.name = name;
        return this;
    }

    public User setAge(int age) {
        this.age = age;
        return this;
    }

    public void printInfo() {
        System.out.println(name + " (" + age + ")");
    }
}
```

使用例：

```java
User user = new User()
    .setName("Hiroaki")
    .setAge(30);  // ← メソッドチェーン！

user.printInfo(); // → Hiroaki (30)
```

---

## ✅ なぜ `this` を返せばチェーンできるのか？

- `this` は **現在のインスタンスの参照**なので、
    
    その戻り値に対して**さらにメソッドを呼び出すことができる**。
    
- よって、**戻り値の型がクラス自身ならチェーンが可能**になります。

---

## ✅ メソッドチェーンのよくある使い方

| 分野 | 例 |
| --- | --- |
| **ビルダー（Builder）** | `new StringBuilder().append("A").append("B")` |
| **設定系メソッド** | `new Config().setHost("...").setPort(1234)` |
| **Java Streams** | `list.stream().filter(...).map(...).collect(...)` |

---

## ✅ まとめ

| あなたの認識 | 判定 | 説明 |
| --- | --- | --- |
| クラス型を返すメソッドならチェーン可能 | ✅ | `戻り値.class.method()` の構造が続く |
| `this` を返すことで副作用メソッドもチェーン化できる | ✅ | 通常は `void` → `クラス型` に変更すればOK |

---

より高度な例（ビルダーパターン、ジェネリクスを用いたチェーンなど）に進むこともできますので、

必要に応じてご希望を教えてください！