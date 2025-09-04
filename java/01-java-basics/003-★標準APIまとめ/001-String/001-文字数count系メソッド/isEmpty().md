# isEmpty()

`isEmpty()`  は、**「文字列が空かどうか？」** をチェックするためのメソッドです。

---

# ✅ `String.isEmpty()` の基本情報

| 項目 | 内容 |
| --- | --- |
| 所属クラス | `java.lang.String`（インポート不要） |
| メソッド名 | `isEmpty()` |
| 戻り値の型 | `boolean` |
| 主な用途 | 文字列が**「完全に何もないか」**を判定する |
| 追加バージョン | Java 6 以降 |

---

# 🔹 シグネチャ（定義）

```java
public boolean isEmpty()
```

- 引数なし
- 戻り値は `true` または `false`
    - `true` → **文字列の長さが0**（空文字）
    - `false` → **1文字以上ある**

---

# 🔹 動作イメージ

```java
String s1 = "";
System.out.println(s1.isEmpty()); // → true

String s2 = " ";
System.out.println(s2.isEmpty()); // → false（スペースは文字1個だから）

String s3 = "abc";
System.out.println(s3.isEmpty()); // → false
```

✅ ポイント：

**たとえ空白だけでも「文字があるならfalse」**になる！

---

# 🔹 内部的な動き

`isEmpty()` の内部は、実はこれとほぼ同じです：

```java
return this.length() == 0;
```

つまり、

**長さ（文字数）が0かどうかだけを見ている**非常に軽い処理です。

---

# 🔹 よくある使い方

### 入力バリデーション（空文字だけ除外）

```java
if (input != null && !input.isEmpty()) {
    System.out.println("入力あり");
} else {
    System.out.println("入力なし");
}
```

✅ nullチェックと組み合わせるのが実務では鉄則です！

---

# 🔸 注意ポイント

| 誤解しやすい点 | 正しい理解 |
| --- | --- |
| スペースだけでもtrueになる？ | ❌ スペース `" "` は長さ1 → `isEmpty()`はfalse |
| nullに対して呼び出すとどうなる？ | ❌ `NullPointerException` が発生（必ずnullチェックを先） |

---

# ✅ まとめ

| 特徴 | 内容 |
| --- | --- |
| 判定基準 | **文字列の長さが0ならtrue** |
| 空白が含まれていたら？ | 文字が存在するので **false** |
| null安全性 | ❌ nullの場合は例外発生、先にnullチェックが必要 |
| 実装内部 | `length() == 0` で判定している |

---

# ✨ 超シンプルまとめ

> isEmpty() は「長さ0か？」だけを素早く判定する軽量メソッド！
> 

✅ これだけ覚えればOKです！

---

# 🔺 `String.isEmpty()` 引っかけ問題集（全5問）

---

### ❓ 問題 1：通常の空文字列

```java
String s = "";
System.out.println(s.isEmpty());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- 文字列の長さが0なので、**isEmpty()はtrue**。

---

### ❓ 問題 2：スペース1個の場合

```java
String s = " ";
System.out.println(s.isEmpty());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- スペースも**文字1個**としてカウントされる！
- 長さ1なので、**isEmpty()はfalse**。

---

### ❓ 問題 3：nullに対して呼び出す

```java
String s = null;
System.out.println(s.isEmpty());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：C. 実行時例外

**解説**：

- null参照に対してメソッドを呼び出すと、**NullPointerException** が発生！

✅ **nullチェックは必須**！

---

### ❓ 問題 4：改行文字だけの場合

```java
String s = "\n";
System.out.println(s.isEmpty());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：B. `false`

**解説**：

- 改行 `"\n"` も**文字1個**とカウントされる！
- ✅ 長さ1なので、**isEmpty()はfalse**。

---

### ❓ 問題 5：length()との関係

```java
String s = "";
System.out.println(s.length() == 0 && s.isEmpty());
```

この出力は？

A. `true`

B. `false`

C. 実行時例外

D. コンパイルエラー

**答え**：A. `true`

**解説**：

- `length()==0`と`isEmpty()`は完全に同じ意味。
- 両方trueなので、式全体も**true**になる！

---

# ✅ よくある誤解まとめ

| 誤解内容 | 正しい理解 |
| --- | --- |
| 空白だけならisEmpty()はtrue？ | ❌ 空白 `" "` も長さ1なので **false** |
| nullならisEmpty()はtrueになる？ | ❌ NullPointerExceptionになる（nullチェック必須） |
| 改行だけならisEmpty()はtrue？ | ❌ 改行も1文字扱い（false） |
| length()==0とisEmpty()は違う？ | ❌ 基本的に同じ（isEmptyは length()==0 をラップ） |

---

# ✨ 超まとめ

> isEmpty()は「長さ0のみtrue」、空白・改行も文字としてカウントされる！
nullなら例外になるので要注意！
> 

✅ これを押さえておけば間違えません！