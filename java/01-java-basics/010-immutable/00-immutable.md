# immutable

# Q,

immutable なオブジェクトにするために必要な要件を上げてください。

Javaで**イミュータブル（immutable）なオブジェクト**を作るためには、**一度生成された後に状態が変わらない**ようにする必要があります。以下に、イミュータブルにするための**5つの主要な要件（ルール）**を示します。

---

## ✅ イミュータブルなオブジェクトにするための要件

### 1. **フィールドはすべて `final` にする**

- 一度だけ値が設定され、それ以降変更できないようにする。

```java
private final String name;
```

---

### 2. **クラス自体を `final` にする**

- 継承を防ぎ、サブクラスによる変更や拡張を防止。

```java
public final class Person {
    ...
}
```

---

### 3. **可変（mutable）なオブジェクトは持たない／コピーして保持する**

- 例：`Date`, `List`, `Map` などの可変オブジェクトをフィールドにする場合は、
    - **ディープコピーで保持**
    - **アクセサでもコピーを返す**

```java
private final Date birthDate;

public Person(Date birthDate) {
    this.birthDate = new Date(birthDate.getTime());  // defensive copy
}

public Date getBirthDate() {
    return new Date(birthDate.getTime());  // defensive copy
}
```

---

### 4. **setterメソッドを作らない**

- フィールドを変更できる手段を提供しない。

```java
// setter を用意しない
public String getName() {
    return name;
}
```

---

### 5. **コンストラクタで全フィールドを初期化**

- 初期化漏れを防ぎ、オブジェクトの状態を確定させる。

```java
public Person(String name, Date birthDate) {
    this.name = name;
    this.birthDate = new Date(birthDate.getTime()); // defensive copy
}
```

---

## 🧱 まとめ（一覧）

| 要件 | 内容 |
| --- | --- |
| ① | すべてのフィールドを `final` にする |
| ② | クラスを `final` にして継承を禁止する |
| ③ | フィールドは変更不能なオブジェクト、またはコピーして保持 |
| ④ | setter を定義しない |
| ⑤ | コンストラクタで全フィールドを初期化する |

---

## ✅ 補足：代表的なイミュータブルクラス

| クラス | 説明 |
| --- | --- |
| `String` | Javaの代表的なイミュータブルクラス |
| `Integer`, `Long`, `Double` | ラッパークラス |
| `LocalDate`, `LocalDateTime` | Java 8以降の日時クラス（`java.time`） |

---

必要であれば、`record`（Java 14以降）を使ったイミュータブルクラスの簡単な定義例もご紹介できます。興味ありますか？

[不変オブジェクト参照変更](immutable%2023162cef73508031b8d1f408eb63da79/%E4%B8%8D%E5%A4%89%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E5%8F%82%E7%85%A7%E5%A4%89%E6%9B%B4%2023662cef7350801c9081da9fc1e71949.md)