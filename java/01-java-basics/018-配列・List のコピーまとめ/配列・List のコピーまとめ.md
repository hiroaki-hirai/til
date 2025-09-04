# 配列・List のコピーまとめ

### ▢ 配列のコピー (シャローコピー、ディープコピー)

1次元プリミティブ配列 | 最も簡単（cloneだけでOK）

1次元オブジェクト配列 | 配列＋要素ごとにdeep copy

2次元プリミティブ配列 | 配列ごとにclone（ネストforが必要）

2次元オブジェクト配列 | 配列clone＋要素deep copy（さらに複雑）

任意次元配列(Object[][][]など) | 再帰的deep copyが必要

### 1次元配列の場合 - プリミティブ型配列

```java
int[] scores = {1,2,3};int[] clonedScores = scores.clone();
```

scoresとclonedScoresは、別の参照を持つ → 変更で影響なし
※1次元配列でも参照型配列の場合は、さらにclone()が必要
##### String 編
→ Stringはイミュータブルなので変更の影響なし。newで独立オブジェクトを作れるのでそちらでコピー
※ Stringは、Cloneableインターフェースを実装していないのでclone()なし → アクセスするとコンパイルエラー

- shallow copy

```java
String[] scores = {"A","B","C"};
String[] clonedScores = scores.clone();
```

- deep copy

```java
String[] scores = {"A", "B", "C"};
String[] clonedScores = new String[scores.length];

for (int i = 0; i < scores.length; i++) {
		// new String()で明示的に新しいオブジェクトを作る    
		clonedScores[i] = new String(scores[i]);
}
```

### オブジェクト 編

```java
Person[] people = {    new Person("Alice", 20),    new Person("Bob", 25)};
```

- コピーコンストラクタを使用する場合

```java
public class Person {
    private String name;
    private int age;
    
    // コピーコンストラクタ
    public Person(Person other) {
            this.name = other.name; // Stringはイミュータブルなので参照コピーでOK
            this.age = other.age;
    }
}
```

```java
Person[] copiedPeople = new Person[people.length];

for (int i = 0; i < people.length; i++) {
    copiedPeople[i] = new Person(people[i]); // 個別に新しく作る！}
```

- clone()を実装する場合

```java
public class Person implements Cloneable {
    private String name;
    private int age;
    @Override
    public Person clone() {
        try {
            return (Person) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();        }    }}
```

```java
Person[] copiedPeople = new Person[people.length];
for (int i = 0; i < people.length; i++) {
    copiedPeople[i] = people[i].clone(); // 個別にclone！}
```

### 2次元配列の場合 - プリミティブ型配列

- shallow copy

```java
int[][] arrayA = {{1,2},{1,2},{1,2,3}};
int[][] arrayB = arrayA.clone();
```

外側の配列(1階層目)は、別の参照を持つが、中の小配列(2階層目)は、同じ参照のまま。 → 変更がそのまま反映

以下のように2階層目も順にコピーする
- deep copy

```java
int[][] arrayA = {{1,2},{1,2},{1,2,3}};
// 外側をcloneして新しい配列を作る
int[][] arrayB = arrayA.clone();
// さらに内側（小配列）も個別にcloneする
for (int i = 0; i < arrayA.length; i++) {
    arrayB[i] = arrayA[i].clone();}
```

#####　オブジェクト型の多次元配列

```java
Person[][] people = {
    { new Person("Alice", 20), new Person("Bob", 25) },
    { new Person("Charlie", 30) }};
```

```java
Person[][] copiedPeople = new Person[people.length][];
 // 外側配列のコピー
 for (int i = 0; i < people.length; i++) {
 　  //　内側配列のコピー
     copiedPeople[i] = new Person[people[i].length];
     for (int j = 0; j < people[i].length; j++) {
     // 中のオブジェクトのコピー
          copiedPeople[i][j] = new Person(people[i][j]);
           // コピーコンストラクタ or clone() を使用
      }}
```

### 3次元配列の場合 - プリミティブ型配列

```java
int[][][] arrayA = {
    {
            {1,2,3},        {4,5,6}    },    {        {7,8},        {9,10}    }};
            // 外側だけ
            cloneint[][][] arrayB = arrayA.clone();// 中間層（2次元配列）をclone
            for (int i = 0; i < arrayA.length; i++) {
                arrayB[i] = arrayA[i].clone();    // 最内層（1次元配列）もclone    
                for (int j = 0; j < arrayA[i].length; j++) {
                        arrayB[i][j] = arrayA[i][j].clone();    }}
```

