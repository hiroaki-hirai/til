# random()

`Math.random()` は Java において、**0以上1未満の疑似乱数（double型）を生成するための標準メソッド**です。

とてもよく使われるメソッドで、簡単にランダムな数値を取得したいときに便利です。

---

## ✅ `Math.random()` の基本情報

| 項目 | 内容 |
| --- | --- |
| クラス | `java.lang.Math`（インポート不要） |
| メソッド名 | `random()` |
| 用途 | **0以上1未満のランダムな `double` 値を返す** |
| 修飾子 | `public static`（staticメソッド） |
| 戻り値の型 | `double` |

---

## 🔹 シグネチャ（定義）

```java
public static double random()
```

---

## 🔹 戻り値の範囲

```java
0.0 <= Math.random() < 1.0
```

- **0.0 を含み、1.0 は含まない**
- `double` 型の乱数なので、小数値で返されます

---

## 🔹 使用例

```java
System.out.println(Math.random()); // → 例: 0.7156340198...
```

---

## 🔸 よくある使い方（整数に変換する）

### ✅ 0以上n未満の整数を取得

```java
int n = 10;
int r = (int)(Math.random() * n); // 0～9
```

### ✅ min以上max以下の整数を取得

```java
int min = 5;
int max = 10;
int r = (int)(Math.random() * (max - min + 1)) + min; // 5～10
```

---

## 🔹 `Math.random()` vs `Random` クラス

| 比較項目 | `Math.random()` | `java.util.Random` |
| --- | --- | --- |
| 簡易さ | 非常に簡単（static呼び出し） | インスタンス化が必要 |
| 戻り値の型 | 常に `double`（0以上1未満） | `nextInt()`, `nextDouble()` など多様 |
| シードの指定 | 不可 | 可（再現性ある乱数を生成可能） |
| 用途 | 簡単な乱数（試験問題など） | 本格的なランダム処理（ゲーム、統計など） |

---

## 🔸 補足：ランダムな値の例

| 使用例 | 結果の例 |
| --- | --- |
| `Math.random()` | 0.573812312 |
| `(int)(Math.random() * 10)` | 0～9の整数 |
| `(int)(Math.random() * 6) + 1` | サイコロ（1～6） |

---

## ✅ 注意点

| 注意点項目 | 内容 |
| --- | --- |
| 上限は「未満」 | 1.0は**含まれない**（境界でミスしやすい） |
| 戻り値は `double` | 整数にしたいときは明示的に `(int)` でキャスト |
| 偏りが出ることもある | 擬似乱数なので**完全なランダムではない** |
| シードの固定はできない | `Random` クラスを使うとシード設定可能 |

---

## ✅ まとめ

| ポイント | 説明 |
| --- | --- |
| `Math.random()` | 0以上1未満の `double` を返す |
| 範囲の調整 | `*範囲 + min` の式で整数にも変換できる |
| 戻り値の型 | 常に `double`。`int` にするにはキャスト必要 |
| 本格的乱数用途 | `java.util.Random` クラスや `ThreadLocalRandom` を使うのがベター |

---

## 🔺 `Math.random()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：戻り値の型

```java
var r = Math.random();
System.out.println(((Object)r).getClass().getSimpleName());
```

A. `Float`

B. `Double`

C. `Integer`

D. `Long`

**答え**：B. `Double`

**解説**：

`Math.random()` は `double` 型の値を返します（float や int ではない）。

→ `var` を使った場合も、型は `double` と推論され、ボクシングされれば `Double` になります。

---

### ❓ 問題 2：戻り値の範囲

```java
double r = Math.random();
System.out.println(r >= 0.0 && r <= 1.0);
```

A. 常に true

B. 常に false

C. 条件によって変わる

D. コンパイルエラー

**答え**：C. 条件によって変わる（たまに false）

**解説**：

戻り値の範囲は **0.0 以上 1.0 未満（[0.0, 1.0)）**。

したがって、`r == 1.0` になることは**絶対にない**ので、`r <= 1.0` は**厳密には誤り**になる可能性があります。

**正しくは： `r >= 0.0 && r < 1.0`**

---

### ❓ 問題 3：整数化の方法（サイコロ）

```java
int dice = (int)(Math.random() * 6 + 1);
System.out.println(dice);
```

このコードが返す値の範囲は？

A. 0～5

B. 1～6

C. 1～7

D. 0～6

**答え**：B. 1～6

**解説**：

