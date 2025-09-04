# parseInt()

はい、`Integer.parseInt()` は Java の数値変換で非常によく使われるメソッドであり、**試験でも実務でも登場頻度が高い重要API**です。

以下に体系的に解説します。

---

## ✅ `Integer.parseInt()` の基本定義

```java
public static int parseInt(String s)
public static int parseInt(String s, int radix)
```

```java
public static int parseInt(String s) throws NumberFormatException {
        return parseInt(s,10);
```

---

## ✅ 主な用途

- **文字列（例: `"123"`）を `int` 型の数値に変換する**
- 入力値が数値文字列であることを前提とする（非数値であれば例外が発生）

---

## ✅ 使用例

```java
int i = Integer.parseInt("123");
System.out.println(i);  // → 123
```

---

## ✅ 基数（radix）指定バージョンの使用例

| 基数 | 例 | 結果 |
| --- | --- | --- |
| 2 | `"1010"` | 10 |
| 8 | `"10"` | 8 |
| 10 | `"10"` | 10 |
| 16 | `"FF"` | 255 |

---

## ✅ エラー発生例と注意点

### ① 非数値文字列

```java
Integer.parseInt("abc");  // ❌ NumberFormatException
```

### ② 空文字やnull

```java
Integer.parseInt("");     // ❌ NumberFormatException
Integer.parseInt(null);   // ❌ NullPointerException
```

---

## ✅ よくある誤解と注意点

| 誤解または落とし穴 | 実際の挙動 |
| --- | --- |
| `"010"` → 8進数になる？ | ❌ Java 7以降では **常に10進数**として扱う |
| `"0x10"` などが使える？ | ❌ `parseInt` では不可。`decode()` を使うべき |
| `"2147483648"` はOK？ | ❌ `int` の上限を超えるため `NumberFormatException` |
| `radix = 1` または `>36` | ❌ `IllegalArgumentException` が発生 |

---

## ✅ parseInt vs valueOf の違い

| 特徴 | `Integer.parseInt()` | `Integer.valueOf()` |
| --- | --- | --- |
| 戻り値の型 | `int`（プリミティブ型） | `Integer`（オブジェクト型） |
| オートボクシング | ❌ なし | ✅ あり（内部で `parseInt` を使用） |
| キャッシュ活用 | ❌ 使わない | ✅ -128～127はキャッシュ再利用 |
| 典型的な用途 | 演算や比較目的の数値変換 | コレクションなどオブジェクト型が必要な場面 |

---

## ✅ 補足：数値以外を扱うなら？

- `Double.parseDouble("3.14")` → 浮動小数点対応
- `Long.parseLong("12345678901")` → long対応
- `Integer.decode("0x10")` → `"0x"`, `"0"` などの接頭辞付きに対応

---

## ✅ 試験で出やすいチェックポイント

| チェック項目 | 試験頻出度 |
| --- | --- |
| `"010"` は8進数ではなく10進数になる | 高 |
| `"0x10"` を `parseInt()` で処理すると失敗 | 高 |
| nullや空文字の例外種別（NFE/NPE） | 中〜高 |
| `parseInt()` と `valueOf()` の違い | 中 |

---

## 🔚 まとめ

| 特性 | 内容 |
| --- | --- |
| 処理内容 | 文字列 → `int` への変換 |
| 失敗時の挙動 | `NumberFormatException` / `NullPointerException` |
| 基数対応 | `parseInt(String s, int radix)` で指定可能 |
| よくある代替 | `Integer.valueOf()` や `decode()` |

承知しました。以下に Java Silver 試験レベルに準拠した **`Integer.parseInt()` の○×形式および選択式の問題集（10問）** を用意しました。

**基本・例外・基数（radix）・valueOfとの違い**まで網羅しています。

---

## ✅ `Integer.parseInt()` 問題集（○×形式 & 選択式）

---

### ❶ 【基本変換】

```java
int i = Integer.parseInt("123");
System.out.println(i == 123);
```

Q: このコードはコンパイル・実行ともに成功し、true を出力する。

✅ 正解：**○**

---

### ❷ 【非数値文字列】

```java
int i = Integer.parseInt("abc");
```

Q: 上記コードは `"abc"` を `int` に変換できないため、**実行時例外が発生する**。

✅ 正解：**○（NumberFormatException）**

---

### ❸ 【8進数と誤解しやすいリテラル】

```java
int i = Integer.parseInt("010");
System.out.println(i);
```

Q: このコードは `8` を出力する。

✅ 正解：**×**

📘 `"010"` は **Java 7以降、10進数扱い** → 出力は `10`

---

### ❹ 【基数指定（16進数）】

```java
int i = Integer.parseInt("FF", 16);
System.out.println(i);
```

Q: 上記コードの出力は `255` である。

✅ 正解：**○**

---

### ❺ 【基数の制限】

```java
int i = Integer.parseInt("10", 1);
```

Q: 上記コードは `"10"` を1進数としてパースするため、出力は常に `1` になる。

✅ 正解：**×**

📘 `radix` が 2〜36 以外 → `IllegalArgumentException`

---

### ❻ 【nullを渡す】

```java
String s = null;
int i = Integer.parseInt(s);
```

Q: `null` を渡すと `NumberFormatException` が発生する。

✅ 正解：**×**

📘 `null` の場合 → `NullPointerException`

---

### ❼ 【空文字列を渡す】

```java
int i = Integer.parseInt("");
```

Q: 空文字列を渡すと `NumberFormatException` が発生する。

✅ 正解：**○**

---

### ❽ 【Integer.valueOf との違い】

Q: `Integer.valueOf("123")` の戻り値は `int` 型である。

✅ 正解：**×**

📘 `valueOf()` は **`Integer`（ラッパー型）** を返す

---

### ❾ 【最大値＋1をパース】

```java
int i = Integer.parseInt("2147483648");
```

Q: 上記は `int` の最大値（2^31−1）+1 をパースするため、**オーバーフローするが正常実行される**

✅ 正解：**×**

📘 `NumberFormatException` が発生する（範囲外）

---

### 🔟 【基数指定での16進数プレフィックス】

```java
int i = Integer.parseInt("0x10", 16);
```

Q: このコードは `"0x10"` を16進数として扱い、出力は `16` となる。

✅ 正解：**×**
📘 `NumberFormatException` が発生する（範囲外）

📘 `"0x"` のような接頭辞は `parseInt()` では使えない → `decode()` を使うべき

---

## ✅ 問題まとめ用チェックリスト

| 項目 | 理解できた？ |
| --- | --- |
| `"010"` は8進数ではなく10進数 | ✅ |
| `radix=1` は例外になる | ✅ |
| `"abc"` や `""` は NFE | ✅ |
| `null` は NPE | ✅ |
| `"0x10"` は `decode` を使う | ✅ |
| `valueOf()` の戻り値は Integer型 | ✅ |