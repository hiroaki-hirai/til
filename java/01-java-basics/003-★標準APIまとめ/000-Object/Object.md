# Object

---

# ✅ **Objectクラスで抑えておくべきメソッド一覧**

| メソッド名 | 説明 | 試験・実務重要度 |
| --- | --- | --- |
| **equals(Object obj)** | 内容が等しいか比較する（==は参照比較） | ★★★ |
| **hashCode()** | オブジェクトのハッシュコードを返す（equalsとセットで重要） | ★★★ |
| **toString()** | オブジェクトの文字列表現を返す（標準はクラス名@ハッシュコード） | ★★★ |
| **clone()** | オブジェクトの複製（浅いコピー）を作る | ★★☆ |
| **finalize()** | ガベージコレクション直前に呼び出される（現在は非推奨） | ★☆☆ |
| **getClass()** | 実行時のクラス情報（Classオブジェクト）を返す | ★★☆ |
| **wait()** | スレッドを一時停止して待機させる（synchronized下のみ） | ★☆☆ |
| **notify()** | waitしているスレッドを1つ起こす（synchronized下のみ） | ★☆☆ |
| **notifyAll()** | waitしているすべてのスレッドを起こす（synchronized下のみ） | ★☆☆ |

---

# ✍️ 【各メソッドの要点簡単まとめ】

| メソッド名 | 抑えるべきポイント |
| --- | --- |
| **equals(Object obj)** | ==（参照比較）と違い、内容比較に使う（要オーバーライド） |
| **hashCode()** | equals()とセット運用する（Map, Setに使われる） |
| **toString()** | デフォルトではクラス名@ハッシュコード、自作クラスではオーバーライド推奨 |
| **clone()** | 浅いコピー（フィールドが参照型なら参照もコピー）、Cloneableインターフェースが必要 |
| **finalize()** | 現在はDeprecated（GC前に呼ばれるが、もう使わない方針） |
| **getClass()** | 反射やデバッグ時に便利（`instanceof`と組み合わせる） |
| **wait()/notify()/notifyAll()** | マルチスレッド制御（高度な同期処理用）、基本synchronizedブロック内で使用 |

---

# ✍️ 【各メソッドの要点簡単まとめ】

| メソッド名 | 抑えるべきポイント |
| --- | --- |
| **equals(Object obj)** | ==（参照比較）と違い、内容比較に使う（要オーバーライド） |
| **hashCode()** | equals()とセット運用する（Map, Setに使われる） |
| **toString()** | デフォルトではクラス名@ハッシュコード、自作クラスではオーバーライド推奨 |
| **clone()** | 浅いコピー（フィールドが参照型なら参照もコピー）、Cloneableインターフェースが必要 |
| **finalize()** | 現在はDeprecated（GC前に呼ばれるが、もう使わない方針） |
| **getClass()** | 反射やデバッグ時に便利（`instanceof`と組み合わせる） |
| **wait()/notify()/notifyAll()** | マルチスレッド制御（高度な同期処理用）、基本synchronizedブロック内で使用 |

---

# 🎯 【試験対策の優先度】

| 優先 | 抑えるメソッド |
| --- | --- |
| 最優先 | equals(), hashCode(), toString() |
| 次点 | clone(), getClass() |
| さらに余裕があれば | wait(), notify(), notifyAll()（マルチスレッド問題向け） |

---

# 📚 【特に重要な3大セット】

| 項目 | 内容 |
| --- | --- |
| equals() | 内容の比較 |
| hashCode() | 同じequals()なら同じhashCode()にする必要あり（Map, Set運用のため） |
| toString() | デバッグ・ログ用にオーバーライド推奨 |

[toString()](Object%201e362cef73508008b6dbe0c9e48298e6/toString()%201e362cef735080a4bfd5d2c14f51eda2.md)