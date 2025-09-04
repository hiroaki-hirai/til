# @Deprecated と @SuppressWarnings

# Q.

@Deprecated と @SuppressWarnings の違い、棲み分けなどまとめてください。

以下に、`@Deprecated` と `@SuppressWarnings` の**違い**や**役割の棲み分け**を、わかりやすく整理して解説します。

---

## ✅ 結論：一言でいうと

| アノテーション | 役割 |
| --- | --- |
| `@Deprecated` | **「使わないで」という警告を出す側** |
| `@SuppressWarnings` | **「警告しないで」と抑制する側** |

---

## 🧩 両者の比較まとめ

| 項目 | `@Deprecated` | `@SuppressWarnings` |
| --- | --- | --- |
| 意味 | この要素は非推奨です | 指定した警告を表示しない |
| 用途 | 古くなったAPIに警告をつける | コンパイラの警告を一時的に抑制する |
| 対象 | クラス、メソッド、フィールドなど | メソッド、ローカル変数、クラスなど |
| 警告の出し手 or 抑制側 | **出し手**（警告を発する） | **抑制側**（警告を受け入れて消す） |
| よく使われるシーン | 古いAPIを段階的に廃止したいとき | 非推奨APIや未チェックキャストなどで意図的に警告を無視したいとき |
| 使用例 | `@Deprecated(since="19", forRemoval=true)` | `@SuppressWarnings("deprecation")` |

---

## 🎯 使用イメージの違い

### `@Deprecated`（提供者側）

```java
@Deprecated(since = "19", forRemoval = true)
public void oldMethod() {
    // 古い処理
}
```

- これは「**このメソッドはもう使わないでください**」という開発者へのサイン。
- 使用すると **警告が表示される**（IDEやjavacで）。

---

### `@SuppressWarnings`（利用者側）

```java
@SuppressWarnings("deprecation")
public void useOldMethod() {
    oldMethod();  // ← 警告が抑制される
}
```

- これは「**この警告は理解していて、無視したい**」という意思表示。
- `@Deprecated` なAPIの使用や、uncheckedなキャストなどに使われる。

---

## 🔄 関係性（図式）

```java
           ┌────────────┐
           │            │
           │ @Deprecated│ ← 警告を出す（供給者）
           │            │
           └────┬───────┘
                ↓ 警告
           ┌────┴───────┐
           │            │
           │@SuppressWarnings("deprecation") │ ← 警告を抑制（利用者）
           │            │
           └────────────┘
```

---

## 🛑 棲み分けポイント

| ケース | 使用するアノテーション | 理由 |
| --- | --- | --- |
| 古いAPIを廃止予定 | `@Deprecated` | 他の開発者に警告を伝える |
| 非推奨APIをやむなく使う | `@SuppressWarnings("deprecation")` | コンパイラ警告を一時的に抑制する |
| 型安全でないキャスト | `@SuppressWarnings("unchecked")` | チェック済みであることを明示するため |
| 設計ミスなどの非推奨APIを避けさせたい | `@Deprecated(forRemoval=true)` | 強く使用中止を促す |

---

## ✏️ まとめ

| 特性 | `@Deprecated` | `@SuppressWarnings` |
| --- | --- | --- |
| 提供側 or 利用側 | 提供側 | 利用側 |
| 意図 | 使用をやめてほしい | 警告は理解しているので無視してほしい |
| 相互補完の関係 | 警告を出す | 警告を受け入れて非表示にする |

---

ご希望があれば、実際の使用例を元にした「組み合わせ使用例」や「IDEでの表示結果」も紹介できます。