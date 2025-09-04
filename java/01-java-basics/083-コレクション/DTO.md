# DTO

# Q,

DTOへの変換というのは、Json形式にデータを再構成することと考えてよいでしょうか？

素晴らしい観点です、Hiroakiさん。

「DTO（Data Transfer Object）への変換」と「JSON形式に再構成すること」との関係は深いですが、**同一ではありません**。

以下に、違いと関係性をわかりやすく整理します。

---

## ✅ 結論：DTOとJSONは **目的とフェーズが異なる**

| 項目 | DTO（Data Transfer Object） | JSON（JavaScript Object Notation） |
| --- | --- | --- |
| 正体 | Javaの**クラス**（通常POJO） | **文字列形式のデータ構造** |
| 目的 | データ構造の**内部表現** | データ構造の**外部表現**（通信・保存） |
| 使用場面 | Javaアプリ内での層間データ転送 | クライアント⇔サーバ間の通信など |
| 変換方向 | Entity → DTO → JSON | DTO → JSON（Jacksonなどで自動変換） |

---

## ✅ 実務の流れで見る：DTOとJSONの関係性

```java
[DBデータ] ⇒ [Entity] 
         ⇒ [DTO]        ← Javaの構造体（POJO）に変換
         ⇒ [JSON]       ← Spring Bootなどで自動シリアライズされる
         ⇒ [HTTPレスポンス]
```

### ▶ 例（Javaコード）

```java
// Entity（DBマッピング）
@Entity
public class UserEntity {
    private Long id;
    private String name;
    private String password; // セキュリティ的に外に出さない
}

// DTO（公開用）
public class UserDto {
    private Long id;
    private String name;

    public UserDto(Long id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

### ▶ Controller（返却処理）

```java
@GetMapping("/users")
public ResponseEntity<List<UserDto>> getAllUsers() {
    List<UserEntity> entities = userRepository.findAll();

    List<UserDto> dtos = entities.stream()
        .map(e -> new UserDto(e.getId(), e.getName()))
        .collect(Collectors.toList());

    return ResponseEntity.ok(dtos); // ← JSONに自動変換されて返る
}
```

### ▶ フロントエンドに返るJSON

```java
[
  { "id": 1, "name": "Hiroaki" },
  { "id": 2, "name": "Alice" }
]
```

---

## ✅ DTOとJSONの補足関係

| DTOでできること | JSONとは違う特徴 |
| --- | --- |
| 不要なフィールドを除外（例：パスワード） | JSONにそのまま出さない |
| フィールド名を変更 | `@JsonProperty` で柔軟に変更可 |
| 計算済みフィールドを追加（例：年齢） | 内部ロジックで加工した値を含める |
| バリデーションや構造制約の注釈 | `@NotNull`, `@Size`など |

---

## ✅ Hiroakiさんの認識の調整ポイント

| 観点 | Hiroakiさんの認識 | 補足修正 |
| --- | --- | --- |
| DTO ＝ JSON？ | △（近い） | ❌ DTOはJavaの内部構造体、JSONは外部表現 |
| DTOの役割 | JSON化のため | ✔ ただしそれだけでなく、**内部構造の整理やセキュリティ制御の役割も** |
| DTO変換の目的 | データ再構成 | ✔ ただし主目的は**API設計と責務分離** |

---

## 🔚 まとめ

- **DTOは「アプリ内のデータ転送用オブジェクト」**
- **JSONは「通信や保存のためのフォーマット」**
- **DTOはJSON生成のために使われることが多いが、それが本質ではない**
- DTOを導入することで、**エンティティとレスポンスの責務分離・セキュリティ確保・柔軟な設計**が可能になる