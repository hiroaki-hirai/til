# Arrays



Javaの配列はプリミティブ型であり、クラスではありませんが、配列の操作には `java.util.Arrays` や `System` クラスのメソッドなどを使うのが一般的です。以下に「配列操作で抑えておくべき標準API」を目的別にリスト化します。

---

### ✅ **配列操作の標準APIリスト（覚えておくべき主要メソッド）**

| 分類 | メソッド | 概要・用途 |
| --- | --- | --- |
| 🔍 **検索・比較** | `Arrays.equals(a, b)` | 配列同士が等しいか比較（要素順序も含めて） |
|  | `Arrays.deepEquals(a, b)` | 多次元配列や参照型配列の中身も含めて比較 |
|  | `Arrays.binarySearch(a, key)` | ソート済み配列から高速検索（※要ソート済み） |
| 🧪 **判定系** | `Arrays.asList(a)` | 配列をListに変換（固定長・ラッパーList） |
| 🛠 **変換系** | `Arrays.copyOf(a, newLength)` | 配列のコピーと長さ変更（後ろは0埋め） |
|  | `Arrays.copyOfRange(a, from, to)` | 部分配列のコピー（`from` ～ `to - 1`） |
|  | `Arrays.toString(a)` | 配列の内容を文字列で表示 |
|  | `Arrays.deepToString(a)` | 多次元配列を再帰的に文字列化 |
| 🔄 **並べ替え** | `Arrays.sort(a)` | 昇順にソート（要素はComparable） |
|  | `Arrays.sort(a, Comparator)` | 独自Comparatorでソート（オブジェクト配列） |
| 🎲 **乱数・シャッフル** | `Collections.shuffle(Arrays.asList(a))` | 配列をランダムに並び替え（List変換必須） |
| ♻ **変更・初期化** | `Arrays.fill(a, val)` | 配列のすべての要素を指定値に設定 |
|  | `Arrays.setAll(a, i -> i * i)` | 関数型で配列の全要素を一括設定（Java 8以降） |
|  | `System.arraycopy(src, srcPos, dest, destPos, length)` | 高速コピー（`native`実装） |
| 📏 **長さ取得** | `array.length` | フィールド（メソッドではない） |

---

### 📝 備考・注意点

- `array.length` は **フィールド** で、`length()` ではありません。
- `Arrays.asList()` で生成された List はサイズ変更不可です（`add()` や `remove()` は例外）。
- `System.arraycopy()` は **パフォーマンスが良く**, forループより高速な場面があります。
- `Arrays.deep～` 系は参照型の配列（例：`String[][]`）で重要。

# Q,

Arraysクラスの主なメソッドをまとめてください。

はい、`java.util.Arrays` クラスは、**配列（Array）操作のユーティリティメソッド**を多数提供しています。以下に主なメソッドを分類・用途別に整理します。

---

## ✅ 配列の操作用途別まとめ（`java.util.Arrays`）

### 1. 🔍 検索・探索系

| メソッド | 概要 | 備考 |
| --- | --- | --- |
| `binarySearch(array, key)` | ソート済み配列に対して二分探索 | 見つからなければ `-(挿入点 + 1)` を返す |
| `binarySearch(array, fromIndex, toIndex, key)` | 範囲指定版 | `fromIndex` 以上 `toIndex` 未満 |

---

### 2. 📋 並び替え（ソート）系

| メソッド | 概要 | 備考 |
| --- | --- | --- |
| `sort(array)` | 昇順にソート | プリミティブ型 / 参照型両対応 |
| `sort(array, fromIndex, toIndex)` | 範囲ソート | 参照型は`Comparable`が必要 |
| `parallelSort(array)` | 並列ソート | Java 8以降：大きな配列に高速 |
| `sort(array, Comparator)` | カスタム順序 | 参照型配列のみ有効 |

---

### 3. 🆚 比較・等価判定系

| メソッド | 概要 | 備考 |
| --- | --- | --- |
| `equals(array1, array2)` | 各要素の等価性を比較（深さ1） | `==` ではなく、値の比較 |
| `deepEquals(array1, array2)` | 多次元配列の等価性 | 再帰的に比較 |
| `compare(array1, array2)` | 辞書順の比較（Java 9～） | `int[]`などに対応（昇順） |
| `mismatch(array1, array2)` | 最初に異なる要素のインデックス（Java 9～） | 同一なら `-1` |

---

### 4. 🧪 生成・コピー系

| メソッド | 概要 | 備考 |
| --- | --- | --- |
| `copyOf(array, newLength)` | 長さを指定してコピー | 足りない分は `0` または `null` |
| `copyOfRange(array, from, to)` | 範囲を指定してコピー | `from` 以上 `to` 未満 |
| `fill(array, value)` | 全要素に値を一括代入 | 一括初期化に便利 |
| `setAll(array, IntFunction)` | 各要素に関数で代入（Java 8～） | `i -> i * i` などで柔軟に |

---

### 5. 🖨️ 表示・変換系

| メソッド | 概要 | 備考 |
| --- | --- | --- |
| `toString(array)` | 配列内容を文字列に変換 | 1次元のみ対応 |
| `deepToString(array)` | 多次元配列の内容も文字列化 | ネストされた配列もOK |
| `stream(array)` | `Stream` に変換（参照型） | Java 8～、配列→Stream |
| `stream(array, from, to)` | 範囲指定の `Stream` | `from` 以上 `to` 未満 |

---

### 6. 🧵 その他（並列・関数型）

| メソッド | 概要 | 備考 |
| --- | --- | --- |
| `parallelPrefix(array, operator)` | 累積処理（畳み込み） | Java 8～、関数型処理 |
| `parallelSetAll(array, i -> func(i))` | 並列で初期化 | 要 `IntUnaryOperator` |

---

## 🔚 まとめ表（主用途別）

| 分類 | 代表メソッド |
| --- | --- |
| 検索 | `binarySearch` |
| ソート | `sort`, `parallelSort` |
| 比較 | `equals`, `deepEquals`, `compare`, `mismatch` |
| コピー | `copyOf`, `copyOfRange` |
| 初期化 | `fill`, `setAll` |
| 表示 | `toString`, `deepToString` |
| Stream変換 | `stream` |
| 関数処理 | `parallelSetAll`, `parallelPrefix` |

---

必要に応じて、**個別のコード例つきで解説**も可能です。気になるメソッドがあれば、お知らせください。