# ClassCastException

# Q,

この「コンパイルは通るが実行時に落ちるキャスト」が起きる具体的なパターンを整理してください。

了解しました。

Javaにおける「**コンパイルは通るが、実行時に `ClassCastException` が発生するキャスト**」について、具体的なパターンを以下に体系的に整理します。

---

## 🔷 発生する背景

Javaは**キャストを文法的に許容しても、実行時の型が一致しなければ `ClassCastException` を投げる**仕様です。

これは、Javaが**型の安全性をコンパイル時だけで完全に保証できない**（≒型消去やポリモーフィズムの影響）ことが原因です。

---

## ✅ よくあるパターン別整理

### ① **親子関係がない型同士でのキャスト**

### コード例

```java
String s = "Hello";
Integer i = (Integer) (Object) s;  // コンパイルは通る（Object → Integer）
```

- `String` → `Object` はOK
- `Object` → `Integer` も文法上OK（実行時チェック）
- しかし、実際は `Object` の中身が `String` のため **`ClassCastException`**

---

### ② **共通の親を持つが、互いに無関係なサブクラス間のキャスト**

### コード例

```java
Number n = Double.valueOf(3.14);  // 実体は Double
Integer i = (Integer) n;          // Integer にキャスト → 実体は Double → 例外
```

- `Double` と `Integer` は共に `Number` を継承
- しかし、相互には継承関係なし → 実行時に `ClassCastException`

---

### ③ **インタフェースの実装をしていないクラスにキャスト**

### コード例

```java
Object o = new Object(); 
Runnable r = (Runnable) o; // Object は Runnable を実装していない → 例外
```

- `Runnable` はインタフェース
- `Object` は `Runnable` を実装していない

---

### ④ **実行時のインスタンスが親型であるが、サブクラスにキャスト**

### コード例

```java
A a = new A(); 
B b = (B) a;  // A is not a B → ClassCastException
```

- `B` が `A` を継承していても、`a` の実体が `A` ならNG
- **動的バインディングにより実体が確認される**

---

### ⑤ **ジェネリクスで型消去されているコレクションのキャスト**

### コード例

```java
List<String> list = new ArrayList<>();
Object obj = list;

@SuppressWarnings("unchecked")
List<Integer> intList = (List<Integer>) obj; // 実行時は String → Integer で落ちる
```

- コンパイル時は警告のみ（unchecked）
- 実行時、キャストされたリストの要素にアクセスして初めて `ClassCastException`

---

### ⑥ **配列のキャスト時に要素型が一致しない**

### コード例

```java
Object[] objs = new String[10];  // OK
Integer[] ints = (Integer[]) objs; // 実行時例外
```

- `Object[]` は `String[]` だが、`Integer[]` にはキャストできない

---

## ❗注意：キャスト自体では例外が出ず、アクセス時に落ちるケースもある

```java
List raw = new ArrayList<String>();
raw.add("hello");

List<Integer> ints = raw;  // 警告あり、でも通る
Integer i = ints.get(0);   // ← ここで例外発生 (String を Integer にキャスト)
```

---

## 🔚 まとめ表

| パターン | 状況 | コンパイル | 実行時例外 |
| --- | --- | --- | --- |
| 親子関係なし | `String → Integer` | 通る | `ClassCastException` |
| 共通の親のみ | `Double → Integer` | 通る | `ClassCastException` |
| インタフェース未実装 | `Object → Runnable` | 通る | `ClassCastException` |
| 実体が親クラス | `A → B`（実体は A） | 通る | `ClassCastException` |
| ジェネリクス型消去 | `List<String> → List<Integer>` | 通る（警告） | 要素アクセス時例外 |
| 配列の不正キャスト | `String[] → Integer[]` | 通る | `ArrayStoreException` |

必要であれば、**図解（継承関係）付き**で各パターンを視覚的にまとめることも可能です。ご希望ありますか？