### Listの場合

- shallow copy

```java
ArrayList<String> listA = new ArrayList<>();listA.add("A");listA.add("B");ArrayList<String> listB = (ArrayList<String>) listA.clone();
```

- deep copy

```java
ArrayList<ArrayList<Integer>> listA = new ArrayList<>();listA.add(new ArrayList<>(List.of(1, 2)));listA.add(new ArrayList<>(List.of(3, 4)));ArrayList<ArrayList<Integer>> listB = new ArrayList<>();for (ArrayList<Integer> innerList : listA) {    listB.add(new ArrayList<>(innerList)); // 内側のリストも新しく作り直す！}
```

### 再帰＋リフレクションで動的にディープコピー

```java
Object[] array = {    new Object[] {        new int[] {1, 2},        new int[] {3, 4}    },    new Object[] {        new int[] {5, 6}    }};
```

```java
import java.lang.reflect.Array;public class DeepCopyUtil {    public static Object deepCopy(Object array) {        if (array == null) {            return null;        }        if (!array.getClass().isArray()) {            return array; // 配列でなければそのまま返す（イミュータブルオブジェクト想定）        }        int length = Array.getLength(array);        Object copiedArray = Array.newInstance(array.getClass().getComponentType(), length);        for (int i = 0; i < length; i++) {            Object element = Array.get(array, i);            Object copiedElement = deepCopy(element); // 再帰呼び出し！            Array.set(copiedArray, i, copiedElement);        }        return copiedArray;    }}
```

## 🤔 疑問・今後調べること・今後の課題

- メソッド洗い出し
- Java Silver 問題傾向分析 続き
- 再帰＋リフレクションによるDeepCopyの理解

## 🛠️ 実行したコード or コマンド

### 

## 🧩学習方針・ロードマップ

以下の項目、スケジュールで学習していきます。

### **第1フェーズ：Java言語の基礎と実行環境の理解**

| 項目 | 内容 |
| --- | --- |
| JDK / JRE / JVM | Javaアプリがどのように動いているかを理解 |
| Java文法復習（Silverレベル） | 基本構文・制御構文・オブジェクト指向・例外処理 |
| Java API | String, List, Map, Stream などの標準ライブラリの使い方 |
| コレクションAPI・ラムダ式・Stream API | データ処理の基本として重要（実務で頻出） |

### **第2フェーズ：設計力の基礎とコードの質の向上**

| 項目 | 内容 |
| --- | --- |
| SOLID原則 | 拡張しやすく壊れにくい設計の5原則 |
| デザインパターン | Strategy, Factory, Decorator, Compositeなど代表パターン |
| リファクタリング | コードの可読性・保守性向上。メソッド抽出、分割、置換など |
| JLS / JPMS | Java仕様の深掘り、モジュール化構造の理解（余裕があれば） |

### **第3フェーズ：テストと品質保証の実践**

| 項目 | 内容 |
| --- | --- |
| JUnit（4 or 5） | 単体テストの書き方、アサーション、セットアップ、例外テスト |
| Mockito | モックによる依存の切り離し |
| TDD（テスト駆動開発） | テスト→実装→リファクタの循環型開発 |
| テスト設計技法 | 等価クラス、境界値、状態遷移など（基本だけでOK） |

### **第4フェーズ：実務開発の準備（フレームワーク／構成管理）**

| 項目 | 内容 |
| --- | --- |
| フレームワーク基礎 | Spring Framework（DI / AOP / MVC / Boot） |
| ビルドツール | Maven or Gradle（依存管理、ビルド、JAR作成） |
| ログ設計 | Logback / SLF4J（ログレベル、ロガーの設計） |
| 例外処理設計 | ハンドリング方針、ログ連携、ユーザーフィードバック方法 |
| レイヤー設計 | Controller / Service / Repository などの役割分離 |

### 学習期間目安

| フェーズ | 学習期間の目安 | 目的 |
| --- | --- | --- |
| 第1フェーズ | 1～2ヶ月 | Javaの基本力を身につける |
| 第2フェーズ | 1ヶ月前後 | 設計と品質への意識を高める |
| 第3フェーズ | 1ヶ月 | 品質保証・テスト技術を実装と共に理解 |
| 第4フェーズ | 2ヶ月～ | 実務で使える構造・運用・フレームワーク技術を習得 |