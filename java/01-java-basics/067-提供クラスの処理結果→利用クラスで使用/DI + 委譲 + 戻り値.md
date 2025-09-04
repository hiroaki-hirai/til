# DI + 委譲 + 戻り値

以下に、**Spring Boot**を想定した「DI + 委譲 + 戻り値」の基本構造（Controller → Service → Repository）をコードで示します。

---

## ✅ 構成概要

```java
UserController ─→ UserService ─→ UserRepository  
   ↑DI              ↑DI              ↑DI（通常はSpring Data JPAなど）
```

各層の責務：

- **Controller**：HTTPリクエスト受け取り、Serviceに委譲
- **Service**：ビジネスロジック処理
- **Repository**：データ取得・保存（DAO）

---

## 💡 サンプルコード（簡易版：ユーザー名の取得）

### 1. `User` エンティティクラス（仮）

```java
public class User {
    private Long id;
    private String name;

    // コンストラクタ・Getter省略
    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

---

### 2. `UserRepository`

```java
import org.springframework.stereotype.Repository;

@Repository
public class UserRepository {

    public User findById(Long id) {
        // 本来はDBアクセスするが、ここではダミーデータ
        return new User(id, "Hiroaki");
    }
}
```

---

### 3. `UserService`

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired  // 明示してもよい（省略可）
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public String getUserName(Long id) {
        User user = userRepository.findById(id);  // 委譲
        return user.getName();                    // 戻り値で返却
    }
}
```

---

### 4. `UserController`

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}/name")
    public String getUserName(@PathVariable Long id) {
        return userService.getUserName(id);  // Serviceに委譲し、戻り値を返す
    }
}
```

---

## ✅ まとめ：この例における設計パターン適用

| 要素 | 内容 |
| --- | --- |
| **DI** | 各クラスに`@Autowired`によって依存注入（コンストラクタベース） |
| **委譲** | Controller → Service → Repositoryのメソッド呼び出し |
| **戻り値** | Repositoryの戻り値をServiceが受け取り、Controllerに返却 |

---

ご希望あれば、これを**Spring Data JPA対応**や**非同期処理（`@Async` + `CompletableFuture`）対応**に拡張することも可能です。どうしますか？