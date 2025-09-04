# IndexOutOfBoundsException

# Q,

IndexOutOfBoundsException、ArrayIndexOutOfBoundsException、StringIndexOutOfBoundsException。
上記３つの違いを具体例を元に解説してください。

はい、Hiroakiさん。

`IndexOutOfBoundsException` とその2つのサブクラス `ArrayIndexOutOfBoundsException`、`StringIndexOutOfBoundsException` は **「インデックスアクセスの範囲外」によって発生する非チェック例外（RuntimeException）** ですが、対象とするデータ構造が異なります。

---

# ✅ 各例外の違いと対象

| 例外クラス | 対象 | 概要 |
| --- | --- | --- |
| `IndexOutOfBoundsException` | **汎用コレクション（Listなど）** | 抽象的なインデックス違反 |
| `ArrayIndexOutOfBoundsException` | **配列（array）** | 配列のインデックスが無効な場合に発生 |
| `StringIndexOutOfBoundsException` | **文字列（String）** | 文字列のインデックスが範囲外の場合に発生 |

---

# 🔍 それぞれの具体例と解説

---

## 🔸 1. `ArrayIndexOutOfBoundsException`

### 💥 発生条件：

- **配列の範囲外にアクセス**したとき

```java
int[] numbers = {1, 2, 3};
System.out.println(numbers[3]); // → インデックス3は存在しない
```

### 🧾 出力例：

```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
```

---

## 🔸 2. `StringIndexOutOfBoundsException`

### 💥 発生条件：

- **文字列のインデックスが範囲外**のとき（例：`charAt`, `substring`）

```java
String text = "abc";
System.out.println(text.charAt(5)); // → インデックス5は存在しない
```

### 🧾 出力例：

```java
Exception in thread "main" java.lang.StringIndexOutOfBoundsException: Index 5 out of range for length 3
```

---

## 🔸 3. `IndexOutOfBoundsException`

### 💥 発生条件：

- **Listなどのコレクションのインデックスが範囲外**

```java
List<String> list = List.of("apple", "banana");
System.out.println(list.get(2)); // → インデックス2は存在しない
```

### 🧾 出力例：

```java
Exception in thread "main" java.lang.IndexOutOfBoundsException: Index 2 out of bounds for length 2
```

---

# 🧠 クラス階層の関係

```java
java.lang.RuntimeException
└── java.lang.IndexOutOfBoundsException
    ├── java.lang.ArrayIndexOutOfBoundsException
    └── java.lang.StringIndexOutOfBoundsException
```

- `IndexOutOfBoundsException` は **汎用的な親クラス**
- 配列・文字列では **より具体的なサブクラス**でスローされる

---

# ✅ 実践的まとめ

| 特徴項目 | List や Set (`get()`) | 配列 (`[]`) | String (`charAt`, `substring`) |
| --- | --- | --- | --- |
| 発生する例外 | `IndexOutOfBoundsException` | `ArrayIndexOutOfBoundsException` | `StringIndexOutOfBoundsException` |
| 修正方法 | `size()` で範囲確認 | `.length` で範囲確認 | `.length()` で範囲確認 |
| 事前チェック例 | `if (i < list.size())` | `if (i < array.length)` | `if (i < str.length())` |

---

必要であれば、**「これらの例外を防ぐためのユーティリティ関数」**や、「実運用でのガードコード例」もお出しできますので、関心あればお知らせください。