# super 一足飛びNG

# Q,

一足飛びNGな構文（直接の親のみ可）なものは、アクセス修飾子を除くと、super 関連に限られる認識で問題ないでしょうか？

はい、**その認識でほぼ問題ありません。**

---

## ✅ 結論：

> アクセス修飾子の影響を除外した場合、Javaにおいて「一足飛びNG（直接の親限定）」な構文は、ほぼ super に関する構文に限られます。
> 

---

## 🔹 該当する `super` 関連構文一覧（= 一足飛びNG）

| 構文 | 直接の親が必要 | 一足飛び可否 | 説明 |
| --- | --- | --- | --- |
| `super.method()` | ✅ 必須 | ❌ | クラス階層で1段上の親のメソッドのみ呼び出せる |
| `super()`（コンストラクタ呼び出し） | ✅ 必須 | ❌ | 親クラスのコンストラクタのみ呼び出せる |
| `InterfaceName.super.method()` | ✅ 必須 | ❌ | 直接の super interface に限定（JLSによる） |

---

## ✅ 一方、それ以外の構文では「一足飛びOK」

以下のような構文では、直接の親かどうかを問わず、階層の深さに関係なくアクセスできます（アクセス修飾子が許す限り）：

| 項目 | 一足飛びOK | 備考 |
| --- | --- | --- |
| フィールドアクセス | ✅ | シャドーイングされていなければ、親の親でも参照される |
| メソッド呼び出し（通常の） | ✅ | 動的バインディングにより最終オーバーライド先が呼ばれる |
| キャストによるフィールド／メソッドアクセス | ✅ | ((A)this).field のようなキャストで回避可能 |
| static メソッドの参照 | ✅ | 階層にかかわらずアクセス可能（推奨されない使い方もあるが） |

### **✅ 整理された理解のための図式**

```java
   A
   ↑
   B
   ↑
   C

super() / super.method() / A.super.method()
→ C → B のみ可、A は ❌（一足飛び NG）

通常のメソッド / フィールド参照（宣言型・実行型による）
→ A, B, C いずれも OK（アクセス修飾子が許せば）

キャストによる参照
→ ((A)this).field / method() で到達可能
```

---

## ✅ 例外を考慮しないと明言している場合（アクセス修飾子なし）、

**`super` の使用に関連する構文以外には、一足飛びNGな制約は基本的に存在しません。**

---

## 🔚 結論（再掲）

> アクセス修飾子の影響を除いた場合、「一足飛びNG」な構文は super 系に限定されるという理解は 正しいです。
> 

より細かく、構文レベルや JLS 条文レベルで確認したい場合もご対応できます。お気軽にどうぞ。

# Q,

上記のコードをeclipseで実行すると以下のようなエラーがでました。

```java
Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
	Illegal reference to super type A, cannot bypass the more specific direct super type test06.B

	at test06.C.sample(C.java:7)
	at test06.Main.main(Main.java:8)
```

ありがとうございます。Hiroakiさんが指摘された **Eclipseでの実行エラー**について、非常に本質的なポイントを突いています。

---

## 🔴 エラー内容の要点

```java
Illegal reference to super type A, cannot bypass the more specific direct super type test06.B
```

---

## ✅ これは何が問題か？

これは Eclipse のコンパイラ（ECJ: Eclipse Compiler for Java）が、

`A.super.sample()` の呼び出しに対して **Java言語仕様（JLS）に従って**

**「より具体的な直接のスーパタイプがあるため、それを飛び越えて `A.super` にアクセスするのは不正」**と判断しているためです。

---

## 🔍 状況の整理

```java
interface A {
    default void sample() {
        System.out.println("A");
    }
}

interface B extends A {
    default void sample() {
        System.out.println("B");
    }
}

class C implements B {
    @Override
    public void sample() {
        A.super.sample(); // ← ★ここが問題として Eclipse で指摘される
    }
}
```

この構成で `A.super.sample();` を書くと：

- `C` は `B` を実装している
- `B` は `sample()` を default メソッドとして **オーバーライド** している
- `C` は `A` よりも **B のほうが直接のスーパタイプ**

➡ よって、**「A.super」として `B` を飛び越えることは許可されない**

---

## 🔸 これは Eclipse 固有の仕様か？

いいえ。

**これは Java Language Specification（JLS）に準拠した正しい制限です。**

---

## ✅ JLS におけるルール（要点）

JLS §15.12.1（Method Invocation）および §15.12.4.1 にて、

**インターフェースの `super` 呼び出しは「直接のスーパタイプ」でなければならない**

というルールが明示されています。

➡ 「間接的に継承している default メソッドの呼び出し」はできない、というのが正式な仕様です。

---

## 🔁 対処法（代替策）

```java
interface A {
    default void sample() {
        System.out.println("A");
    }
}

interface B extends A {
    default void sample() {
        A.super.sample(); // ★ ここで A の default を呼び出す
    }
}

class C implements B {
    @Override
    public void sample() {
        B.super.sample(); // ✅ OK：C は B を直接実装しているため
    }
}
```

✅ `C` では `B.super.sample()` と書き、

`B` の中で `A.super.sample()` を使えば目的の動作が得られます。

---

## 🔚 結論

| 観点 | 内容 |
| --- | --- |
| ❌ NG構文 | `A.super.sample()` は、`B` が `A` の default をオーバーライドしていると **`C` では使えない** |
| ✅ 正しい方法 | `B` の中で `A.super.sample()` を使い、`C` では `B.super.sample()` を呼び出す |
| 背景 | Java言語仕様（JLS）で、**super呼び出しは最も直接のスーパタイプに限定される** と定められている |

必要であれば、この構成に関するバイトコード確認や、他のバリエーションパターンもご紹介できます。お気軽にどうぞ。