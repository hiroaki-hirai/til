# try 文の外で初期化

# Q,

try-with-resourcesにおいて、リソースの初期化式をtry() の()内に記述する場合とtry文の上に記述する場合でどのような違いがありますか？

Hiroakiさんの質問は非常に鋭い視点です。

「`try-with-resources` において、リソースの**初期化を `try()` のカッコ内に記述する場合**と、**その上（外側）で記述する場合**の違いを、以下に体系的に解説します。

---

## ✅ 結論（バージョン依存あり）

| 記述場所 | Java 8まで | Java 9以降 |
| --- | --- | --- |
| `try (Type name = new ...)` | ✅ OK | ✅ OK |
| try 文の外で宣言＆初期化 → `try (name)` | ❌ コンパイルエラー | ✅ OK（条件あり：finalまたはeffectively final） |

---

## 🔹 ① `try()` の **中で初期化（従来の記述）**

```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    System.out.println(br.readLine());
}
```

### ✅ 特徴

- **Java 7以降すべてのバージョンで使用可能**
- `br` は `try` ブロックのスコープ内限定
- 処理終了時に **自動で `br.close()` が呼ばれる**

---

## 🔹 ② `try` 文の外で初期化 → `try (変数名)`（Java 9以降）

```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
try (br) {
    System.out.println(br.readLine());
}
```

### ✅ 特徴（Java 9以降）

- リソースを**事前に宣言・初期化**し、その変数を `try()` に渡す
- 変数は **`final` または effectively final（実質的に再代入なし）** である必要がある
- `try` ブロック外でもスコープを共有できる（例：catch や finally 内で使える）

---

## 🔴 Java 8以前では使えない（構文エラー）

```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
try (br) {  // ❌ Java 8ではコンパイルエラー
    System.out.println(br.readLine());
}
```

---

## 🔸 実務上の使い分けポイント

| 観点 | `try()` 内で初期化 | `try` 外で初期化（Java 9以降） |
| --- | --- | --- |
| コードの簡潔さ | ✅ すっきりしている | やや冗長になる |
| 複数スコープで使う（tryの外でも使いたい） | ❌ 不可（スコープ内限定） | ✅ 可能（ただしfinal性必要） |
| エラー調査（デバッグ） | 変数を見づらい | 外で定義すれば監視しやすい |
| Java 7 / 8 互換 | ✅ 使用可 | ❌ 非対応 |

---

## 🔸 補足：effectively final とは？

```java
BufferedReader br = new BufferedReader(new FileReader("file.txt")); // ← 再代入していない
try (br) {
    ...
}
```

- `final` 修飾子がなくても、「一度だけ初期化され、再代入されていない」変数は `effectively final` として扱われます。
- Java 9以降であれば、`try(br)` と書くことができます。

---

## ✅ まとめ

| 質問 | 回答 |
| --- | --- |
| `try()` の中と外でのリソース初期化、何が違う？ | → **Javaのバージョン、スコープ、final性** によって扱いが異なる |
| Java 8までなら？ | → **必ず `try()` 内で初期化が必要** |
| Java 9以降なら？ | → **外で宣言・初期化し、`try()` に渡すことが可能（final or effectively final）** |
| 使い分けのポイントは？ | → リソースのスコープや見通し、テストのしやすさなど |

---

ご希望があれば、Java 9以降のこの機能を活用した応用例（ログ記録付きのリソース管理など）もご紹介できます！