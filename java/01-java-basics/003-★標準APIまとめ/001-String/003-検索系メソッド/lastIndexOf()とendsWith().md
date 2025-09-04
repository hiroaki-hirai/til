# lastIndexOf()とendsWith()

`lastIndexOf()`と`endsWith()`は**どちらも拡張子チェックで使えます**が、

**「目的」「性質」「使い方」に明確な違い**があります。

順番に、**丁寧に比較しながら解説**していきますね！

---

# ✅ `lastIndexOf()`と`endsWith()`の拡張子処理の違い

| 項目 | lastIndexOf() | endsWith() |
| --- | --- | --- |
| 主な役割 | 位置（インデックス）を探す | 最後が指定文字列と**一致しているか**判定する |
| 戻り値 | `int`（見つかった位置 or -1） | `boolean`（一致していればtrue） |
| 使い方 | 自分で`substring()`などで**切り出す**必要がある | 判定だけでOK（**切り出し不要**） |
| 処理対象 | 文字・部分文字列（何文字でも探せる） | 部分文字列（必ず**末尾**を対象に完全一致を確認） |
| 目的に向いている場面 | ファイル名や拡張子を**取り出す**とき | ファイルが特定の拡張子かどうか**チェック**したいとき |
| 見つからない場合 | `-1`を返す（要チェック） | `false`を返す（そのまま使える） |
| null渡し | null渡すと例外（NullPointerException） | null渡すと例外（NullPointerException） |

---

# 🔹 【使い方イメージ比較】

---

## ◆ lastIndexOf()の使い方（拡張子を「取り出す」）

```java
String path = "photo.jpg";

// 1. 最後のピリオドの位置を探す
int pos = path.lastIndexOf('.');

// 2. そこから拡張子を切り出す
String ext = (pos != -1) ? path.substring(pos + 1) : "";

System.out.println(ext); // 出力：jpg
```

✅ **lastIndexOf()は「ピリオドの位置」だけ教えてくれる**ので、

切り出し（substring）を**自分で**やる必要がある！

---

## ◆ endsWith()の使い方（拡張子を「チェックする」）

```java
String path = "photo.jpg";

if (path.endsWith(".jpg")) {
    System.out.println("これはJPGファイルです");
}
```

✅ **endsWith()は「この文字列で終わってる？」だけを判定**するので、

**切り出し不要でそのまま使える**！

---

# 🔸 【まとめたイメージ図】

```java
lastIndexOf() → 「位置」取得 → 自分で切り出し！
endsWith()   → 「末尾一致？」を直接判定！
```

---

# 🔹 【どう使い分ける？】

| やりたいこと | 使うべきメソッド |
| --- | --- |
| ファイルの拡張子（文字列）を取り出したい | lastIndexOf() ＋ substring() |
| ファイルが特定の拡張子かチェックしたい | endsWith() |

---

# ✅ まとめ

| 比較項目 | lastIndexOf() | endsWith() |
| --- | --- | --- |
| 戻り値 | インデックス番号（int型） | 判定結果（boolean型） |
| 処理内容 | 「位置」を探してから自分で切り出す必要あり | 「末尾一致」を即判定できる |
| 向いている場面 | ファイル名・拡張子を**取り出す** | ファイルが特定拡張子かどうか**チェックする** |
| null渡しの影響 | NullPointerExceptionが発生する | 同じくNullPointerExceptionが発生する |

---

# ✨ 超シンプルまとめ

> lastIndexOf()は「位置を探して切り出す」、
endsWith()は「末尾一致を即チェック」！
> 

✅ この感覚さえ押さえれば、両者を正しく使い分けられます！