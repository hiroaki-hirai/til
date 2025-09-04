# 起動パラメータとSystem.in

# Q,

起動パラメータからの入力の場合もSystem.inで受けることができますか？

いいえ、起動パラメータ（コマンドライン引数）の場合は **`System.in` では受け取れません**。

Javaでは、起動パラメータは**`main`メソッドの引数 `String[] args`** に渡されます。

---

## 1. 起動パラメータの受け取り方法

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println("引数: " + arg);
        }
    }
}
```

実行例：

```java
java Main apple banana
```

出力：

```java
引数: apple
引数: banana
```

- `args[0]` → `"apple"`
- `args[1]` → `"banana"`

---

## 2. `System.in`との違い

| 項目 | コマンドライン引数 (`args`) | 標準入力 (`System.in`) |
| --- | --- | --- |
| 入力タイミング | プログラム起動時にOSから渡される | プログラム実行中にユーザーや他プロセスから送られる |
| データ形式 | すでに**文字列（String）**として渡される | **バイト列（InputStream）**として渡される（必要に応じてReaderで文字化） |
| 例 | `java Main test 123` → `args = {"test", "123"}` | `echo test 123 |

---

## 3. なぜ `System.in` で起動パラメータを受け取れないのか

- 起動パラメータは**標準入力ではなく、OSのプロセス起動時の引数領域**として渡されるため
- JVMはこの引数領域を`String[] args`に展開して`main`に渡す
- 標準入力 (`System.in`) は、実行後にキーボードやパイプなどから送られるデータ用

---

## 4. 両方使う例

```java
public static void main(String[] args) throws Exception {
    System.out.println("引数:");
    for (String arg : args) {
        System.out.println(arg);
    }

    System.out.println("標準入力から1行:");
    try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
        String line = br.readLine();
        System.out.println("入力: " + line);
    }
}
```

実行例：

```java
echo Hello | java Main apple banana
```

出力：

```java
引数:
apple
banana
標準入力から1行:
入力: Hello
```

---

つまり、

**起動パラメータは `args[]` で受け取る**

**実行中の外部データは `System.in` で受け取る**

という住み分けになります。

---

希望があれば、この `args` と `System.in` の**使い分け早見表＋データフロー図**を作って、完全に整理できます。

# Q,

args と [System.in](http://system.in/) の使い分け早見表＋データフロー図を作成してください。

了解！`args`（コマンドライン引数）と `System.in`（標準入力）の**使い分け早見表＋データフロー図**をまとめました。保存用にコピペOKです。

---

# 使い分け早見表

| 項目 | **`String[] args`（起動パラメータ）** | **`System.in`（標準入力）** |
| --- | --- | --- |
| 入力のタイミング | **起動時に一度だけ** OSから渡される | **実行中いつでも** キーボード/パイプ/リダイレクトから流入 |
| データの形 | すでに **文字列（String）** | **バイト列（InputStream）** → 必要なら `Reader` で文字化 |
| 典型用途 | フラグ・設定値・入力ファイルパスなど **実行条件** | 本文・データ本体、長文、多行、ストリーム処理 |
| 容量/長さ | OS依存の制限あり（短め前提） | 実質大きい（ファイル/パイプで大量OK） |
| パースの負担 | ほぼ不要（文字列配列） | 自分で分解：`BufferedReader`/`Scanner`等 |
| 再読込み | 不可（固定） | 可能（継続的に読み進める） |
| 例 | `java Main --mode=prod input.csv` | `cat data.json | java Main` / `java Main < input.txt` |
| Java 側の入口 | `public static void main(String[] args)` | `System.in` → `InputStreamReader` → `BufferedReader` / `Scanner` |

---

# データフロー図

```java
┌───────────────────────────────┐                 ┌───────────────────────────────┐
│ 起動時に渡す（OSの引数領域）          │                 │ 実行中に流す（標準入力）              │
│ 例: java Main foo bar             │                 │ 例: echo "Hello" | java Main     │
└────────────┬───────────────────┘                 └────────────┬───────────────────┘
             │                                                  │
             ▼                                                  ▼
     main(String[] args)                                System.in (InputStream: バイト)
             │                                                  │
             │（文字列としてそのまま使う）                      │（必要に応じて文字へ変換）
             ▼                                                  ▼
     args[i]（String）                                 InputStreamReader（文字コード変換）
                                                           │
                                                           ▼
                                                    BufferedReader / Scanner
                                                           │
                                                           ▼
                                                    アプリのロジックで処理
```

---

# 実務での選び方（超要約）

- **固定の設定・モード・ファイルパス指定** → `args`
- **中身のデータ（多行/大量/ストリーム）** → `System.in`
- **両方使う**：`args` で「どう処理するか」を決め、データ本体は `System.in` から受ける
    
    例）`java Main --format=json < data.json`
    

---

# 最小テンプレ

```java
public static void main(String[] args) throws Exception {
    // args: 実行条件・フラグ・ファイルパスなど
    for (String arg : args) {
        System.out.println("arg: " + arg);
    }

    // System.in: データ本体（必要なら文字へ）
    try (var br = new java.io.BufferedReader(
            new java.io.InputStreamReader(System.in, java.nio.charset.StandardCharsets.UTF_8))) {
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println("in: " + line);
        }
    }
}
```

必要なら、この表と図を**PDF/画像**に整形してお渡しします。欲しい体裁（A4縦/横、等）を教えてください！