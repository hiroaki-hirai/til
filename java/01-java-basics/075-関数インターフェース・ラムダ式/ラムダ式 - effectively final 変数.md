# ラムダ式 - effectively final 変数

# Q,

ラムダ式の外で宣言された変数にラムダ式内部からアクセスする場合は、当該変数が変更されると例外が走るという認識はあっていますか？

とても惜しいです！Hiroakiさんの認識には近い部分がありますが、**少し誤解がある**ので、正確にお伝えします。

---

## ✅ 正しい答え

> ラムダ式の内部から参照できるのは、「final」または「effectively final（実質的にfinal）」な変数のみです。
> 
> 
> その変数が **ラムダ式の外で変更されていた場合、コンパイルエラーになります**。
> 
> **例外は発生しません。コンパイル自体が通りません。**
> 

---

## 🔍 詳しく説明

### ● 例：OKなパターン（effectively final）

```java
public class Sample {
    public static void main(String[] args) {
        int x = 10;  // ← 変更していない → effectively final

        Runnable r = () -> {
            System.out.println(x);  // OK
        };

        r.run();
    }
}
```

### ● NGなパターン（xが変更された → effectively finalでなくなる）

```java
public class Sample {
    public static void main(String[] args) {
        int x = 10;

        x = 20;  // ← この変更によって、xは effectively final ではなくなる

        Runnable r = () -> {
            System.out.println(x);  // ❌ コンパイルエラー
        };
    }
}
```

---

## ✅ なぜそうなっているのか？

### ● Javaのラムダ式は「**クロージャ（Closure）**」の一種

ラムダ式内で外の変数を参照する場合、その変数の**スナップショット（読み取り専用のコピー）**を取ってラムダ式に渡します。

> そのため、「変更される可能性がある変数」だと、実行時の値とラムダ式の中で見える値がずれる可能性がある → バグの元
> 
> 
> → Javaは「**変更されない（＝finalもしくはeffectively final）** 変数だけをラムダ式内から参照可能」とルール化している。
> 

---

## ❗️例外が起きるわけではない

- ラムダ式が参照する変数が変更されていたら → **コンパイルエラー**
- Javaのコンパイラが**コンパイル時に制限するので、実行時例外にはならない**

---

## 🧠 用語補足：「effectively final」とは？

- 明示的に `final` と書かれていなくても、**一度だけ代入されて以降、再代入されていない変数**
- Java 8以降で導入された概念

```java
int x = 10;  // effectively final（OK）
int y;
y = 10;     // これも effectively final（OK）

int z = 10;
z = 20;     // NOT effectively final（NG）
```

---

## ✅ まとめ

| 状況 | 結果 | 備考 |
| --- | --- | --- |
| finalまたはeffectively finalな変数を参照 | ✅ OK | 読み取りのみ可 |
| 変更された変数を参照しようとする | ❌ コンパイルエラー | 実行時例外ではない |
| ラムダ内で変数を変更 | ❌ 不可 | 参照のみ可能（基本） |

---

必要であれば、内部クラスとの比較や `AtomicInteger` を使って可変状態をラムダで扱う方法なども紹介できます。興味ありますか？