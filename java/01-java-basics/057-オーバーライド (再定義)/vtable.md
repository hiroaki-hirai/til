# vtable

# Q,

オーバーライドのメソッド・ティスパッチ・テーブルの処理の流れを視覚化してください。
画像ではなく文字で表現してください。図はOKです。

はい、Hiroakiさん。

ここでは、**オーバーライドされたメソッドのディスパッチ処理（仮想メソッドテーブル vtable）**の流れを、**文字ベースの図**で視覚化して説明します。

---

## ✅ 前提コード（オーバーライドの例）

```java
class Parent {
    void sayHello() {
        System.out.println("Hello from Parent");
    }
}

class Child extends Parent {
    @Override
    void sayHello() {
        System.out.println("Hello from Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();  // 実体は Child
        p.sayHello();            // ← ここが「動的ディスパッチ」
    }
}
```

---

## ✅ オーバーライドのディスパッチ処理の概念図（vtableの流れ）

```java
             ┌─────────────┐
             │  Parent p = │────┐
             └─────────────┘    │
                                ▼
                          ┌───────────────┐
                          │ 実体: new Child│
                          └───────────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │  メソッドディスパッチ│
                         │ （仮想メソッド表 vtable）│
                         └─────────────────────┘
                                   │
                ┌──────────────────┼──────────────────┐
                │ 親クラスの vtable │   子クラスの vtable │
                ▼                  ▼
          +----------------+   +----------------------+
          | sayHello() → P |   | sayHello() → C ← ★実行対象 |
          +----------------+   +----------------------+
                                ▲
                                │
                p.sayHello() はこの実装を呼び出す
```

---

## 🔍 解説ステップごとに

| ステップ | 内容 |
| --- | --- |
| ① 変数宣言 | `Parent p = new Child();` で、参照は親型だが、実体は子クラス |
| ② メソッド呼び出し | `p.sayHello();` で、**動的ディスパッチ（実行時バインディング）**が発動 |
| ③ vtable参照 | JVMは「実体が Child だから、Child の仮想メソッドテーブルを見る」 |
| ④ 実行 | `sayHello()` のエントリが `Child.sayHello()` を指しているため、それが呼ばれる |

---

## ✅ vtable（仮想メソッドテーブル）とは？

- JVMが各クラスごとに持つ「メソッドテーブル」。
- オーバーライドされたメソッドは、**同じスロット番号に上書きされている**。
- `Parent` と `Child` の `sayHello()` は、**同じスロットに配置されているが、内容が異なる**。

---

## ✅ まとめフレーズ

> オーバーライドは**「実体に応じたメソッド呼び出し」**を実現する仕組みであり、
> 
> 
> **JVMは仮想メソッドテーブル（vtable）を参照して、実体に一致するメソッドを呼び出す。**
> 

---

ご希望があれば、**vtableの構造を模した表形式**や、**インターフェース実装時のディスパッチとの違い（invokeinterface vs invokevirtual）**も説明できます！

# Q,

vtableの構造を模した表形式をお願いします

はい、Hiroakiさん。

ここでは、**Javaの仮想メソッドテーブル（vtable）** の構造を、**表形式で模した形**で示します。

例をベースに、**クラスの継承とオーバーライド**がvtable上でどう表現されるかを具体的に解説します。

---

## ✅ サンプルクラス定義

```java
class Parent {
    void sayHello() { System.out.println("Parent"); }
    void walk()     { System.out.println("Walking..."); }
}

class Child extends Parent {
    @Override
    void sayHello() { System.out.println("Child"); }
    void run()      { System.out.println("Running..."); }
}
```

---

## ✅ 仮想メソッドテーブル（vtable）の模擬構造

| スロット | Parent の vtable | Child の vtable |
| --- | --- | --- |
| 0 | `sayHello() → Parent.sayHello()` | `sayHello() → Child.sayHello()` ✅オーバーライド |
| 1 | `walk() → Parent.walk()` | `walk() → Parent.walk()`     ✅継承そのまま |
| 2 | ― | `run() → Child.run()`       ✅Child専用 |

---

### 🔍 解説ポイント：

- **スロット番号**は、JVM内部でメソッドを高速に探すためのインデックスです（クラスごとに固定）。
- `sayHello()` は Parent と Child で **同じスロット（0番）に存在**しており、Child がオーバーライドすることでエントリが**上書き**されています。
- `walk()` はオーバーライドされていないため、ParentとChildのvtableで同じ実装を参照します。
- `run()` はChild固有のメソッドなので、Childのvtableにのみ存在します。

---

## ✅ 実行時呼び出しの挙動

```java
Parent p = new Child();
p.sayHello(); // → vtable[0] → Child.sayHello() が呼ばれる
p.walk();     // → vtable[1] → Parent.walk() が呼ばれる
```

---

## ✅ vtableを使う理由（メリット）

| 項目 | 内容 |
| --- | --- |
| 高速な動的ディスパッチ | スロット番号で即座にメソッドを参照できるため、実行時でも高速 |
| オーバーライド対応 | 同じスロットをサブクラスが上書きできるため、実体に応じたメソッド呼び出しが可能 |
| メモリ効率 | メソッドの配置はスロットテーブル上に一元管理されるため効率的 |

---

## ✅ 補足：インターフェースとの違い

- クラスのvtable参照は `invokevirtual` 命令を使う。
- インターフェースは `invokeinterface` 命令を使い、**より複雑なディスパッチ処理（別の表構造）**になる。

---

必要であれば、**interface実装のメソッドテーブル構造（itable）**との比較表も作成できます！