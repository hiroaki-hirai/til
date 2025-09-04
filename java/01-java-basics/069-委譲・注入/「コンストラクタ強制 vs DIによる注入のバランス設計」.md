# 「コンストラクタ強制 vs DIによる注入のバランス設計」

これは非常に実務的かつ設計判断が問われるテーマです。

以下に「**コンストラクタ強制 vs DIによる注入**」の違いと、それぞれの**長所・短所・適用基準**を整理した上で、**バランス設計の考え方**をお伝えします。

---

## ✅ 1. 定義と違い

| 項目 | コンストラクタ強制 | DI（Dependency Injection）による注入 |
| --- | --- | --- |
| 方法 | new 時に必須の値や依存オブジェクトを引数で渡す | フレームワークや手動で依存を外部から渡す |
| 主な利用ケース | **必須の構成要素の初期化**（例：ID、テーブル名） | **外部リソースや戦略オブジェクトの注入**（例：Service, DAO） |
| 責務の所在 | **自クラスが依存の生成・管理責任も持つ** | **依存のライフサイクルを外部に委譲**（疎結合） |

---

## ✅ 2. 両者の比較表

| 観点 | コンストラクタ強制 | DIによる注入 |
| --- | --- | --- |
| 明示性 | ✅ 高い（何が必要か一目でわかる） | △ 設定ファイルやDI設定を見ないと不明 |
| 柔軟性 | ❌ 低い（固定された依存） | ✅ 高い（差し替えやテストが容易） |
| テストのしやすさ | ❌ モックが差し込みにくい | ✅ 任意の実装を注入できる |
| 意図の強制 | ✅ 強い（初期値が必須と分かる） | △ 任意注入の場合、nullにもなりうる |
| フレームワーク依存 | ❌ なし（純粋なJavaで完結） | ✅ あり（SpringなどのDIコンテナが必要） |

---

## ✅ 3. 具体例で理解

### ◾ コンストラクタ強制の例（必須情報）

```java
abstract class AbstractRepository {
    protected final String tableName;

    public AbstractRepository(String tableName) {
        if (tableName == null || tableName.isBlank()) {
            throw new IllegalArgumentException("tableName is required");
        }
        this.tableName = tableName;
    }
}
```

→ ✅ 絶対に必要な情報を**設計で強制**

---

### ◾ DIによる注入の例（差し替え可能な依存）

```java
class UserService {
    private final UserRepository userRepository;

    @Inject  // Springなどで自動注入
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void register(User user) {
        userRepository.save(user);
    }
}
```

→ ✅ `UserRepository` をテスト用モックに差し替え可能

---

## ✅ 4. バランス設計：どう使い分けるか？

### 🔸 判断基準：依存の性格で使い分ける

| 依存の性格 | 手段 | 理由 |
| --- | --- | --- |
| 絶対に必要、null不可、ロジックに必須 | コンストラクタ強制 | 不正状態を防ぎ、オブジェクトの不変条件を保証できる |
| 可変・差し替え可能・テスト対象 | DI（コンストラクタ or セッター） | 柔軟な切り替えと疎結合を実現 |

---

### 🔸 現実的なバランス例：両者の併用

```java
class OrderProcessor {
    private final OrderValidator validator;      // 必須（new時に保証）
    private PaymentService paymentService;       // 差し替え可能（セッター注入）

    public OrderProcessor(OrderValidator validator) {
        if (validator == null) {
            throw new IllegalArgumentException("validator is required");
        }
        this.validator = validator;
    }

    @Inject
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

→ ✅ 不変の依存は**コンストラクタ強制**で守り、

→ ✅ 実装差し替え前提の依存は**DIで柔軟に注入**

---

## ✅ 5. バランス設計の鉄則

| 原則 | 内容 |
| --- | --- |
| **不変条件の強制はコンストラクタで行う** | null許容でない必須依存は`new`時点で確実に設定 |
| **拡張・テスト・切り替え前提の依存はDIで持つ** | 特にフレームワーク/外部リソース/戦略系オブジェクト |
| **依存の“重さ”で設計する** | 軽い依存は委譲/注入、重い依存は管理・制御付きのコンストラクタへ |

---

## ✅ まとめ

| ポイント | コンストラクタ強制 | DIによる注入 |
| --- | --- | --- |
| 使用目的 | **必須依存、null防止、状態初期化** | **差し替え可能、疎結合、テスト支援** |
| バランス設計のコツ | 両者を**「依存の性質・目的」で使い分ける** |  |
| 結論 | 「不変条件」は強制し、「戦略」は注入する |  |

---

ご希望であれば、「Springでのコンストラクタ vs セッター vs フィールド注入の選び方」や、「実務でやりがちなアンチパターン」なども併せてご紹介できます！