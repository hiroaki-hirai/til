# System.out.println()

- `System.out.println(オブジェクト)`
    
    は、**内部的に**そのオブジェクトの`valueOf()`→ `toString()` メソッドを自動的に呼び出して出力します。
    

つまり

```java
    public void println(String x) { 
        if (getClass() == PrintStream.class) { 
            writeln(String.valueOf(x));
        } else {
            synchronized (this) {
                print(x);
                newLine();
            }
        }
    }
```

```java
    public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    }
```

※ ここで動的バインディングが起こり、エントリーポイントにて実際に渡された**実体型（インスタンス型）**に応じてtoStringの型が決まる。

---

## 【流れをざっくりまとめると】

```java
System.out.println(obj);
```

を実行すると、**内部では**こんな順番で処理されます。

1. `println(Object x)` が呼ばれる
2. `String.valueOf(x)` が呼び出される
3. `x == null` かどうかチェック
    - nullなら `"null"` という文字列を返す
    - nullでなければ `x.toString()` を呼び出して、その結果の文字列を返す
    
      → ここで、動的バインディング発生
    
4. 最後に、文字列を標準出力に流し込む（改行もつける）

---

## 🔥 【要点まとめ】

| 観点 | 説明 |
| --- | --- |
| `System.out.println(obj)` | = `System.out.println(String.valueOf(obj))` と同じ |
| nullチェックあり | nullなら"null"文字列、例外は出ない |
| toString()呼び出し | オブジェクトのtoString()が呼ばれる |
| 改行あり | printlnだから自動で改行がつく |

---

## 【例：toString()をオーバーライドしていない場合】

```java
class Dog {}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        System.out.println(d);
    }
}
```

【出力例】

```java
Dog@6d06d69c
```

👉

これは

- クラス名（Dog）
- アットマーク（@）
- ハッシュコード（オブジェクトのメモリアドレスっぽいものを16進数表記）
    
    を連結した文字列です。
    

---

## 🔥 【toString()オーバーライドの効果まとめ】

| 観点 | 説明 |
| --- | --- |
| 目的 | オブジェクトを**見やすく**、**意味のある形で**表示するため |
| 使い道 | ログ出力、デバッグ、画面表示 |
| デフォルトの動き | クラス名@ハッシュコード |
| オーバーライド後 | 好きな文字列にできる（状態をわかりやすく出せる） |

---

# 🎯 【ここまでのまとめ】

- `System.out.println(obj)`は内部で`obj.toString()`が呼び出される
- toString()をオーバーライドしていないと、クラス名＋ハッシュコードが出る
- toString()をオーバーライドすると、**わかりやすい情報**を出せる