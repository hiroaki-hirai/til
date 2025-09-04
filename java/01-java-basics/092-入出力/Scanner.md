# Scanner

# Q,

Scannerクラスの主なメソッドをまとめてください。

もちろんです。`java.util.Scanner` クラスは、**文字入力を簡単に処理するための便利なクラス**で、標準入力（`System.in`）やファイル、文字列などから、**トークン単位でデータを読み取る**のに使われます。

---

## ✅ Scanner の主な用途

- キーボード入力の読み取り（標準入力）
- ファイルや文字列からのデータ解析
- データを「トークン単位」（空白や改行で区切られた単語など）で取得

---

## ✅ 主なメソッド（カテゴリ別）

### 🔸 1. 入力取得メソッド（型ごと）

| メソッド | 読み取る型 | 説明例 |
| --- | --- | --- |
| `next()` | `String` | 次のトークン（空白まで） |
| `nextLine()` | `String` | 次の1行（改行まで） |
| `nextInt()` | `int` | 次の整数トークン |
| `nextLong()` | `long` | 長整数 |
| `nextDouble()` | `double` | 浮動小数点（倍精度） |
| `nextFloat()` | `float` | 浮動小数点（単精度） |
| `nextBoolean()` | `boolean` | 真偽値（"true" または "false"） |
| `nextByte()` | `byte` | バイト型 |
| `nextShort()` | `short` | ショート型 |
| `nextBigInteger()` | `BigInteger` | 非常に大きな整数（文字列→数） |
| `nextBigDecimal()` | `BigDecimal` | 高精度な小数 |

---

### 🔸 2. トークン存在チェックメソッド

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `hasNext()` | `boolean` | 次のトークンが存在するか |
| `hasNextLine()` | `boolean` | 次の行が存在するか |
| `hasNextInt()` | `boolean` | 次のトークンが整数として読めるか |
| `hasNextDouble()` | `boolean` | 小数として読めるか |
| …（型ごとにあり） |  | 各型に応じた確認 |

---

### 🔸 3. 入力ソース制御・設定系

| メソッド | 説明 |
| --- | --- |
| `useDelimiter(String pattern)` | 区切り文字（デリミタ）の変更（デフォルトは空白） |
| `useLocale(Locale locale)` | 数値のロケール（小数点や桁区切り）指定 |
| `reset()` | スキャナを初期状態にリセット |
| `close()` | スキャナを閉じる（`System.in` 以外で推奨） |

---

### 🔸 4. 情報取得

| メソッド | 戻り値 | 説明 |
| --- | --- | --- |
| `delimiter()` | `Pattern` | 現在の区切り文字 |
| `locale()` | `Locale` | 現在のロケール |
| `radix()` | `int` | 現在の基数（10進/16進など） |

---

## 🔸 使用例（標準入力）

```java
Scanner scanner = new Scanner(System.in);

System.out.print("名前を入力してください: ");
String name = scanner.nextLine();

System.out.print("年齢を入力してください: ");
int age = scanner.nextInt();

System.out.println("こんにちは、" + name + "さん（" + age + "歳）");
scanner.close();
```

---

## ✅ よくある注意点

| 現象 | 原因と対策例 |
| --- | --- |
| `nextInt()` の後に `nextLine()` を呼ぶと空文字になる | `nextInt()` が改行を読み飛ばさないため → `scanner.nextLine()` を1回挟む |
| デリミタの誤解 | デフォルトは「空白全般」 → 必要に応じて `useDelimiter()` を使う |
| ファイルスキャン後のリソース解放 | `Scanner` は `close()` すべき（`System.in` は除く） |

---

## ✅ まとめ

| 項目 | 内容 |
| --- | --- |
| 主な用途 | 標準入力、文字列やファイルからのデータ取得 |
| デフォルト区切り | 空白文字（空白・改行・タブなど） |
| トークン単位 | `next()`, `nextInt()` など |
| 行単位 | `nextLine()` |
| 存在確認 | `hasNextXxx()` 各種 |
| 注意点 | 改行の食い違いやリソース解放忘れに注意 |

---

必要であれば、**ファイルから読み取る Scanner** の実践例や、**正規表現によるトークン分割**の例も紹介できます。ご希望あればどうぞ！