- `Math.random()` → `[0.0, 1.0)`
- `6` → `[0.0, 6.0)`
- `(int)` → `[0, 5]`（小数点切り捨て）
- `+1` → **`[1, 6]`**

✅ これは**正しい「サイコロ」ロジック**

---

### ❓ 問題 4：不正な整数化（オフバイワンエラー）

```java
int r = (int)Math.random() * 10;
System.out.println(r);
```

出力の可能性として正しいのは？

A. 0～9

B. 0～10

C. 常に 0

D. コンパイルエラー

**答え**：C. 常に 0

**解説**：

`(int)Math.random()` は `0` になる（`Math.random()` < 1.0）→ `0 * 10` = `0`

✅ 正しくは：`(int)(Math.random() * 10)`

---

### ❓ 問題 5：Random クラスとの違い

```java
Random rand = new Random();
System.out.println(rand.random());
```

このコードはどうなる？

A. ランダムな値を出力

B. コンパイルエラー

C. 実行時エラー

D. 常に 0.0

**答え**：B. コンパイルエラー

**解説**：

`Random` クラスには `random()` というメソッドは**存在しません**。

`nextDouble()` を使う必要があります。

```java
rand.nextDouble(); // 0.0以上1.0未満の double
```

---

## ✅ よくある誤解まとめ

| 誤解・ミス内容 | 正しい理解 |
| --- | --- |
| `Math.random()` の戻り値は `int`？ | ❌ `double` 型 |
| `Math.random()` の最大値は `1.0`？ | ❌ `1.0` は含まれない（未満） |
| `(int)Math.random() * n` で整数化できる？ | ❌ 必ず 0 になる。正しくは `(int)(Math.random() * n)` |
| `Random` クラスにも `random()` がある？ | ❌ `nextInt()`, `nextDouble()` などを使う |

---

## ✅ Java 乱数生成クラス 比較表

| 比較項目 | `Math.random()` | `Random`（`java.util`） | `ThreadLocalRandom`（`java.util.concurrent`） |
| --- | --- | --- | --- |
| 利用の簡易さ | ◎（static メソッドで即使用可） | ○（インスタンス化が必要） | ○（static メソッド呼び出し） |
| スレッドセーフ | ×（内部で `Random` を共有） | ×（明示的に同期が必要） | ◎（各スレッドごとに独立して高速） |
| Javaバージョン | JDK 1.0～ | JDK 1.1～ | JDK 1.7～ |
| シードの設定 | ×（できない） | ◎（コンストラクタで指定可能） | ×（シード指定不可・内部で自動処理） |
| 型の多様性 | ×（`double` のみ） | ◎（`nextInt()`, `nextDouble()`, など） | ◎（`nextInt()`, `nextDouble()`, など） |
| 性能（スレッド環境） | △（競合が起きやすい） | △（共有すると遅くなる） | ◎（スレッドローカルで高速） |
| 簡単な用途（試験・教材など） | ◎ | ○ | △（シンプルな用途ではやや複雑） |
| 主な使用シーン | 簡易的な乱数 | 一般的なランダム処理 | 並列処理・並行性が重要なケース |

---

## 🔹 使用例コードの比較

### 🔸 Math.random()

```java
double r = Math.random(); // 0.0 ～ 1.0
int i = (int)(Math.random() * 10); // 0 ～ 9
```

---

### 🔸 java.util.Random

```java
Random rand = new Random();
int i = rand.nextInt(10);       // 0 ～ 9
double d = rand.nextDouble();   // 0.0 ～ 1.0
```

---

### 🔸 java.util.concurrent.ThreadLocalRandom

```java
ThreadLocalRandom rand = ThreadLocalRandom.current();
int i = rand.nextInt(10);       // 0 ～ 9
double d = rand.nextDouble();   // 0.0 ～ 1.0
```

---

## ✅ まとめ：選び方ガイド

| 条件・用途 | 推奨されるクラス | 理由 |
| --- | --- | --- |
| とりあえず簡単に乱数を使いたい | `Math.random()` | 1行で済む、補助的な使用に十分 |
| 同じ乱数列を再現したい（シード） | `Random` | シード値の指定が可能 |
| マルチスレッドで高速な乱数が必要 | `ThreadLocalRandom` | スレッドごとに独立していて競合しない |
| JDK 1.6以下の互換性を意識したい | `Random` | `ThreadLocalRandom` は JDK 1.7 以降のみ対応 |