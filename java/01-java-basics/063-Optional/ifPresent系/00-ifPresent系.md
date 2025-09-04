# ifPresent系

# Q,

```java
    public void ifPresent(Consumer<? super T> action) {
        if (value != null) {
            action.accept(value);
        }
    }

    public void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction) {
        if (value != null) {
            action.accept(value);
        } else {
            emptyAction.run();
        }
    }
```

ifPresent系の使用が想定されるケースにはどんなものがありますか？

`ifPresent(...)` や `ifPresentOrElse(...)` の使用が適しているのは、**Optionalに格納された値があるかないかに応じて「副作用（何らかの処理）」を起こしたいとき**です。

つまり、「値があるときは処理を実行したい（そして戻り値は不要）」というケースが対象になります。

---

## ✅ `ifPresent(...)` / `ifPresentOrElse(...)` の使用が想定される代表ケース

---

### ① 🔔 **ログ出力・通知・UI表示などの副作用処理**

```java
Optional<User> opt = findUser();

opt.ifPresent(user -> System.out.println("User found: " + user.getName()));
```

- 値があればその情報をログに出力
- 戻り値は不要（表示などで完結）

---

### ② 💾 **永続化・データ更新などのアクション実行**

```java
Optional<Data> dataOpt = repository.findDataById(id);

dataOpt.ifPresent(data -> auditService.recordAccess(data));
```

- 値があるときだけ、監査記録や処理を起こす
- 無ければ何もしない

---

### ③ 📩 **メール送信・通知などのイベントトリガー**

```java
Optional<String> emailOpt = user.getEmailAddress();

emailOpt.ifPresent(email -> mailer.sendVerification(email));
```

- メールアドレスがあれば通知を送る
- 無ければ黙ってスルー

---

### ④ 🪪 **ユーザー入力のバリデーション後の処理**

```java
Optional<String> nameOpt = Optional.ofNullable(form.getName());

nameOpt.ifPresent(name -> System.out.println("名前が入力されました: " + name));
```

---

## ✅ `ifPresentOrElse(...)` のユースケース（else側の処理がある場合）

### ⑤ ⛔ **デフォルト動作・警告・代替処理**

```java
Optional<User> opt = findUser();

opt.ifPresentOrElse(
    user -> System.out.println("User found: " + user.getName()),
    ()   -> System.out.println("User not found. Showing default screen.")
);
```

- 値がないときに**必ず何か処理が必要**
- 例えば警告表示・エラー通知・リダイレクトなど

---

### ⑥ ✅ **バリデーション + 入力エラー時の処理**

```java
Optional<String> nameOpt = Optional.ofNullable(form.getName());

nameOpt.ifPresentOrElse(
    name -> save(name),
    () -> showError("名前を入力してください")
);
```

---

### ⑦ 🔃 **状態の切り替え・状態遷移制御**

```java
Optional<State> next = current.getNextState();

next.ifPresentOrElse(
    s -> transitionTo(s),
    () -> handleNoNextState()
);
```

---

## ✅ 適さないケース（あえて避けるべき）

| 避けたほうがよいケース | 代替案 |
| --- | --- |
| 値を使って別の値に変換したい | `map()`, `flatMap()` |
| 値の有無によって値を返したい | `orElse()`, `orElseGet()` |
| 値がなければ例外を出したい | `orElseThrow()` |

[ifPresentOrElse()](ifPresent%E7%B3%BB%2021a62cef735080009638de07bc892c41/ifPresentOrElse()%2021b62cef7350802d8973fb6f5511457c.md)