# Unnamed → Named アクセス

# Q,

eclipse を使用してプロジェクトを立ち上げる場合、Unnamed module に配置される認識です。
java.Baseモジュールの利用は、Unnamed module からでも問題なくアクセスできると思いますが、他のモジュール(java.sqlなど)へアクセスする場合は、module-info.java へrequiresを記述する必要があるという認識はあっていますか？
また、java.sql などの既存モジュールは、exportsはされているのでしょうか？

はい、Hiroakiさんの認識は**ほぼ正確**です！

以下に、`Unnamed module`・`java.base`・`java.sql` などの扱いと `exports`/`requires` の関係を**正確に整理して解説**します。

---

## ✅ 結論：ご質問の要点と回答

| 項目 | 結論 |
| --- | --- |
| Eclipse で module-info.java を作らずにプロジェクト作成した場合 | ✅ そのプロジェクトは **Unnamed Module** に属する |
| Unnamed Module から `java.base` を使えるか | ✅ **常にアクセス可能（自動で読み込まれる）** |
| Unnamed Module から `java.sql` を使うには `requires` が必要か | ❌ **不要（Unnamed Module は全ての Named Module にアクセス可能）** |
| `java.sql` などの標準モジュールは `exports` されているか | ✅ **必要なパッケージはすでに `exports` 済み**（でないとアクセスできない） |

---

## 🔍 詳しく解説します

---

### 🔸 1. **Unnamed Module** の特徴

Unnamed Module とは：

- `module-info.java` を**持たないクラスやJAR**から成る暗黙的なモジュール
- 従来のクラスパス方式（`cp`）で扱われるコードは、Unnamed Module に分類される

🔑 **特徴的な振る舞い**：

| 項目 | 振る舞い |
| --- | --- |
| `java.base` へのアクセス | ✅ 常に可能（全モジュールが自動的に依存） |
| 他のモジュール（例：`java.sql`） | ✅ **Named Module 側が `exports` 済みならアクセス可能** |
| Named Module からの参照 | ❌ **Named Module 側から Unnamed Module を `requires` することはできない** |

---

### 🔸 2. `java.sql` などは exports 済み？

はい、**Java標準モジュール（`java.sql`, `java.xml`, など）では、利用が想定されるパッケージは `exports` 済みです**。

### 例：`java.sql` モジュールの module-info.java（抜粋）

```java
module java.sql {
    exports java.sql;
    exports javax.sql;
    // 他にも内部向けの opens や requires あり
}
```

つまり、次のようなコードは **Unnamed Module 内でも問題なく利用可能**：

```java
import java.sql.Connection;
import java.sql.DriverManager;
// ...
```

---

## ✅ 実用的なまとめ

| 使用ケース | `module-info.java` 必要？ | アクセスできる？ | 解説 |
| --- | --- | --- | --- |
| Eclipse の通常プロジェクト（Unnamed Module） | ❌ 不要 | ✅ `java.sql` OK | `java.sql` が `exports` 済み → Unnamed Module から直接使える |
| 明示的に module-info.java を作った Named Module | ✅ 必要 | 🚫 `requires` がなければ NG | `requires java.sql;` がなければ使えない |
| `java.base` の使用 | ❌ 常にOK | ✅ OK | すべてのモジュールに自動で含まれる |
| Unnamed Module を Named Module から参照 | 🚫 不可 | 🚫 不可 | Unnamed Module は **依存される対象にならない**（設計上の制約） |

---

## ✅ おまけ：module-info.java がある場合の例

```java
module com.example.myapp {
    requires java.sql;
}
```

➡ このように書くことで、`com.example.myapp` モジュール内のクラスで `java.sql.*` を利用できます。

---

ご希望があれば：

- Named Module → Unnamed Module の依存がなぜ不可なのか（図解）
- Eclipseでモジュール付きプロジェクトを作成する方法
- 標準モジュールの `exports` 情報を確認する手順（`jdeps` など）

などもご案内できます。気になるポイントはありますか？

# Q,

Unnamed Module が、すべてのNamed Moduleに無条件でアクセスできるのは、Moduleシステムを導入できないバージョンのシステムでも変更による問題発生を抑えるためという認識です。
そもそも、Moduleシステムを導入できないバージョンでも、Named Moduleを利用することになるのでしょうか？

はい、Hiroakiさんの認識はとても本質的で正確です。以下にその背景と意図を詳しく解説します。

---

## ✅ 結論

> Java 8以前のバージョンではモジュールシステム（Named/Unnamed Module）は存在しません。
> 
> 
> よって、**モジュールシステムを導入「できない」環境では、Named Module は一切利用されません。**
> 

---

## 🔍 そもそもモジュールシステムはどこから？

- Java 9（2017年リリース）で **Jigsawプロジェクト** により導入されました。
- それ以前（Java 8 など）では、クラスパスベースの管理のみが存在していました。

---

## ✅ 「Unnamed Module の特別扱い」の理由

> Java 9 以降においても、従来のコード（Java 8以前）を壊さずに動作させるために「Unnamed Module」が定義されています。
> 

### 🔸 背景と目的

| 項目 | 内容 |
| --- | --- |
| 既存システムとの互換性 | 多くのライブラリ・アプリが `module-info.java` を持たずに配布されているため |
| 移行を段階的にする必要があった | いきなりすべてのプロジェクトに `module-info.java` を強制できない |
| 移行時にNamed Moduleと共存できる設計 | → Unnamed Module から Named Module に**無条件アクセス可能**とした |

---

## 🔸 では「モジュールシステムを導入できない」バージョンでは？

| Javaバージョン | モジュールシステム使用可？ | Named Module 利用可？ | 補足 |
| --- | --- | --- | --- |
| Java 8以前 | ❌ 不可 | ❌ 利用できない | `module-info.java` は存在しない |
| Java 9以降 | ✅ 可 | ✅ 利用可能 | `module-info.java` により構成管理が可能 |

---

## ✅ まとめ

| 観点 | 内容 |
| --- | --- |
| Java 8以前 | モジュールシステムなし。すべてクラスパスで管理 |
| Java 9以降 | モジュールシステムあり。`module-info.java` による明示的依存制御が可能 |
| Unnamed Module の設計目的 | 既存コード（非モジュール）をそのまま動作させるため |
| Named Module の強制はあるか？ | ❌ 強制ではなく、段階的に導入可能 |

---

## ✅ Hiroakiさんのご認識に対する回答

> 「モジュールシステムを導入できないバージョンでも、Named Moduleを使うか？」
> 

➡ **使えません。**

Java 9以降でのみ `module-info.java` による **Named Module の定義と管理が可能**です。

---

ご希望であれば：

- Java 8 と 9 のコード構成と実行の違い（図付き比較）
- 実務でのモジュール導入パターン（段階的移行戦略）
- Eclipse や Maven でのモジュール非対応と対応の使い分け

などもご案内できます！必要があればお知らせください。