# プロセッサ (APT)

# Q,

APTやリフレクションで実際に値を読み取って分岐処理するミニサンプルをお願いします

了解しました！

以下に、**APT（アノテーションプロセッサ）とリフレクション**のそれぞれで「アノテーションの値を読み取り、処理を分岐する」ミニサンプルを用意しました。

---

## ✅ 1. リフレクションでアノテーションの値を読み取る（実行時）

### ◉ カスタムアノテーション定義

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Role {
    String value();  // "ADMIN" など
}
```

---

### ◉ アノテーション付きクラス

```java
public class UserService {

    @Role("ADMIN")
    public void deleteUser() {
        System.out.println("Deleting user...");
    }

    @Role("USER")
    public void viewUser() {
        System.out.println("Viewing user...");
    }
}
```

---

### ◉ リフレクションで値を読み取り処理分岐

```java
import java.lang.reflect.Method;

public class AnnotationProcessor {

    public static void main(String[] args) throws Exception {
        UserService service = new UserService();

        for (Method method : UserService.class.getDeclaredMethods()) {
            if (method.isAnnotationPresent(Role.class)) {
                Role role = method.getAnnotation(Role.class);
                String roleValue = role.value();

                if ("ADMIN".equals(roleValue)) {
                    System.out.println("ADMIN権限が必要な処理: " + method.getName());
                } else if ("USER".equals(roleValue)) {
                    System.out.println("USER用処理: " + method.getName());
                }
            }
        }
    }
}
```

---

## ✅ 2. APT（Annotation Processing Tool）でアノテーションの値を読み取る（コンパイル時）

### ◉ カスタムアノテーション定義

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.TYPE)
public @interface GenerateClass {
    String className();
}
```

---

### ◉ アノテーション付きクラス

```java
@GenerateClass(className = "HelloGenerated")
public class MyOriginalClass {
}
```

---

### ◉ APTプロセッサ実装（`javax.annotation.processing.AbstractProcessor`）

```java
import javax.annotation.processing.*;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.*;
import java.io.IOException;
import java.io.Writer;
import java.util.Set;

@SupportedAnnotationTypes("GenerateClass")
@SupportedSourceVersion(SourceVersion.RELEASE_17)
public class GenerateClassProcessor extends AbstractProcessor {

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        for (Element element : roundEnv.getElementsAnnotatedWith(GenerateClass.class)) {
            GenerateClass annotation = element.getAnnotation(GenerateClass.class);
            String className = annotation.className();

            try (Writer writer = processingEnv.getFiler().createSourceFile(className).openWriter()) {
                writer.write("public class " + className + " {\n");
                writer.write("    public static void sayHello() {\n");
                writer.write("        System.out.println(\"Hello from generated class!\");\n");
                writer.write("    }\n");
                writer.write("}\n");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return true;
    }
}
```

📌 このプロセッサを `META-INF/services/javax.annotation.processing.Processor` に登録することで有効になります。

---

## 🔚 補足と結論

| 技術 | タイミング | 処理対象 | 用途例 |
| --- | --- | --- | --- |
| リフレクション | 実行時 | メソッド/フィールド/クラス | DI、バリデーション、ログ処理など |
| APT | コンパイル時 | ソースコード | 自動生成、バリデーション、コードチェック |

---

リフレクションの方が試しやすいですが、APTは静的解析・コード自動生成の分野で強力です。

必要であれば「Lombok風の自動コード生成」や「独自DIの注入処理」などの高度な例もご用意できます。

# Q,

ここでいうAPTが、プロセッサという認識で問題ないでしょうか？
リフレクションはプロセッサではない。

はい、まさにその通りです。

Hiroakiさんの認識は **完全に正確**です。

---

## ✅ APT（Annotation Processing Tool）＝プロセッサ

- APTとは、**「コンパイル時」にアノテーションを解析し、追加のソース生成などを行う仕組み**です。
- 実際には `javax.annotation.processing.Processor` インタフェースを実装したクラス（または `AbstractProcessor` のサブクラス）で処理されます。

> 🔸 よって、APT = アノテーションプロセッサ（Processor） という認識で問題ありません。
> 

---

## 🟢 一方、リフレクションはプロセッサではない

- リフレクション（Reflection）は **「実行時」にアノテーションやクラス情報を読み取り・操作する仕組み**
- これは、`java.lang.reflect` パッケージや `Class` クラスを通じて行われます。
- コンパイル時ではなく、**プログラムがすでに実行されている段階で使う**ものです。

---

## ✅ 比較表：APT（プロセッサ） vs リフレクション

