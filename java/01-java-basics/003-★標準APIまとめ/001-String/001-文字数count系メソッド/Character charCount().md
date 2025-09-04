# Character.charCount()

`Character.charCount(int codePoint)` は、

**指定したUnicodeコードポイント（int型）が、UTF-16で何個のchar（16bit単位）を必要とするか**を教えてくれるメソッドです。

つまり、

**通常の文字なら1個、サロゲートペアが必要な文字（絵文字や拡張漢字など）なら2個**

という違いを**判定するためのメソッド**です！

---

# ✅ `Character.charCount()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.Character`（インポート不要） |
| メソッド名 | `charCount(int codePoint)` |
| 戻り値の型 | `int`（1 または 2） |
| 主な用途 | コードポイントが必要とするUTF-16単位の数を取得 |

---

# 🔹 シグネチャ（定義）

```java
public static int charCount(int codePoint)
```

- `codePoint`：対象のUnicodeコードポイント（int型）
- 戻り値：
    - **1** → 通常のchar1個で表現できる文字（BMP内、U+0000～U+FFFF）
    - **2** → サロゲートペアが必要な文字（BMP外、U+10000以上）

---

# 🔹 使用例（基本）

```java
System.out.println(Character.charCount('A'));        // → 1
System.out.println(Character.charCount(0x1F600));    // → 2 （😀）
System.out.println(Character.charCount(0x29E3D));    // → 2 （𩸽）
```

✅ `A` など普通の英字は1個のcharでOK

✅ 絵文字などはサロゲートペア（2char）必要！

---

# 🔹 何に使うのか？

- **文字列を1文字ずつ正しく進めたい時**
- **サロゲートペア対応のカーソル移動、文字列処理**をするとき
- **正確な「人間が見る文字数」をカウントしたい時**

👉 たとえば、`codePointAt()` でコードポイントを取得した後に、

何char進めるべきかを知るために使います！

---

# 🔸 実際の使用例（1文字ずつ安全に進める）

```java
String s = "A𩸽B"; // 'A' + '𩸽'(サロゲートペア) + 'B'
for (int i = 0; i < s.length(); ) {
    int codePoint = s.codePointAt(i);
    System.out.println(Character.toChars(codePoint)); // 1文字表示
    i += Character.charCount(codePoint); // 正しく1文字分進める
}
```

出力：

```java
A
𩸽
B
```

✅ サロゲートペアも**1回でまとめて進める**ことができます！

---

# 🔹 `Character.charCount()` まとめイメージ

| 入力コードポイント | 戻り値 | 備考 |
| --- | --- | --- |
| `0x0041`（'A'） | 1 | 通常の英字 |
| `0x3042`（'あ'） | 1 | ひらがななども1 |
| `0x1F600`（'😀'） | 2 | 絵文字（サロゲートペアが必要） |
| `0x29E3D`（'𩸽'） | 2 | 拡張漢字（サロゲートペアが必要） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 戻り値 | `1`（通常文字）か `2`（サロゲートペア必要） |
| サロゲートペア対応 | ✅ きちんと2char必要な場合に2を返す |
| 主な用途 | 文字列を「人間が見る1文字ずつ」進めるために使う |

---

# ✨ 超シンプルまとめ

> charCount() は、そのコードポイントが必要とする UTF-16要素数を返す！
> 

✅ こう覚えれば完璧です！

---

# 🔺 `Character.charCount()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：通常文字の場合

```java
int result = Character.charCount('B');
System.out.println(result);
```

この出力は？

A. `0`

B. `1`

C. `2`

D. コンパイルエラー

**答え**：B. `1`

**解説**：

- `'B'`（U+0042）は通常のUnicode基本多言語面（BMP）内の文字。
- ✅ 1charで表現できるので `charCount()` は **1** を返します！

---

### ❓ 問題 2：絵文字の場合

```java
int emojiCodePoint = 0x1F600; // 😀
System.out.println(Character.charCount(emojiCodePoint));
```

この出力は？

A. `1`

B. `2`

C. `0`

D. コンパイルエラー

**答え**：B. `2`

**解説**：

- U+1F600（😀）はBMP外のコードポイント。
- ✅ サロゲートペアが必要なため、**2** を返します！

---

### ❓ 問題 3：charCount()の戻り値型

