# java.lang.Class

# Q,

java.lang.Classクラスについて解説してください。

はい、`java.lang.Class` クラスは Java の実行時型情報（Runtime Type Information, RTI）を扱う中心的なクラスで、**JVM内部にあるクラスのメタデータへの“窓口”**の役割を持ちます。

Javaプログラムから型情報を取得したり、動的にクラスを操作したりする場合は、必ずこの `Class<T>` オブジェクトを経由します。

---

## 1. 基本概要

- **パッケージ**：`java.lang`
- **宣言**：

```java
public final class Class<T> implements java.io.Serializable, java.lang.reflect.GenericDeclaration, java.lang.reflect.Type, java.lang.reflect.AnnotatedElement
```

- **型パラメータ**：`<T>` は、その `Class` オブジェクトが表す型
    
    例：`Class<String>` は `String` 型を表す
    

---

## 2. 生成方法（直接 new は不可）

`Class` オブジェクトは自分で `new` できません。JVMがクラスをロードした時に自動生成し、同じクラスに対しては常に同じ `Class` オブジェクトが返されます。

取得方法は主に以下の3つです。

| 方法 | 例 | 説明 |
| --- | --- | --- |
| **クラスリテラル** | `String.class` | コンパイル時に型が決まる |
| **インスタンスから** | `obj.getClass()` | 実行時に取得 |
| **文字列から動的ロード** | `Class.forName("java.lang.String")` | 実行時にクラス名でロード |

---

## 3. 主な用途

### (1) メタ情報の取得

`Class` オブジェクトは、その型に関するあらゆる情報を取得できます。

```java
Class<?> clazz = String.class;
System.out.println(clazz.getName());                // 完全修飾名
System.out.println(clazz.getPackageName());         // パッケージ名
System.out.println(clazz.getSuperclass());          // 親クラス
System.out.println(Arrays.toString(clazz.getInterfaces())); // 実装インタフェース
```

### (2) リフレクションによる操作

`Class` オブジェクト経由でコンストラクタ、フィールド、メソッドを取得し、動的に操作できます。

```java
Object s = String.class.getConstructor(String.class).newInstance("Hello");
Method m = String.class.getMethod("toUpperCase");
System.out.println(m.invoke(s)); // HELLO
```

### (3) 型判定・キャスト

```java
if (Number.class.isAssignableFrom(Integer.class)) {
    System.out.println("IntegerはNumberのサブクラス");
}
Number n = Number.class.cast(10); // 安全なキャスト
```

### (4) リソース取得

```java
InputStream in = MyClass.class.getResourceAsStream("config.properties");
```

### (5) 配列や型情報の操作

```java
Class<?> arrType = int[].class.getComponentType(); // → int.class
```

---

## 4. JVM内部での位置づけ

- **Javaヒープ上**に `Class<T>` オブジェクトがあり、そこから **JVMメタスペース**にあるクラス定義情報への参照を持っています。
- `Class` は **final** なので継承できません。
- 同一クラスに対する `Class` オブジェクトはクラスローダ単位で唯一です（`==` で比較可）。

```java
[Javaヒープ]                 [Metaspace(ネイティブ領域)]
java.lang.Class<T> ─────→  クラスのメタデータ（フィールド情報、メソッド情報、定数プール…）
```

---

## 5. 代表的なメソッド一覧（カテゴリ別）

| カテゴリ | メソッド例 | 説明 |
| --- | --- | --- |
| 名前・型情報 | `getName()`, `getSimpleName()`, `getTypeName()` | クラス名取得 |
| 継承・実装 | `getSuperclass()`, `getInterfaces()`, `isInterface()` | 型関係 |
| コンストラクタ | `getConstructor(...)`, `getDeclaredConstructors()` | コンストラクタ取得 |
| メソッド | `getMethod(...)`, `getDeclaredMethods()` | メソッド取得 |
| フィールド | `getField(...)`, `getDeclaredFields()` | フィールド取得 |
| アノテーション | `getAnnotation(...)`, `getAnnotations()` | アノテーション取得 |
| 型判定 | `isInstance(obj)`, `isAssignableFrom(clazz)` | 型チェック |
| 配列関連 | `isArray()`, `getComponentType()` | 配列型情報 |
| 修飾子 | `getModifiers()` | public, private, abstract 等 |
| クラスローダ | `getClassLoader()` | クラスロード元の取得 |

---

## 6. 特徴まとめ

- `Class` オブジェクトは **1クラスにつき1つ（クラスローダ単位）** しか存在しない
- JVMがクラスロード時に自動生成
- リフレクションの入口だが、それ以外にも型判定やリソース取得など幅広く利用
- `Class<T>` の `<T>` は表す型を意味し、型安全に扱える

---

もし希望あれば、この解説を**「JVMクラスローディングの流れ＋Classオブジェクト生成ポイント」付きのシーケンス図**にして見せられます。

そうすると「いつClassが作られて、どのメモリに置かれるのか」が一目で分かります。

[Class オブジェクトを利用したインスタンス化](java%20lang%20Class%2024b62cef735080769cf9d311d75505bf/Class%20%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%82%92%E5%88%A9%E7%94%A8%E3%81%97%E3%81%9F%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E5%8C%96%2024b62cef7350801bad26ee6e61cb63ac.md)

[構造図](java%20lang%20Class%2024b62cef735080769cf9d311d75505bf/%E6%A7%8B%E9%80%A0%E5%9B%B3%2024c62cef735080cf857fc3f3dec9471b.md)

[導入](java%20lang%20Class%2024b62cef735080769cf9d311d75505bf/%E5%B0%8E%E5%85%A5%2024c62cef73508038bc69e3655b606397.md)

[Classｵﾌﾞｼﾞｪｸﾄを得るリテラル表記](java%20lang%20Class%2024b62cef735080769cf9d311d75505bf/Class%EF%BD%B5%EF%BE%8C%EF%BE%9E%EF%BD%BC%EF%BE%9E%EF%BD%AA%EF%BD%B8%EF%BE%84%E3%82%92%E5%BE%97%E3%82%8B%E3%83%AA%E3%83%86%E3%83%A9%E3%83%AB%E8%A1%A8%E8%A8%98%2024c62cef735080ae86a0c17278af053f.md)