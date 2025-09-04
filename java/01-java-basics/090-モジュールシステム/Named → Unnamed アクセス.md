# Named → Unnamed アクセス

# Q,

Named Module → Unnamed Module の依存がなぜ不可なのか解説してください。

素晴らしいご質問です、Hiroakiさん。

モジュールシステムの設計思想の核心をついた問いですね。

---

## ✅ 結論：

> Named Module → Unnamed Module の依存は不可です。
> 
> 
> なぜなら、**Unnamed Module は「名前がない」ため、明示的に依存を記述できず、依存関係の整合性を保証できないから**です。
> 

---

## 🔍 背景と設計思想

Java 9 以降のモジュールシステム（JPMS: Java Platform Module System）は、次の目的で設計されました：

| 目的 | 内容 |
| --- | --- |
| ① 明示的な依存関係の宣言 | `module-info.java` を使って `requires`, `exports` を記述 |
| ② カプセル化の強化 | 外部に公開すべきパッケージだけを `exports` で制御 |
| ③ 予測可能な構成 | ビルド・実行時にすべての依存関係を**事前に把握・検証できる**ようにする |

この中で、**「予測可能な構成」＝グラフ化可能な依存関係**を成立させるには、「名前で参照できること」が絶対条件になります。

---

## 🔸 Named Module → Unnamed Module ができない理由

| 理由 | 解説 |
| --- | --- |
| ❌ 名前がないから `requires` できない | `requires ???;` という記述が不可能 |
| ❌ 依存グラフに含められない | `Unnamed Module` は他のモジュールからの**参照対象として表現不能** |
| ❌ カプセル化違反になる可能性がある | Unnamed Module は全パッケージを `exports` しているのと同じ |
| ✅ 安全性と再利用性を守るためにブロックされている | モジュール構成の**一貫性を守るための制約** |

---

## 🔸 例：Named Module → Unnamed Module は記述不可

```java
// com.example.library の module-info.java
module com.example.library {
    requires ???; // Unnamed Module に対しては書けない！
}
```

➡ `Unnamed Module` には名前がないため、参照のしようがありません。

---

## ✅ 設計上の目的：**一方向の互換性**

| 経路 | 可否 | 理由 |
| --- | --- | --- |
| ✅ Unnamed → Named Module | ✅ OK | 互換性維持のため、既存コードが標準モジュールを自由に使えるようにする |
| ❌ Named → Unnamed Module | ❌ NG | 名前がないため参照不可能。カプセル化もできず、安全でない |

---

## 🔍 どうしても Named Module から従来のライブラリ（Unnamed）を使いたいときは？

その場合の対策は以下の通り：

### ✅ 対策①：**そのライブラリを automatic module に変換する**

- `.jar` ファイルを `-module-path` に置くだけで、**自動モジュール（automatic module）** として扱われる
- 自動で仮のモジュール名が付き、`requires` できるようになる

```java
# libs/guava.jar → 仮想モジュール名 com.google.common
module com.example.myapp {
    requires com.google.common;
}
```

📌 注意：あくまで仮のモジュール名なので、安定性・将来性には注意が必要

---

### ✅ 対策②：**モジュール対応版のライブラリを使う**

- GradleやMavenでは最近のライブラリは `module-info.java` を持っているものも増えている
- これにより Named Module 間で正式に依存関係を構築できる

---

## ✅ まとめ

| 質問内容 | 回答 |
| --- | --- |
| なぜ Named Module → Unnamed Module は不可か？ | 名前がないため参照できず、依存グラフに含められないから |
| Unnamed Module は自由に Named Module を使えるのか？ | ✅ はい。後方互換性を保つために許可されている |
| 解決策は？ | 📌 自動モジュール化 または 📌 正式なモジュールを使う |

---

ご希望があれば：

- automatic module の作り方と使い方
- Eclipse や CLI で Named ↔ Unnamed の混在プロジェクトを動かす演習
- Gradle/Maven でモジュール対応する手順

などもご紹介できます！気になるトピックはありますか？