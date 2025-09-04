# empty()

# Q,

どういう文脈でOptional.empty()を使用するのですか？
コード例をお願いします

素晴らしい質問です、Hiroakiさん。

---

## ✅ `Optional.empty()` を使う主な文脈

`Optional.empty()` は「**結果として返すべき値が存在しないことを** *明示的に表現する*」ために使います。

つまり、従来の `null` を返す場面を **安全で意図が明確な形に置き換える** 目的で使用します。

---

## 🎯 使用される具体的な文脈パターン

| パターン | 説明 |
| --- | --- |
| 検索結果なし | データが見つからないときに `Optional.empty()` を返す |
| バリデーション不合格 | 条件に合わないときに値を返さない |
| オブジェクト生成に失敗 | `Optional.empty()` を返して呼び出し元に判断させる |
| 事前チェックで不正値 | 無効な入力に対して処理をスキップするためのシグナル |

---

## 🧪 コード例：簡易検索メソッド

### ① 従来の null 返却

```java
public User findUser(String id) {
    if (db.contains(id)) {
        return db.get(id);
    } else {
        return null; // ← 呼び出し元がnullチェックしないとNPE
    }
}
```

### ② Optional.empty() を使うバージョン

```java
Map<String, User> db = new HashMap<>();

public Optional<User> findUser(String id) {
    if (db.containsKey(id)) {  // ← ここが正しい
        return Optional.of(db.get(id));
    } else {
        return Optional.empty();
    }
}
```

### 呼び出し側のコード（安全）

```java
Optional<User> maybeUser = findUser("u123");

maybeUser.ifPresent(user -> {
    System.out.println("Found: " + user.getName());
});

User user = maybeUser.orElse(new User("guest", "ゲストユーザー"));
```

---

## 📘 他のコード例：バリデーションで使用

```java
public Optional<String> parseToUpperIfAlpha(String input) {
    if (input.matches("[a-zA-Z]+")) {
        return Optional.of(input.toUpperCase());
    } else {
        return Optional.empty();  // アルファベット以外なら空で返す
    }
}
```

---

## 📌 メリット：なぜ null ではなく Optional.empty() を使うのか？

| 項目 | null | Optional.empty() |
| --- | --- | --- |
| 可読性 | 「無いかも」と読み取りにくい | 「明示的に値が無い」 |
| 安全性 | `NullPointerException` の危険 | `Optional` APIで安全に処理できる |
| 設計意図 | 曖昧 | 明示的に「値が無いという意味」 |
| 呼び出し側の負担 | nullチェックが必要 | `ifPresent` や `orElse` で安全に処理 |

---

## ✅ まとめ

| 用途 | 説明 |
| --- | --- |
| データ取得で見つからなかった | `return Optional.empty();` |
| 条件を満たさなかった | `return Optional.empty();` |
| 失敗や無効を明示したい | `return Optional.empty();` |
| 呼び出し元に処理を委ねたい | `Optional` を返して判断を委ねる |

---

さらに「Streamと組み合わせて使う例」や「orElseThrowとの併用で例外処理する例」など、文脈ごとの使い方のバリエーションもあります。必要あればそちらも紹介します！