# java.Base など

# Q,

java.Baseモジュールは、exportsしなくても公開される認識ですが、同じように指定なしで公開などされるモジュールは他にありますか？

とても鋭い質問です、Hiroakiさん。

---

## ✅ 結論

> java.base モジュールだけが特別に、常に読み込まれ、常に公開されるモジュールです。
> 
> 
> 他の標準モジュール（例：`java.sql`, `java.desktop` など）は、**自動で公開されることはありません**。
> 
> **使用する側で `requires` を明示**する必要があります。
> 

---

## 🔷 `java.base` モジュールとは？

| 項目 | 内容 |
| --- | --- |
| モジュール名 | `java.base` |
| 特徴 | Javaのすべてのモジュールが自動的に依存（`requires java.base`）している |
| 例に含まれるAPI | `java.lang`, `java.io`, `java.util`, `java.nio` など多数 |
| `module-info.java` に書く必要 | ❌ 不要（すでに暗黙的に含まれている） |

---

### ✅ つまり：

```java
module com.example.myapp {
    // requires java.base; ← 書かなくても自動で有効！
}
```

---

## 🔶 その他の標準モジュールはどうなる？

| モジュール名 | 自動で読み込まれる？ | `requires` 必要？ |
| --- | --- | --- |
| `java.base` | ✅ はい（常に） | ❌ 不要 |
| `java.sql` | ❌ いいえ | ✅ 必要 |
| `java.desktop` | ❌ いいえ | ✅ 必要 |
| `java.xml` | ❌ いいえ | ✅ 必要 |
| `jdk.unsupported` | ❌ いいえ | ✅ 必要（非公開API） |

---

## ✅ 自動読み込みされないモジュールを使う場合の例

```java
module com.example.dbapp {
    requires java.sql; // 明示的に記述しないとコンパイルエラーになる
}
```

---

## 🔍 よくある誤解

| 誤解 | 正しい理解 |
| --- | --- |
| `java.lang` などは自動でインポートされる→全て公開されている | ✅ 正しい。ただしそれは `java.base` の一部だから |
| 他の `java.*` パッケージも全部自動で使える | ❌ 間違い → `requires` での明示が必要 |
| 複数モジュールを使うときは全部自動でリンクされる | ❌ 間違い → `module-info.java` で `requires` を管理する必要がある |

---

## ✅ まとめ

| モジュール | 特別な扱い | `requires` の必要性 | 説明 |
| --- | --- | --- | --- |
| `java.base` | ✅ 常に読み込まれる | ❌ 不要 | 全モジュールにデフォルトで含まれる（`java.lang`, `java.util` 等） |
| 他の `java.*` モジュール | ❌ 通常のモジュール | ✅ 必要 | `java.sql`, `java.xml` などを使うには明示的に依存関係を書く必要がある |

---

ご希望があれば：

- よく使う標準モジュールの一覧とその用途
- `module-info.java` に記述する `requires` 練習問題
- Eclipse や `javac` を使った依存エラーの再現パターン

などもご案内できます！どうしましょうか？