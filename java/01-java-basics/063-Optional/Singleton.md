# Singleton

# Q,

```java
    // 空の Optional（シングルトン）
    private static final Optional<?> EMPTY = new Optional<>(null);
```

シングルトンとは何ですか？

とても良い質問です、Hiroakiさん。

---

## ✅ 結論：**シングルトン（Singleton）とは、「インスタンスがただ1つだけであることを保証する設計パターン」**です。

---

## 🔍 詳細な解説

### ◉ シングルトンの定義

「**あるクラスに対して、1つのインスタンスしか存在させないようにする**」というデザインパターンのこと。

### ◉ なぜ使うのか？

同じ状態や設定・共有情報を持つオブジェクトが、**複数存在しては困る**ようなケースで使われます。

---

## 🧱 Optional におけるシングルトン：`EMPTY`

```java
private static final Optional<?> EMPTY = new Optional<>(null);
```

これは `Optional.empty()` を呼んだときに返されるインスタンスです。

### ✳ 重要なポイント：

- `Optional.empty()` を何回呼んでも **毎回同じ `EMPTY` インスタンス** が返されます。
- `new Optional<>(null)` を **毎回作らず、1つだけ使い回す**ようにしている。
- **無駄なインスタンス生成を避けて効率化**している（== メモリ＆パフォーマンス最適化）。

---

## 🔄 イメージ

```java
Optional<String> a = Optional.empty();
Optional<Integer> b = Optional.empty();

// 実体は同じ
System.out.println(a == b);  // true（同じインスタンス）

// 型は異なるように見えても、実装上は1つの EMPTY をキャストして使い回している
```

---

## 🎓 よく使われるシングルトンの例

| クラス | 用途例 |
| --- | --- |
| `Runtime.getRuntime()` | JVMの実行環境（Java標準） |
| `Logger.getInstance()` | ロギング（設計でよく見る） |
| `Optional.empty()` | 空オプショナル（今回のケース） |

```java
public final class Singleton {
    private static final Singleton instance = new Singleton();  // 1つだけ作る

    private Singleton() {}  // 外部から new できないようにする

    public static Singleton getInstance() {
        return instance;  // いつも同じインスタンスを返す
    }
}
```

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 用語 | Singleton（シングルトン） |
| 意味 | インスタンスが1つしか存在しない設計 |
| 目的 | 状態の一貫性、資源節約、不要な重複回避 |
| Optionalにおける例 | `Optional.empty()` が返す `EMPTY` はシングルトン |

---

ご希望であれば、シングルトンを**実際にコードで実装する例（マルチスレッド対応含む）**もご紹介できます。ご興味ありますか？