| 項目 | APT（アノテーションプロセッサ） | リフレクション |
| --- | --- | --- |
| 実行タイミング | コンパイル時 | 実行時 |
| 主な用途 | ソース生成、バリデーション | 動的呼び出し、DI、バリデーション |
| 技術名 | `javax.annotation.processing.Processor` | `java.lang.reflect.*` |
| 属性取得方法 | `element.getAnnotation(MyAnno.class).value()` | `clazz.getMethod(...).getAnnotation(...).value()` |
| 処理の出力 | ファイル生成・警告表示など | 実行中の処理そのもの |
| 開発例 | Lombok、MapStruct、Dagger | Spring, Hibernate, Jackson, JUnit |

---

## ✅ 結論

- ✅ **APTは「プロセッサ」そのもの**
- ✅ **リフレクションはプロセッサではない**（実行時API）

---

もし今後、APTとリフレクションを **同時に使い分けるようなツールの設計（例：アノテーションベースのDIやコード生成）** を考えている場合は、実務での設計パターンも含めてご提案できますので、ぜひご相談ください。

# Q,

APTを使用する場合は、メソッド単位でタグ付けするのではなくクラス単位でタグ付けしその内部のフィールドやメソッドも呼び出せるイメージでしょうか？
リフレクションより呼び出せる範囲が広い認識です。

はい、Hiroakiさんの認識は**ほぼ正確**であり、非常に良い観点です。

ただし、いくつか補足するとより明確になります。

---

## ✅ 回答概要

> ◉ APT（Annotation Processing Tool）は、クラス単位だけでなく、メソッドやフィールド単位でもタグ付け（注釈）できます。
> 
> 
> ◉ ただし、APTでは「アノテーションが付与された要素（Element）」だけでなく、**その内部構造（子要素）にもアクセスできる**ため、結果的に「広く深く扱える」構造になっています。
> 

> 🔸 よって、「リフレクションより呼び出せる範囲が広い」というのは、構造解析という観点では正しい認識です。
> 

---

## 🔍 APTで扱える構造：クラスだけではない

APT（`AbstractProcessor` の `process` メソッド）では、次のようにして **アノテーションが付けられたすべての構造要素**を処理対象にできます：

```java
for (Element element : roundEnv.getElementsAnnotatedWith(MyAnnotation.class)) {
    // element はクラス、メソッド、フィールド、パッケージなど
}
```

さらに、次のようにして **子要素（メソッド・フィールドなど）** にアクセスできます：

```java
if (element.getKind() == ElementKind.CLASS) {
    for (Element enclosed : element.getEnclosedElements()) {
        // メソッドやフィールドにアクセスできる
    }
}
```

---

## 🔸 APTとリフレクションの「呼び出せる範囲」比較

| 観点 | APT（プロセッサ） | リフレクション |
| --- | --- | --- |
| 対象 | Element（あらゆる構造：クラス、メソッド、フィールド、パッケージ） | Class, Method, Field など |
| タグ付け位置 | クラス、メソッド、フィールド、パッケージも可能 | 同上（ただし基本は実行時対象） |
| 内部構造取得 | `getEnclosedElements()` で自由に探索可能 | クラス配下の構造に限定的アクセス |
| 処理対象 | ソースコード構造全体 | 実行中のクラス構造のみ |
| ソース生成 | ✅ 可能（Filer API） | ❌ 不可（既存構造の操作のみ） |

---

## 💡 実例：APTでクラスに注釈→内部のフィールドとメソッドを読み取り

```java
for (Element element : roundEnv.getElementsAnnotatedWith(Entity.class)) {
    if (element.getKind() == ElementKind.CLASS) {
        // クラス名
        String className = element.getSimpleName().toString();

        // 内部フィールドやメソッドを取得
        for (Element e : element.getEnclosedElements()) {
            if (e.getKind() == ElementKind.FIELD) {
                System.out.println("フィールド: " + e.getSimpleName());
            } else if (e.getKind() == ElementKind.METHOD) {
                System.out.println("メソッド: " + e.getSimpleName());
            }
        }
    }
}
```

---

## ✅ 結論

- ✅ APTは「**クラスにアノテーションを付けて、内部のフィールドやメソッドもまとめて処理する**」のに適している
- ✅ ただし、「**クラス以外（メソッド単体、フィールド単体）にもアノテーションを付けて処理する**」ことも可能
- ✅ リフレクションと比較して、「**より静的に・より広く・より深く構造にアクセスできる**」という点で **呼び出し範囲が広いという認識は正しい**

---

必要であれば「APTで複数の注釈を同時に扱う」「@Target を使って意図的に制限する」などの応用技術もご紹介可能です。