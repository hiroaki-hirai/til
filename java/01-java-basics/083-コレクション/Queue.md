# Queue

Javaの`Queue`は、**「先入れ先出し（FIFO: First-In First-Out）」のデータ構造**を扱うためのインターフェースで、**待ち行列のような動作**をプログラムで表現する際に使われます。

---

## ✅ Queueとは？

```java
public interface Queue<E> extends Collection<E>
```

- Javaの`Queue`はインターフェースであり、`List`などと同じく`Collection`の一種です。
- 要素は**順番に並び、先に入れたものから順番に取り出す**構造です（ただし実装によって順序の扱いは異なります）。

---

## 🔸 主な用途

- **プリントキュー**、**タスクスケジューラ**、**イベント処理**など
- データを一時的に順番に保持し、順に処理したいケースで使用

---

## 🔸 主なメソッド

| メソッド | 説明 | 失敗時の挙動 |
| --- | --- | --- |
| `add(e)` | 要素を追加。容量制限に達した場合は例外を投げる | 例外 `IllegalStateException` |
| `offer(e)` | 要素を追加。容量制限に達しても `false` を返す | `false` を返す |
| `remove()` | 先頭を削除して返す | 例外 `NoSuchElementException` |
| `poll()` | 先頭を削除して返す | `null` を返す |
| `element()` | 先頭を参照する（削除しない） | 例外 |
| `peek()` | 先頭を参照する（削除しない） | `null` を返す |

🟡 `add/remove/element`：**例外を投げる厳格版**

🟢 `offer/poll/peek`：**nullで返す緩やかな版**

---

## 🔸 主な実装クラス

| クラス名 | 特徴 |
| --- | --- |
| `LinkedList` | `Queue`と`Deque`の両方を実装。双方向リスト。汎用性が高い。 |
| `ArrayDeque` | 配列ベースの`Deque`実装。スタック/キュー両対応。 |
| `PriorityQueue` | **要素を並び替えて取り出す**（自然順またはComparator） |
| `ConcurrentLinkedQueue` | 非同期スレッドセーフなキュー。 |
| `BlockingQueue`系 | スレッド間通信・待機処理を含む高機能なキュー。 |

---

## 🔸 使用例：基本の FIFO 処理

```java
Queue<String> queue = new LinkedList<>();
queue.offer("A");
queue.offer("B");
queue.offer("C");

System.out.println(queue.poll()); // A
System.out.println(queue.poll()); // B
System.out.println(queue.poll()); // C
System.out.println(queue.poll()); // null
```

---

## 🔸 PriorityQueue の例（優先度付き）

```java
Queue<Integer> pq = new PriorityQueue<>();
pq.offer(30);
pq.offer(10);
pq.offer(20);

System.out.println(pq.poll()); // 10（昇順に取り出される）
```

🔸 `PriorityQueue` は、**並び順ではなく取り出し順を優先度順に制御**します。

---

## 🔸 QueueとListの違い（Silver/Gold試験にも出やすい）

| 特徴 | Queue | List |
| --- | --- | --- |
| アクセス方法 | 先頭・末尾のみ（FIFO） | インデックスで自由にアクセス |
| 主な用途 | 順次処理、待機行列 | 汎用的な要素の格納 |
| 操作方法 | `offer/poll/peek` 等 | `get/set/add/remove` など |

---

## ✅ まとめ

- `Queue` はインターフェースで、**先頭から取り出す構造**（FIFO）が基本
- 例外を投げる系と`null`や`false`を返す系でメソッドがペアになっている
- `LinkedList` や `PriorityQueue` など複数の実装がある
- `Deque`（両端キュー）により **スタックや双方向キュー** の実装も可能

---

さらに：

- `Deque` との比較
- `BlockingQueue` や `ArrayBlockingQueue` などのスレッド制御
- `Queue` のパターン問題（Silver/Gold風）