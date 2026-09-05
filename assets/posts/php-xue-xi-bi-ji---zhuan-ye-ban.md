# PHP 学习笔记 - 专业版

## 1. PHP 基础语法与特性

### 1.1 PHP 标记与输出

```

```

### 1.2 变量与数据类型

```
 'John', 'age' => 25];
$null = null;

// 类型检测
is_string($string); // true
is_int($integer); // true
is_array($array); // true

// 类型转换
$number = (int) "123";
$text = (string) 456;
?>
```

### 1.3 运算符

```
 $b;
// $a  $b: 1

// 空合并运算符 (PHP 7+)
$username = $_GET['user'] ?? 'anonymous';

// 空合并赋值运算符 (PHP 7.4+)
$array['key'] ??= 'default';
?>
```

## 2. 函数编程

### 2.1 函数定义与使用

```

```

### 2.2 匿名函数与闭包

```
 $n ** 3;

// 闭包使用use传递变量
$multiplier = 2;
$double = function($n) use ($multiplier) {
    return $n * $multiplier;
};
?>
```

## 3. 面向对象编程

### 3.1 类与对象

```
name = $name;
        $this->email = $email;
        $this->age = $age;
    }
    
    // 方法
    public function getName(): string {
        return $this->name;
    }
    
    public function setEmail(string $email): void {
        if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
            $this->email = $email;
        }
    }
    
    // 静态方法
    public static function createAnonymous(): self {
        return new self('Anonymous', 'anon@example.com');
    }
    
    // 魔术方法
    public function __toString(): string {
        return "User: {$this->name} ({$this->email})";
    }
}

// 使用
$user = new User("John", "john@example.com", 25);
echo $user->getName();
?>
```

### 3.2 继承与多态

```
name = $name;
    }
    
    abstract public function getRole(): string;
}

class Student extends Person {
    private string $studentId;
    
    public function __construct(string $name, string $studentId) {
        parent::__construct($name);
        $this->studentId = $studentId;
    }
    
    public function getRole(): string {
        return "Student";
    }
    
    // final 方法，禁止子类重写
    final public function getStudentId(): string {
        return $this->studentId;
    }
}

// 接口
interface Loggable {
    public function log(string $message): void;
}

class Teacher extends Person implements Loggable {
    public function getRole(): string {
        return "Teacher";
    }
    
    public function log(string $message): void {
        echo "[LOG] $message\n";
    }
}
?>
```

### 3.3 特性 (Traits)

```
log("Application started");
        echo "Current time: " . $this->getTimestamp();
    }
}
?>
```

## 4. 错误与异常处理

### 4.1 异常处理

```
code}]: {$this->message}\n";
    }
}

class DataProcessor {
    /**
     * @throws CustomException
     */
    public function process(array $data): bool {
        if (empty($data)) {
            throw new CustomException("Data cannot be empty");
        }
        
        try {
            // 处理逻辑
            return true;
        } catch (InvalidArgumentException $e) {
            throw new CustomException("Processing failed", 0, $e);
        }
    }
}

// 使用异常处理
try {
    $processor = new DataProcessor();
    $processor->process([]);
} catch (CustomException $e) {
    error_log($e->getMessage());
    echo "Error: " . $e->getMessage();
} finally {
    echo "Processing completed\n";
}
?>
```

### 4.2 错误处理

```
getMessage());
    http_response_code(500);
    echo "An error occurred. Please try again later.";
});

// 关闭错误处理
restore_error_handler();
restore_exception_handler();
?>
```

## 5. 安全编程实践

### 5.1 输入验证与过滤

```
 $options]);
    }
}

// 使用
$email = InputValidator::validateEmail($_POST['email']);
$name = InputValidator::sanitizeString($_POST['name']);
?>
```

### 5.2 密码安全

```
 self::COST]);
    }
    
    public static function verify(string $password, string $hash): bool {
        return password_verify($password, $hash);
    }
    
    public static function needsRehash(string $hash): bool {
        return password_needs_rehash($hash, PASSWORD_BCRYPT, ['cost' => self::COST]);
    }
}
?>
```

## 6. 数据库操作 (PDO)

### 6.1 数据库连接与查询

```
 PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
            PDO::ATTR_EMULATE_PREPARES => false,
        ];
        
        try {
            $this->pdo = new PDO($dsn, $username, $password, $options);
        } catch (PDOException $e) {
            throw new PDOException($e->getMessage(), (int)$e->getCode());
        }
    }
    
    public function getUsers(): array {
        $stmt = $this->pdo->prepare("SELECT id, name, email FROM users WHERE active = :active");
        $stmt->execute(['active' => 1]);
        return $stmt->fetchAll();
    }
    
    public function createUser(array $userData): bool {
        $sql = "INSERT INTO users (name, email, password) VALUES (:name, :email, :password)";
        $stmt = $this->pdo->prepare($sql);
        
        return $stmt->execute([
            'name' => $userData['name'],
            'email' => $userData['email'],
            'password' => PasswordManager::hash($userData['password'])
        ]);
    }
}
?>
```

## 7. 现代 PHP 特性

### 7.1 PHP 8+ 新特性

```
 'success',
    400, 401, 403 => 'client_error',
    500, 502 => 'server_error',
    default => 'unknown'
};

// 构造器属性提升 (PHP 8.0)
class Order {
    public function __construct(
        private int $id,
        private DateTime $createdAt = new DateTime()
    ) {}
}

// 只读属性 (PHP 8.1)
readonly class UserProfile {
    public function __construct(
        public string $username,
        public DateTime $createdAt
    ) {}
}
?>
```

## 8. 性能优化与最佳实践

### 8.1 代码优化

```
prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$userId]);

// 批量插入
$values = [];
$placeholders = [];
$data = [];

foreach ($users as $index => $user) {
    $placeholders[] = "(?, ?, ?)";
    array_push($data, $user['name'], $user['email'], $user['age']);
}

$sql = "INSERT INTO users (name, email, age) VALUES " . implode(',', $placeholders);
$stmt = $pdo->prepare($sql);
$stmt->execute($data);
?>
```

### 8.2 缓存策略

```
redis = new Redis();
        $this->redis->connect('127.0.0.1', 6379);
    }
    
    public function remember(string $key, int $ttl, callable $callback) {
        $cached = $this->redis->get($key);
        
        if ($cached !== false) {
            return unserialize($cached);
        }
        
        $result = $callback();
        $this->redis->setex($key, $ttl, serialize($result));
        
        return $result;
    }
}
?>
```

## 9. 测试与调试

### 9.1 单元测试示例 (PHPUnit)

```
assertEquals("John", $user->getName());
        $this->assertEquals("john@example.com", $user->getEmail());
    }
    
    public function testInvalidEmail(): void {
        $this->expectException(InvalidArgumentException::class);
        new User("John", "invalid-email");
    }
}
?>
```

## 总结

这份学习笔记涵盖了 PHP 开发的核心概念，从基础语法到高级特性，包括：

- 
现代 PHP 语法和类型系统

- 
面向对象编程原则

- 
安全编程实践

- 
数据库交互

- 
错误和异常处理

- 
性能优化技巧

- 
测试方法

持续学习和实践这些概念将帮助您成为专业的 PHP 开发者。建议结合实际项目应用这些知识，并关注 PHP 官方文档以了解最新特性更新。
