# superクラスフィールド呼び出し

# Q,

A→B→C という継承関係があり、A内にあるフィールドへCからメソッドを使用しアクセスすることは可能でしょうか？

はい、**可能です**。

ただし、**アクセス修飾子（`private`, `protected`, `public`, なし）**によってアクセスの可否が決まります。

---

### 🔷 継承関係の前提

```java
class A {
    protected int value = 100; // Aのフィールド
}

class B extends A {
    // BはAを継承
}

class C extends B {
    // CはBを継承 → 結果としてAのフィールドにもアクセス可能
    public void printValue() {
        System.out.println(value);  // ← Aのvalueにアクセス
    }
}
```

このように、CからAの`value`にアクセスできます。

---

### 🔸 アクセス修飾子による違い

| フィールドの修飾子 | Cクラスからのアクセス可否 | 説明 |
| --- | --- | --- |
| `public` | ✅ 可能 | どこからでもアクセス可能 |
| `protected` | ✅ 可能 | 継承関係にあればアクセス可能 |
| （修飾子なし） | ❌ 不可（別パッケージの場合） | 同一パッケージ内でのみアクセス可能 |
| `private` | ❌ 不可 | サブクラスからはアクセス不可 |

---

### 🔷 まとめ

- CからAのフィールドにアクセスできるかどうかは、その**フィールドのアクセス修飾子**に依存します。
- 継承関係があっても、`private`なフィールドには直接アクセスできません。

必要に応じて、`A`に`protected`なgetterメソッドを用意するのが一般的です：

```java
class A {
    private int value = 100;

    protected int getValue() {
        return value;
    }
}

class C extends B {
    public void printValue() {
        System.out.println(getValue());  // ← getter経由でアクセス
    }
}
```

必要であれば、アクセス修飾子の詳細やgetter/setterの設計方針についても解説できます。