# toString()

---

# ✅ 【代表的なtoStringオーバーライド済みクラス一覧】

| クラス | toString()の役割・内容 |
| --- | --- |
| **String** | そのまま自分自身を返す |
| **StringBuilder** | 内部にたまった文字列を返す |
| **StringBuffer** | 同上（スレッドセーフ版） |
| **ArrayList** | 要素をカンマ区切り＋[]で囲んだ形で返す |
| **HashMap** | {キー=値, キー=値} 形式で返す |
| **HashSet** | 要素をカンマ区切り＋[]で囲んだ形で返す |
| **Optional** | 値がある場合：Optional[値]／ない場合：Optional.empty |
| **Date** | 「Tue Apr 29 12:34:56 JST 2025」みたいな日時表記を返す |
| **LocalDate, LocalDateTime** | ISO-8601形式（例：2025-04-29T12:34:56）で返す |
| **Path (NIOのパスクラス)** | ファイルパスの文字列を返す |
| **File** | ファイルパスの文字列を返す |

---

# 📚 【よく出る例：実際の出力イメージ】

### ArrayList

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
System.out.println(list);
```

【出力例】

```java
[apple, banana]
```

→ 内部で`toString()`オーバーライド済み！

---

### HashMap

```java
Map<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);
System.out.println(map);
```

【出力例】

```java
{A=1, B=2}
```

→ これも `toString()` がオーバーライドされてます。

---

# 🎯 【要点まとめ】

| 観点 | 内容 |
| --- | --- |
| オーバーライドされてないと | クラス名@ハッシュコードが出るだけ（人間には読めない） |
| オーバーライドされていると | 中身がわかる形（リストの要素、マップのキーと値など）が出る |
| 基本ルール | 「中身を人間に見せたい」クラスはtoStringをオーバーライドしている |

---

# ✅ **toString()をオーバーライドしていない標準クラスリスト**

| クラス名 | 出力される内容 | 解説 |
| --- | --- | --- |
| **Object**（親クラス） | クラス名@ハッシュコード | 当然、基礎となるデフォルト |
| **Thread** | ThreadのIDや状態情報（実は部分的にオーバーライドされている） | ただし細かい情報までは出さない |
| **Class**（クラス情報を持つ） | クラスの完全修飾名（例：java.lang.String） | 人間に読めるが、内容ではない |
| **Throwable（Exceptionの親）** | クラス名だけ（メッセージは別メソッド） | `printStackTrace()`は別処理 |
| **Enum**（列挙型、標準のままの場合） | 列挙子の名前 | toString()自体はオーバーライドされてないが、列挙名が出る（ちょっと特殊） |
| **InputStream/OutputStream**（抽象クラス） | クラス名@ハッシュコード | 具体クラスでも特に中身表示されないことが多い |
| **Socket** | アドレス情報は出るけど、内部データは出ない | 情報量は少ない |
| **URL**（古いネットワーククラス） | 文字列化はされるが詳細ではない | toExternalForm()で詳細出せる |
| **Process** | 実行中プロセスのIDっぽいもの | プロセスの詳細内容までは見えない |

---

# 📚 【ポイントまとめ】

| 観点 | 内容 |
| --- | --- |
| 完全未オーバーライド | Object直伝のtoString()そのまま（クラス名@ハッシュコード） |
| 部分的・特殊なケース | Enum, Thread, Classなど（内容が見えたり、少し加工されてる） |
| 入出力系・ネットワーク系 | InputStream/OutputStream/Socket/Processなどは中身出ない |

---

# 🎯 【要注意ポイント】

- **基本的に「データ保持系のクラス」**（List、Map、Set、Optionalなど）はtoStringオーバーライド済み
- **「処理系クラス（スレッド、ストリーム、ネットワーク系）」はオーバーライドされてないか、ほとんど意味のない出力**が多い
- *例外系（Throwable）**は、toStringだけではメッセージもスタックトレースも出ない