# static

# Q,

static メソッドのあるクラスにて、そのクラスのフィールドにアクセスしようとする場合、そのフィールドも static である必要がある認識です。
ユーティリティクラスのような static メインのクラスの場合はすべてのメンバが static になるのでしょうか？

はい、Hiroakiさんの理解は **完全に正しいです。**

---

## ✅ 結論：

> staticメソッドからは、staticなフィールドしかアクセスできません。
> 
> 
> よって、**ユーティリティクラスのような構造では、すべてのメンバ（フィールド・メソッド）を static にするのが一般的**です。
> 

---

## 🔍 なぜそうなるのか？

### ◾ staticとは「インスタンスに依存しないクラスレベルのメンバ」

- `static`メソッドは**インスタンスを持たない**状態で使われることを前提としています。
- したがって、その中で **インスタンスフィールド（非static）にアクセスしようとすると、参照先がないためコンパイルエラー**になります。

### ❌ 例（エラーになるケース）

```java
public class MyUtil {
    int count = 0; // 非staticフィールド

    public static void increment() {
        count++; // コンパイルエラー
    }
}
```

---

## ✅ 正しい形（staticメンバに統一）

```java
public class MyUtil {
    private static int count = 0;

    public static void increment() {
        count++;
    }

    public static int getCount() {
        return count;
    }
}
```

- このように `static` メソッドと `static` フィールドを組み合わせれば、
    
    クラス名から直接使える「**状態を持つユーティリティクラス**」になります。
    

---

## ✅ ユーティリティクラスの特徴と慣習

| 特徴 | 説明 |
| --- | --- |
| 全メンバが `static` | メソッドもフィールドも static で完結 |
| コンストラクタは `private` | インスタンス生成を防ぐため |
| `final` + `class` を組み合わせることも | 継承禁止の意図（例：`final class MathUtil`） |
|  |  |

### ◾ 典型例：

```java
public final class MathUtil {
    private MathUtil() {} // インスタンス化禁止

    public static int square(int x) {
        return x * x;
    }

    public static final double PI = 3.1415;
}
```

---

## ✅ 実際のJava標準APIにも存在

| クラス | 用途 |
| --- | --- |
| `java.lang.Math` | 数学系ユーティリティ（すべて static） |
| `java.util.Collections` | コレクション操作ユーティリティ |
| `java.util.Objects` | nullチェックなどの補助ツール |

---

## ✅ 注意点：staticばかり使うのが常に正しいとは限らない

- 状態（＝値）を **インスタンスごとに分けたい** 場合は、`static`は使わない。
- `static`は便利ですが、**オブジェクト指向の拡張性・柔軟性が下がる**ので、ユーティリティ以外では乱用注意。

---

## ✅ まとめ

| ユーティリティクラスでの扱い | 内容 |
| --- | --- |
| static メソッド内で使えるのは static フィールドのみ | ✅ 正しい理解です |
| すべてのメンバを static に統一 | ✅ 一般的な実装方針 |
| コンストラクタを private にしてインスタンス化防止 | ✅ 慣習 |

---

もし「状態をもつユーティリティ」や「staticを避けた設計」についても考えたければ、そこも一緒に掘り下げられますよ！