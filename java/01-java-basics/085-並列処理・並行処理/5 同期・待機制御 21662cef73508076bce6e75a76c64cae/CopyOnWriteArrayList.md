# CopyOnWriteArrayList

# Q,

CopyOnWriteArrayListについて解説してください

はい、Hiroakiさんのご質問にお答えして、**`CopyOnWriteArrayList`** について詳しく解説いたします。これは `java.util.concurrent` パッケージにある、**スレッドセーフなリストの一種**であり、特定の用途において非常に効果的です。

---

## 🔷 CopyOnWriteArrayList とは？

### 📌 定義：

> 書き込みが発生するたびに内部配列をコピーして新しいバージョンを作成するスレッドセーフな List 実装
> 
- `ArrayList` に似た使い方ができる
- 読み取りが非常に多く、書き込みが少ない状況に最適
- **すべての変更操作（add/remove/setなど）は、新しいコピーを作成して反映する**

---

## ✅ 特徴まとめ

| 特性 | 内容 |
| --- | --- |
| スレッドセーフ | 内部的にロックを使いながらコピー操作を行うため、複数スレッドで同時に安全にアクセス可能 |
| 読み取りが高速 | 読み取り時はロックなしで読み込める（＝配列参照だけ） |
| 書き込みが重い | 書き込み操作ごとに**新しい配列を生成するためコスト大** |
| Iterator は安全 | 取得時点のスナップショットを保持するため、**ConcurrentModificationException を投げない** |

---

## 📘 使用例

```java
import java.util.concurrent.CopyOnWriteArrayList;

public class Example {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();

        list.add("Java");
        list.add("Python");
        list.add("Go");

        for (String lang : list) {
            System.out.println(lang);
        }

        // 書き込み中にループしても安全
        for (String lang : list) {
            list.add("New-" + lang); // ← これでもConcurrentModificationExceptionにならない！
        }
    }
}
```

---

## 🔄 書き込みの仕組み（イメージ図）

```java
変更前配列:  ["A", "B", "C"]
↓ add("D")
コピー:      ["A", "B", "C", "D"] ← 新しい配列を作成
内部状態更新: 参照を新配列に差し替え
```

→ 読み取り中の他スレッドには旧配列が見えているので**干渉しない**

---

## 🔧 主なメソッド（`List` インタフェースを継承）

| メソッド名 | 説明 |
| --- | --- |
| `add(E e)` | 要素を追加（コピー発生） |
| `remove(Object o)` | 要素を削除（コピー発生） |
| `get(int index)` | 要素を取得（高速） |
| `set(int index, E element)` | 要素を上書き（コピー発生） |
| `iterator()` | スナップショットを返すイテレータ（変更検出しない） |

---

## 🚦 適切な使用ケースと注意点

### ✅ 向いているケース（読み取り多・書き込み少）

- **監視対象の一覧**
- **イベントリスナーの登録リスト**
- **読み取りが頻繁で、更新がまれなキャッシュ**
- **イテレータで並列読み込みをする場面**

### ❌ 不向きなケース（頻繁な書き込み）

- リアルタイムで大量に `add/remove` を繰り返す処理
- 書き込みコストが高く、メモリ消費も大きくなりやすい

---

## 🔍 他のスレッドセーフコレクションとの比較

| クラス | 特徴 | 読み取り性能 | 書き込み性能 | Iterator 安全性 |
| --- | --- | --- | --- | --- |
| `CopyOnWriteArrayList` | 読み取り中心向け | ✅ 非常に高速 | ❌ 高コスト（コピー発生） | ✅ 安全（スナップショット） |
| `Collections.synchronizedList` | `ArrayList`をラップ | ❌ ロックあり | ❌ ロックあり | ❌ 変更中に例外発生の可能性 |
| `ConcurrentLinkedQueue` | キュー構造 | ✅ 良好 | ✅ 非ブロッキング | ✅ イテレータも安全だが「弱一貫性」 |

---

## 🏁 まとめ

| 項目 | 内容 |
| --- | --- |
| 読み取り性能 | 非常に高い（ロック不要） |
| 書き込みコスト | 高い（毎回配列コピー） |
| スレッドセーフ | 完全に保証されている |
| 主な用途 | 読み取り中心の並列アクセス、イベントリスナー、参照スナップショット |
| 注意点 | 頻繁な更新がある場面には不適切 |

---

さらにご希望があれば、`CopyOnWriteArrayList` を使用した**マルチスレッドの具体的サンプルアプリ**もご紹介できます！