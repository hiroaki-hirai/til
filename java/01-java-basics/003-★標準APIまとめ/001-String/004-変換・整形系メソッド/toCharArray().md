# toCharArray()

# ✅ **String.toCharArray()とは**

**`String.toCharArray()`** は、

「**文字列を1文字ずつ分解して、char型の配列にする**」

ためのメソッドです。

---

# 📚 **基本情報まとめ**

| 項目 | 内容 |
| --- | --- |
| メソッドシグネチャ | `public char[] toCharArray()` |
| 戻り値 | 元の文字列と**同じ文字列を持つchar型配列** |
| 例外 | なし（安全に使える） |
| 文字列との関係 | **コピーされた配列**なので、元のStringは変わらない |

---

# ✍️ **使用例**

```java
String str = "hello";
char[] chars = str.toCharArray();

for (char c : chars) {
    System.out.println(c);
}
```

出力結果：

```java
h
e
l
l
o
```

---

# 📌 **ポイント整理**

| 観点 | 説明 |
| --- | --- |
| **コピーが作成される** | 元の文字列とは**独立したchar配列**が作られる |
| **文字列を直接いじれないJavaの制約回避** | Stringはイミュータブル（不変）なので、配列にしてから**個別に編集可能** |
| **順番は維持される** | 文字列の先頭から順に、配列に格納される |
| **配列の長さ** | 配列の要素数 = 元の文字列の長さ (`str.length()`) |

---

# ⚠️ **試験・実務で注意するべき点**

| 注意点 | 内容 |
| --- | --- |
| **配列を書き換えても元のStringは変わらない** | Stringはイミュータブルだから、配列にしても「元」は影響を受けない |
| **サロゲートペアに注意** | Unicodeの一部文字（絵文字など）は2charで1文字の場合があるので、配列にすると**分かれて格納される**（細かいですが重要） |
| **大量の文字列操作ならStringBuilderの方が効率的** | 配列操作後にまた文字列を組み立てるなら、最初から`StringBuilder`を使う方が良いケースも |

---

# 🌟 **実務的な使い方例**

例えば、

**「文字列を1文字ずつ逆順に並べ替えたい」**

ときに使います。

```java
String str = "abcde";
char[] chars = str.toCharArray();

// 配列を逆順に並び替え
for (int i = 0; i < chars.length / 2; i++) {
    char temp = chars[i];
    chars[i] = chars[chars.length - 1 - i];
    chars[chars.length - 1 - i] = temp;
}

String reversed = new String(chars);
System.out.println(reversed);  // 出力：edcba
```

---

# 🎯 **まとめ**

- `toCharArray()`は「文字列をばらしてchar型配列にする」メソッド
- 元のStringはそのまま、**独立した配列ができる**
- 配列を操作することで、細かい文字単位の制御が可能になる

---

# ✏️ **① toCharArray() を使った小テスト（練習問題集）**

---

## 【問題1】

次のコードの出力結果は？

```java
String str = "ABCD";
char[] chars = str.toCharArray();
System.out.println(chars[1]);
```

【問題1】→ **B（B）**

---

## 【問題2】

次のコードの実行結果を求めてください。

```java
String str = "Java";
char[] chars = str.toCharArray();
chars[0] = 'L';
String newStr = new String(chars);
System.out.println(newStr);
System.out.println(str);
```

【問題2】→ **出力**

```java
Lava
Java
```

---

## 【問題3】

toCharArray()について**正しいもの**を選びなさい。

✅ 選択肢

A. 元のStringの中身を直接書き換えることができる

B. 配列を書き換えても元のStringには影響しない

C. 返される配列はnullになる可能性がある

D. 配列の長さは元のStringよりも常に1多い

【問題3】→ **B**

---

## 【問題4】

次のコードを修正して、「逆順の文字列」を出力するようにしてください。

```java
String str = "dog";
char[] chars = str.toCharArray();

// ここに逆順にする処理を追加

String reversed = new String(chars);
System.out.println(reversed);  // 正解：god
```

【問題4】→ （参考コード）

```java
for (int i = 0; i < chars.length / 2; i++) {
    char temp = chars[i];
    chars[i] = chars[chars.length - 1 - i];
    chars[chars.length - 1 - i] = temp;
}
```

---

# ✨ **② toCharArray() と StringBuilder の使い分け練習**

---

## 【テーマ】

次の2つのケースでは、どちらを使うべきでしょうか？

| ケース | toCharArray()？ | StringBuilder？ | 理由 |
| --- | --- | --- | --- |
| 1. **1文字ずつ読んで加工し、結果を再構築する** | ✅ |  | 配列で1文字ずつ操作するのが効率的 |
| 2. **大量の文字列を連結する（ループで追加）** |  | ✅ | StringBuilderで連結処理が高速 |

---

## 【まとめ図】

| 使う場面 | 使うべきもの |
| --- | --- |
| 文字列を「ばらして1文字ずつ見る」「細かく操作する」 | **toCharArray()** |
| 文字列を「つなげる」「繰り返し加工する」 | **StringBuilder** |

---

# 🎯 【ポイント最終整理】

| 項目 | toCharArray() | StringBuilder |
| --- | --- | --- |
| 主な用途 | 1文字ずつアクセス | 連結・加工 |
| 変換コスト | 軽い（配列） | やや重い（オブジェクト生成） |
| 使う場面 | 配列を直接操作したいとき | 何度も文字列を変化させたいとき |