次のうち、`Character.charCount(codePoint)` の戻り値型はどれ？

A. `Integer`

B. `int`

C. `char`

D. `short`

**答え**：B. `int`

**解説**：

- `charCount()` はプリミティブ型の `int` を返します。
- ただし返る値は **常に 1 または 2**！

---

### ❓ 問題 4：存在しないコードポイント（範囲外）

```java
int result = Character.charCount(0x110000); // U+110000
System.out.println(result);
```

この出力は？

A. `1`

B. `2`

C. `0`

D. 実行時例外

**答え**：C. `0`

**解説**：

- Unicodeの有効範囲は U+0000 ～ U+10FFFF。
- `0x110000` は範囲外なので、**charCountは0を返す**！
- ✅ **例外は出ず、0が返る**のがポイント！

---

### ❓ 問題 5：サロゲートペアとcharAtの連携

次のコードで、何が出力される？

```java
String s = "A𩸽B"; // 'A' + '𩸽'(サロゲートペア) + 'B'
int codePoint = s.codePointAt(1);
System.out.println(Character.charCount(codePoint));
```

A. `1`

B. `2`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `2`

**解説**：

- `s.codePointAt(1)` はサロゲートペアの上位サロゲート位置だが、ちゃんと「𩸽」全体のコードポイントを返す。
- そのコードポイントに対する `charCount()` は **2**！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| `charCount()` はcharを返す？ | ❌ int型（プリミティブ）を返す |
| BMP外でも1文字扱い？ | ❌ サロゲートペアで2char必要（charCountは2） |
| 無効なコードポイントは例外が発生する？ | ❌ 例外は出ず、0を返す |
| サロゲートペアの中間インデックス指定でも正しくカウントできる？ | ❌ 正しいコードポイント位置に注意！ |

現場では、主に次のような**「Unicode（見た目の1文字単位）を正しく扱う必要がある場面」**で使います！

---

# ✅ 実務での主な利用シーン

| 利用シーン | 説明 |
| --- | --- |
| 1. **バリデーション（検証）** | 入力文字数チェック（絵文字など含めた「人間が見る文字数」基準でチェック） |
| 2. **文字列の分割/切り詰め** | 見た目の1文字単位できれいに区切る（例：ツイート140文字制限対応、プロフィール文字数制限など） |
| 3. **禁則文字チェック** | 特定のUnicode範囲（例：絵文字禁止、サロゲートペア禁止など）の検出 |
| 4. **フォントレンダリング** | サロゲートペアを1つの文字として正しく描画（ゲーム・スマホアプリのテキスト描画など） |
| 5. **カスタム文字列エディタ** | 1文字ずつカーソル移動、文字選択（IME入力、右→左言語対応など） |
| 6. **文字列正規化・変換** | Unicode正規化（NFC/NFD変換）や特殊記号を通常文字に変換する際に正確に文字単位で処理する |

---

# 🔵 具体例でもう少しイメージ！

### 例①：ユーザー名入力バリデーション（絵文字も1文字扱い）

```java
public boolean isValidUserName(String name) {
    int charCount = 0;
    for (int i = 0; i < name.length(); ) {
        int codePoint = name.codePointAt(i);
        charCount++;
        i += Character.charCount(codePoint);
    }
    return charCount <= 20; // 「20文字以内」で許可
}
```

✅ **"A𩸽B"** は見た目3文字なのでOK、

✅ **char配列の長さだけ見てたらズレる問題を防げる！**

---

### 例②：特定の絵文字だけ除外するフィルター

```java
public String removeEmojis(String input) {
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < input.length(); ) {
        int codePoint = input.codePointAt(i);
        if (Character.isSupplementaryCodePoint(codePoint)) {
            // サロゲートペア（絵文字など）はスキップ
        } else {
            sb.appendCodePoint(codePoint);
        }
        i += Character.charCount(codePoint);
    }
    return sb.toString();
}
```

---

# 🔥 まとめ

| ポイント | 説明 |
| --- | --- |
| 目に見える「1文字」単位で正確に処理したい | サロゲートペア（絵文字など）も含めて正しく扱う必要がある |
| char配列の長さ(length)だけでは不十分 | 実際の「文字数」ではないため |
| 文字単位での厳密処理が必要な場面がある | バリデーション、描画、分割、フィルターなど |