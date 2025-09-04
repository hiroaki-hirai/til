# toLowerCase()/toUpperCase() vs equalsIgnoreCase()

# ✅ `toLowerCase()/toUpperCase()` vs `equalsIgnoreCase()` 比較総まとめ

---

## 🔹 基本的な違い

| 項目 | toLowerCase()/toUpperCase()＋equals() | equalsIgnoreCase() |
| --- | --- | --- |
| 目的 | 自分で大文字・小文字を統一してから比較する | メソッド1つで大文字小文字無視して比較できる |
| コードの量 | 少し長くなる | 短く簡潔に書ける |
| 柔軟性（途中加工できるか） | 加工できる（比較前に文字列を整形できる） | 加工できない（そのまま比較） |
| null安全性 | ❌ nullチェックを自分でやる必要あり | ❌ nullを渡すとNullPointerExceptionになる |
| 実務での使用頻度 | フィルタリング・部分一致用途でよく使う | 完全一致チェックに便利 |

---

## 🔹 使用例（比較）

---

### ◆ toLowerCase()＋equals()版

```java
String a = "OpenAI";
String b = "openai";

if (a.toLowerCase().equals(b.toLowerCase())) {
    System.out.println("一致しました");
}
```

✅ **柔軟に文字列加工できる（例：trim(), replace()などを挟める）**

---

### ◆ equalsIgnoreCase()版

```java
String a = "OpenAI";
String b = "openai";

if (a.equalsIgnoreCase(b)) {
    System.out.println("一致しました");
}
```

✅ **単純な大小無視比較ならequalsIgnoreCase()だけで十分！**

---

## 🔹 さらに深堀り：コードの流れ比較

| 流れ | toLowerCase()版 | equalsIgnoreCase()版 |
| --- | --- | --- |
| 1. 大文字小文字を揃える | toLowerCase()/toUpperCase()を呼び出す | 内部的にequalsIgnoreCaseがやってくれる |
| 2. 比較する | equals()で比較 | そのままequalsIgnoreCase()を呼ぶ |

---

## 🔹 それぞれの「得意パターン」

| 得意な場面 | 使うメソッド |
| --- | --- |
| 完全一致をシンプルに判定したい | ✅ `equalsIgnoreCase()` |
| 比較前に文字列を加工（trim, replaceなど）したい | ✅ `toLowerCase()/toUpperCase()`＋equals() |
| 部分一致でフィルタリングしたい | ✅ `toLowerCase()/toUpperCase()`＋contains()など |

---

## 🔹 注意点まとめ

| 注意点 | 内容 |
| --- | --- |
| null対象を比較すると？ | どちらもNullPointerExceptionになる（要nullチェック） |
| 数字・記号・漢字などの影響は？ | 英字以外は変換しないため安心（大小問題なし） |
| 部分一致用途では？ | equalsIgnoreCase()は**完全一致専用**、部分一致には使えない |

---

# ✅ 最後に使い分け指針まとめ

| 使い方シチュエーション | 推奨メソッド |
| --- | --- |
| 「単純な大小無視の完全一致を判定したい」 | ➔ `equalsIgnoreCase()`を使う！ |
| 「大小無視で部分一致検索したい」 | ➔ `toLowerCase()`や`toUpperCase()`＋contains()やstartsWith()を使う！ |
| 「比較前に空白削除・正規化もしたい」 | ➔ `toLowerCase()`や`toUpperCase()`＋整形＋equals()を使う！ |

---

# ✨ 超シンプルまとめ

> 完全一致だけならequalsIgnoreCase()でOK！
部分一致や加工込みならtoLowerCase()/toUpperCase()＋equals()/contains()！
nullチェックはどちらも必要！
> 

✅ この感覚を押さえれば、試験も実務もバッチリ対応できます！