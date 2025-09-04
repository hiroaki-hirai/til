# ClassCastException

# Q,

ClassCastException のパターン問題を作成してください。

もちろんです。Javaの`ClassCastException`は、**実行時に不正な型変換（キャスト）を行ったときに発生します。以下に、Java Silver～Gold レベルを意識したパターン問題**をいくつか用意しました。

---

## 🔸 パターン問題 ①：基本的なキャストの失敗（Silverレベル）

```java
public class Main {
    public static void main(String[] args) {
        Object obj = "Hello";
        Integer num = (Integer) obj; // ★ここで例外が発生する
        System.out.println(num);
    }
}
```

### ❓ 問題：

上記のコードを実行するとどうなりますか？

1. `"Hello"`が出力される
2. `null`が出力される
3. コンパイルエラーになる
4. `ClassCastException`が発生する

✅ **正解：4**

（`obj`は実体として`String`型なので、`Integer`型へのキャストは失敗）

---

## 🔸 パターン問題 ②：配列におけるキャストの失敗（Goldレベル）

```java
public class Main {
    public static void main(String[] args) {
        Object[] objs = new String[3];
        objs[0] = 100; // ★ここで例外が発生
    }
}
```

### ❓ 問題：

このコードを実行するとどうなりますか？

1. `"100"`が格納される
2. コンパイルエラーになる
3. `ClassCastException`が発生する
4. `ArrayStoreException`が発生する

✅ **正解：4（ArrayStoreException）**

※ これは`ClassCastException`ではなく**配列の型安全違反**のため別の例外です。混同しやすいので注意！

---

## 🔸 パターン問題 ③：List要素の不正キャスト（Goldレベル）

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Object> list = new ArrayList<>();
        list.add("abc");
        list.add(123);

        for (Object obj : list) {
            String s = (String) obj; // ★ここで例外が発生する
            System.out.println(s.length());
        }
    }
}
```

### ❓ 問題：

このコードの出力結果として正しいのは？

1. `3` と `3`
2. `3` と `ClassCastException`
3. コンパイルエラー
4. 例外は発生しない

✅ **正解：2**

（1つ目の`"abc"`はキャスト成功、2つ目の`123`は`Integer`なのでキャスト失敗）

---

## 🔸 パターン問題 ④：キャスト先の参照型を誤る（Silver-Gold中間）

```java
class Animal {}
class Dog extends Animal {}
class Cat extends Animal {}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        Cat c = (Cat) a; // ★ここで例外
    }
}
```

### ❓ 問題：

このコードを実行すると？

1. 正常にCatにキャストされる
2. `ClassCastException`が発生する
3. コンパイルエラーになる
4. `NullPointerException`が発生する

✅ **正解：2**

（実体が`Dog`であるのに`Cat`にキャストしている）

---

## 🔸 パターン問題 ⑤：ジェネリクスの型安全（ボーナス）

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List rawList = new ArrayList();  // ← 型未指定（raw型）
        rawList.add("text");
        rawList.add(10);

        for (Object obj : rawList) {
            String s = (String) obj; // ★ここで例外が発生
            System.out.println(s.toUpperCase());
        }
    }
}
```

### ❓ 問題：

実行時に何が起こりますか？

✅ **正解：`ClassCastException`**

→ `"text"` は OK、`10` は `String` ではないため実行時に例外。

---

## 💡補足：コンパイルOKでも実行時エラー！

`ClassCastException`は：

- **コンパイル時は通る**
- **実行時にインスタンスの実型とキャスト先の型が一致しないと例外**

という性質を持ち、**参照型の代入・キャスト・ジェネリクスの理解**を問う問題でよく使われます。

---

ご希望であれば、**出題形式（選択肢なし／選択肢付き／穴埋めなど）**や、**分野別（配列、List、継承、ジェネリクス）**でも分類できます！