# Classｵﾌﾞｼﾞｪｸﾄを得るリテラル表記

# Q,

この「型から Class オブジェクトを得るリテラル表記」は、どのメソッドでもその引数に代入できるということでしょうか？
それともメソッドの一部ですか？

これは**メソッドの一部ではなく、Javaの文法として存在する「クラスリテラル表記」です。
なので、特定のメソッド専用ではなく、`Class<T>` 型を受け取れる場所ならどこでも引数に渡せます**。

---

## 1. 文法的な位置づけ

- 構文：

```java
<型名>.class
```

- これは Java 言語仕様（JLS）にある**式**の一種で、実行時にその型の `Class<T>` インスタンスを返します。
- メソッドの一部ではなく、**`int` や `"文字列"` と同じ「リテラル」扱い**です。

---

## 2. 使える場所

### 例：メソッド引数に渡す

```java
public static void printClassInfo(Class<?> clazz) {
    System.out.println(clazz.getName());
}

printClassInfo(String.class);     // OK
printClassInfo(int[].class);      // OK
printClassInfo(void.class);       // OK
```

### 例：変数に代入

```java
Class<MyService> serviceType = MyService.class;
```

### 例：アノテーションの属性値（`Class<?>`型）

```java
@interface MyAnno {
    Class<?> value();
}

@MyAnno(MyService.class)
public class Test {}
```

### 例：マップのキーに

```java
Map<Class<?>, Object> registry = new HashMap<>();
registry.put(MyService.class, new MyServiceImpl());
```

---

## 3. `ServiceLoader.load()` の場合

`ServiceLoader.load(MyService.class)` は、たまたま引数の型が `Class<S>` だから、そこにこの「クラスリテラル表記」を渡しているだけです。

**他にも同じように `Class<T>` を引数に取るメソッドなら全部使えます。**

---

## 4. まとめ

- **`型名.class` は文法上のリテラル表記**で、メソッド固有の書き方ではない
- 返り値は `java.lang.Class<T>` オブジェクト
- `Class<T>` 型を受け付けるなら**どのメソッド引数にも代入可能**

---

もし希望があれば、これが**バイトコードではどう表現されるか（`ldc`命令でClass定数をロード）**まで説明できます。

そうすると、「なぜ実行時コストが小さいか」までクリアにできます。

# Q,

```java
ServiceLoader.load(MyService.class) 
```

上記のMyService.class のようなサービス型のClass を表す .classの表記の仕方はここのみでしょうか？

いい質問です。

`MyService.class` という **「型から `Class` オブジェクトを得るリテラル表記」** は、`ServiceLoader.load()` に限らず **Java全般で使える文法** です。

---

## 1. `.class` リテラル表記とは

- 構文：

```java
<型名>.class
```

- 意味：
    - その型に対応する **`java.lang.Class<T>` オブジェクト** をコンパイル時に取得
    - 型はクラス、インタフェース、配列型、プリミティブ型、void もOK

例：

```java
Class<String> strClass = String.class;     // クラス型
Class<Runnable> runClass = Runnable.class; // インタフェース型
Class<int[]> arrClass = int[].class;       // 配列型
Class<Integer> intClass = int.class;       // プリミティブ型
Class<Void> voidClass = void.class;        // void 型
```

---

## 2. `.class` がよく使われる場面

`ServiceLoader.load()` 以外にも、かなり多くのAPIで使います。

| 用途 | 例 |
| --- | --- |
| **リフレクション** | `Class<?> clazz = MyService.class;` / `clazz.getDeclaredMethods()` |
| **クラスリテラルをキーにする** | `Map<Class<?>, Object> registry = new HashMap<>(); registry.put(MyService.class, new MyServiceImpl());` |
| **Class.forName() の型安全版** | `MyService.class.getName()` で完全修飾名取得 |
| **依存注入（DI）設定** | `binder.bind(MyService.class).to(MyServiceImpl.class);` |
| **アノテーションの属性値** | `@SuppressWarnings("unchecked")` のように使うアノテーションの `Class<?>` 型引数に指定（例：`@MyAnno(MyService.class)`） |
| **ジェネリクスで型情報を渡す** | `public <T> T createInstance(Class<T> type) { return type.getDeclaredConstructor().newInstance(); }` |

---

## 3. `.class` の特徴

- コンパイル時にクラスリテラルとして埋め込まれるので、`Class.forName("...")` のように**文字列→動的ロード**ではなく、**型安全**で**早期に存在チェック**されます。
- **型パラメータ付きで使える**（例：`Class<MyService>`）
- 配列やプリミティブ、`void` にも使えるのが文字列指定との大きな違い

---

## 4. まとめ

- **`MyService.class` は ServiceLoader 専用ではない**
    
    → Java の基本文法「クラスリテラル」で、いろいろな場面で利用可能
    
- メリットは **型安全・IDE補完あり・コンパイル時チェック** が効くこと
- 文字列指定の `Class.forName()` より安全で保守性が高い

---

もしご希望なら、この `.class` の**コンパイル後バイトコード上での動き（`ldc`命令でClassオブジェクトをロード）**を説明できます。

そうすると「なぜ実行時コストが小さいか」までわかります。