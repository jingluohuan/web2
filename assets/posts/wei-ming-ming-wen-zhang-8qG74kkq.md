# JavaScript 全面深入学习教程

## 第一部分：JavaScript 基础

### 1.1 JavaScript 简介与历史

**JavaScript 的诞生与发展：**

- 
1995年：Brendan Eich 在 Netscape 公司开发

- 
1997年：ECMAScript 1.0 标准发布

- 
2009年：ES5 发布，带来严格模式等特性

- 
2015年：ES6/ES2015 发布，重大更新

- 
每年：ECMAScript 年度更新

**JavaScript 与 ECMAScript 关系：**

```
// ECMAScript 是标准，JavaScript 是实现
// 现代 JavaScript 包含：
// - ECMAScript (核心语法)
// - DOM (文档对象模型)
// - BOM (浏览器对象模型)
```

### 1.2 变量与数据类型

#### 变量声明

```
// var (函数作用域，存在变量提升)
var oldVariable = "传统方式";

// let (块级作用域，推荐使用)
let modernVariable = "现代方式";

// const (块级作用域，常量)
const PI = 3.14159;

// 变量命名规则
let userName;          // 驼峰命名
const MAX_SIZE = 100;  // 常量全大写
```

#### 数据类型

```
// 原始类型 (Primitive Types)
let str = "Hello";           // string
let num = 42;               // number
let bigInt = 9007199254740991n; // bigint
let bool = true;            // boolean
let nullVar = null;         // null
let undefinedVar = undefined; // undefined
let symbol = Symbol("id");  // symbol

// 引用类型 (Reference Types)
let obj = { name: "John" }; // object
let arr = [1, 2, 3];        // array
let func = function() {};   // function

// 类型检测
typeof "hello";        // "string"
typeof 42;            // "number"
typeof true;          // "boolean"
typeof undefined;     // "undefined"
typeof null;          // "object" (历史遗留问题)
typeof {};            // "object"
typeof [];            // "object"
typeof function(){};  // "function"

// 更准确的类型检测
Array.isArray([]);           // true
Object.prototype.toString.call([]); // "[object Array]"
```

### 1.3 类型转换与比较

#### 显式类型转换

```
// 转换为数字
Number("123");        // 123
parseInt("123px");    // 123
parseFloat("12.34");  // 12.34
+"123";               // 123

// 转换为字符串
String(123);          // "123"
(123).toString();     // "123"
123 + "";             // "123"

// 转换为布尔值
Boolean(1);           // true
Boolean(0);           // false
!!"hello";            // true

// 特殊转换规则
Number("");           // 0
Number("  ");         // 0
Number("123abc");     // NaN
Boolean("");          // false
Boolean(" ");         // true
```

#### 隐式类型转换

```
// 字符串连接
"2" + 2;              // "22"
2 + "2";              // "22"

// 数学运算
"10" - 5;             // 5
"10" * "2";           // 20
"10" / "2";           // 5

// 布尔上下文
if ("hello") {        // true
    console.log("执行");
}
```

#### 比较运算符

```
// 宽松相等 (==) - 会进行类型转换
"5" == 5;             // true
0 == false;           // true
"" == false;          // true
null == undefined;    // true

// 严格相等 (===) - 不会类型转换
"5" === 5;            // false
0 === false;          // false
null === undefined;   // false

// 特殊比较
NaN === NaN;          // false
0 === -0;             // true

// 最佳实践：总是使用 === 和 !==
```

### 1.4 运算符详解

```
// 算术运算符
let a = 10, b = 3;
a + b;    // 13
a - b;    // 7
a * b;    // 30
a / b;    // 3.333...
a % b;    // 1
a ** b;   // 1000 (指数)

// 赋值运算符
let x = 10;
x += 5;   // x = 15
x -= 3;   // x = 12
x *= 2;   // x = 24
x /= 4;   // x = 6

// 自增/自减
let i = 5;
i++;      // 后自增，返回5，然后i=6
++i;      // 前自增，i=7，返回7

// 逻辑运算符
true && false;    // false (AND)
true || false;    // true (OR)
!true;            // false (NOT)

// 短路求值
let result = value || "default";
let name = user && user.name;

// 空值合并运算符 (??)
let input = null;
let value = input ?? "default"; // "default"

// 可选链运算符 (?.)
let user = {};
let userName = user?.profile?.name; // undefined
```

## 第二部分：函数深入理解

### 2.1 函数定义与调用

```
// 函数声明 (会提升)
function sayHello(name) {
    return `Hello, ${name}!`;
}

// 函数表达式
const sayGoodbye = function(name) {
    return `Goodbye, ${name}!`;
};

// 箭头函数 (ES6+)
const multiply = (a, b) => a * b;

// 方法简写 (在对象中)
const calculator = {
    add(a, b) {
        return a + b;
    },
    multiply: (a, b) => a * b
};

// 立即调用函数表达式 (IIFE)
(function() {
    console.log("立即执行");
})();

// 生成器函数
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
}
```

### 2.2 参数处理

```
// 默认参数
function greet(name = "Guest", greeting = "Hello") {
    return `${greeting}, ${name}!`;
}

// 剩余参数
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

// 参数解构
function printUser({ name, age, email = "N/A" }) {
    console.log(`Name: ${name}, Age: ${age}, Email: ${email}`);
}

// arguments 对象 (传统函数)
function oldStyle() {
    console.log(arguments); // 类数组对象
    console.log(Array.from(arguments)); // 转换为数组
}
```

### 2.3 作用域与闭包

```
// 词法作用域
let globalVar = "global";

function outer() {
    let outerVar = "outer";
    
    function inner() {
        let innerVar = "inner";
        console.log(globalVar);  // 可以访问
        console.log(outerVar);   // 可以访问
        console.log(innerVar);   // 可以访问
    }
    
    inner();
    // console.log(innerVar);    // 错误：无法访问
}

// 闭包
function createCounter() {
    let count = 0;
    
    return {
        increment() {
            count++;
            return count;
        },
        decrement() {
            count--;
            return count;
        },
        getCount() {
            return count;
        }
    };
}

const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2

// 模块模式
const MyModule = (function() {
    let privateVar = "secret";
    
    function privateMethod() {
        return privateVar;
    }
    
    return {
        publicMethod() {
            return privateMethod();
        }
    };
})();
```

### 2.4 this 关键字

```
// this 的绑定规则
const obj = {
    name: "MyObject",
    
    // 方法调用 - this 指向调用对象
    regularMethod() {
        console.log(this.name);
    },
    
    // 箭头函数 - this 继承自外层
    arrowMethod: () => {
        console.log(this); // 指向外层 this
    }
};

// 构造函数中的 this
function Person(name) {
    this.name = name;
    this.sayHello = function() {
        console.log(`Hello, I'm ${this.name}`);
    };
}

// 显式绑定
function introduce(greeting) {
    console.log(`${greeting}, I'm ${this.name}`);
}

const person = { name: "Alice" };
introduce.call(person, "Hello");     // 立即调用
introduce.apply(person, ["Hi"]);     // 数组参数
const bound = introduce.bind(person); // 返回新函数
```

## 第三部分：面向对象编程

### 3.1 构造函数与原型

```
// 构造函数
function Animal(name, type) {
    this.name = name;
    this.type = type;
    this.isAlive = true;
}

// 原型方法
Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

Animal.prototype.feed = function() {
    return `${this.name} is eating`;
};

// 创建实例
const dog = new Animal("Rex", "Dog");
console.log(dog.speak()); // "Rex makes a sound"

// 原型链
console.log(dog.__proto__ === Animal.prototype); // true
console.log(Animal.prototype.__proto__ === Object.prototype); // true
```

### 3.2 ES6 类语法

```
class Animal {
    // 构造函数
    constructor(name, type) {
        this.name = name;
        this.type = type;
        this.isAlive = true;
    }
    
    // 实例方法
    speak() {
        return `${this.name} makes a sound`;
    }
    
    feed() {
        return `${this.name} is eating`;
    }
    
    // 静态方法
    static isAnimal(obj) {
        return obj instanceof Animal;
    }
    
    // Getter/Setter
    get status() {
        return this.isAlive ? "alive" : "not alive";
    }
    
    set status(value) {
        if (value === "alive") {
            this.isAlive = true;
        } else {
            this.isAlive = false;
        }
    }
}

// 继承
class Dog extends Animal {
    constructor(name, breed) {
        super(name, "Dog"); // 调用父类构造函数
        this.breed = breed;
    }
    
    // 方法重写
    speak() {
        return `${this.name} barks!`;
    }
    
    // 新方法
    fetch() {
        return `${this.name} is fetching the ball`;
    }
}
```

### 3.3 高级面向对象概念

```
// 私有字段 (ES2022)
class BankAccount {
    #balance = 0; // 私有字段
    
    constructor(initialBalance) {
        this.#balance = initialBalance;
    }
    
    deposit(amount) {
        if (amount > 0) {
            this.#balance += amount;
        }
    }
    
    getBalance() {
        return this.#balance;
    }
}

// 混入模式 (Mixin)
const CanSwim = {
    swim() {
        return `${this.name} is swimming`;
    }
};

const CanFly = {
    fly() {
        return `${this.name} is flying`;
    }
};

class Duck {
    constructor(name) {
        this.name = name;
    }
}

// 应用混入
Object.assign(Duck.prototype, CanSwim, CanFly);

// 组合优于继承
class AnimalBehavior {
    constructor(name) {
        this.name = name;
    }
    
    eat() {
        return `${this.name} is eating`;
    }
}

class SwimmingBehavior {
    swim() {
        return `${this.name} is swimming`;
    }
}

class Fish {
    constructor(name) {
        this.animal = new AnimalBehavior(name);
        this.swimmer = new SwimmingBehavior();
    }
    
    eat() {
        return this.animal.eat();
    }
    
    swim() {
        return this.swimmer.swim();
    }
}
```

## 第四部分：异步编程

### 4.1 回调函数与 Promise

```
// 回调地狱示例
function oldStyleAsync(callback) {
    setTimeout(() => {
        console.log("第一步完成");
        setTimeout(() => {
            console.log("第二步完成");
            setTimeout(() => {
                console.log("第三步完成");
                callback("完成");
            }, 1000);
        }, 1000);
    }, 1000);
}

// Promise 基础
const promise = new Promise((resolve, reject) => {
    // 异步操作
    setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
            resolve("操作成功");
        } else {
            reject(new Error("操作失败"));
        }
    }, 1000);
});

// Promise 使用
promise
    .then(result => {
        console.log("成功:", result);
        return "下一步数据";
    })
    .then(nextResult => {
        console.log("链式调用:", nextResult);
    })
    .catch(error => {
        console.error("错误:", error);
    })
    .finally(() => {
        console.log("无论如何都会执行");
    });

// Promise 静态方法
Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log("全部完成:", results);
    })
    .catch(error => {
        console.error("有一个失败:", error);
    });

Promise.race([promise1, promise2])
    .then(firstResult => {
        console.log("第一个完成的:", firstResult);
    });

Promise.allSettled([promise1, promise2])
    .then(results => {
        results.forEach(result => {
            if (result.status === "fulfilled") {
                console.log("成功:", result.value);
            } else {
                console.log("失败:", result.reason);
            }
        });
    });
```

### 4.2 Async/Await

```
// Async 函数总是返回 Promise
async function fetchData() {
    try {
        // 模拟异步操作
        const response = await new Promise((resolve) => {
            setTimeout(() => resolve("数据获取成功"), 1000);
        });
        
        const processed = await processData(response);
        return processed;
    } catch (error) {
        console.error("获取数据失败:", error);
        throw error;
    }
}

// 并行执行
async function parallelExecution() {
    try {
        const [user, posts, settings] = await Promise.all([
            fetchUser(),
            fetchPosts(),
            fetchSettings()
        ]);
        
        return { user, posts, settings };
    } catch (error) {
        console.error("并行执行失败:", error);
    }
}

// 错误处理模式
async function robustFunction() {
    // 方法1: try-catch
    try {
        return await riskyOperation();
    } catch (error) {
        return fallbackValue;
    }
    
    // 方法2: .catch()
    const result = await riskyOperation().catch(() => fallbackValue);
    
    // 方法3: 包装函数
    const [error, data] = await asyncWrapper(riskyOperation());
    if (error) {
        return fallbackValue;
    }
    return data;
}

// 辅助函数
function asyncWrapper(promise) {
    return promise
        .then(data => [null, data])
        .catch(error => [error, null]);
}
```

### 4.3 高级异步模式

```
// 异步生成器
async function* asyncGenerator() {
    let i = 0;
    while (i  setTimeout(resolve, 1000));
        yield i++;
    }
}

// 使用异步生成器
(async () => {
    for await (const value of asyncGenerator()) {
        console.log(value);
    }
})();

// 取消异步操作
function createCancellableAsync() {
    let cancel = null;
    
    const promise = new Promise((resolve, reject) => {
        const timeoutId = setTimeout(() => {
            resolve("操作完成");
        }, 5000);
        
        cancel = () => {
            clearTimeout(timeoutId);
            reject(new Error("操作被取消"));
        };
    });
    
    return { promise, cancel };
}

// 重试机制
async function retryOperation(operation, maxRetries = 3, delay = 1000) {
    for (let attempt = 1; attempt  setTimeout(resolve, delay));
            delay *= 2; // 指数退避
        }
    }
}
```

## 第五部分：数组与对象操作

### 5.1 数组方法大全

```
const numbers = [1, 2, 3, 4, 5];
const users = [
    { id: 1, name: "Alice", age: 25 },
    { id: 2, name: "Bob", age: 30 },
    { id: 3, name: "Charlie", age: 35 }
];

// 遍历方法
numbers.forEach((num, index) => {
    console.log(`索引 ${index}: ${num}`);
});

// 映射
const doubled = numbers.map(num => num * 2); // [2, 4, 6, 8, 10]
const names = users.map(user => user.name); // ["Alice", "Bob", "Charlie"]

// 过滤
const even = numbers.filter(num => num % 2 === 0); // [2, 4]
const youngUsers = users.filter(user => user.age  num % 2 === 0); // 2
const user = users.find(user => user.id === 2); // { id: 2, ... }
const userIndex = users.findIndex(user => user.id === 2); // 1

// 归约
const sum = numbers.reduce((total, num) => total + num, 0); // 15
const ageSum = users.reduce((total, user) => total + user.age, 0); // 90

// 排序
const sortedNumbers = [...numbers].sort((a, b) => a - b); // 升序
const sortedUsers = [...users].sort((a, b) => a.age - b.age); // 按年龄排序

// 其他实用方法
numbers.some(num => num > 3);    // true (至少一个满足)
numbers.every(num => num > 0);   // true (全部满足)
numbers.includes(3);             // true
numbers.flat();                  // 扁平化数组
numbers.flatMap(num => [num, num * 2]); // 映射后扁平化
```

### 5.2 对象操作

```
// 对象创建
const obj1 = { a: 1, b: 2 };
const obj2 = new Object();
const obj3 = Object.create(null);

// 属性操作
const person = {
    name: "John",
    age: 30
};

// 访问属性
person.name;                    // "John"
person["age"];                  // 30

// 动态属性名
const key = "name";
person[key];                    // "John"

// 属性描述符
Object.getOwnPropertyDescriptor(person, "name");
Object.defineProperty(person, "email", {
    value: "john@example.com",
    writable: false,
    enumerable: true,
    configurable: true
});

// 对象方法
Object.keys(person);            // ["name", "age"]
Object.values(person);          // ["John", 30]
Object.entries(person);         // [["name", "John"], ["age", 30]]

// 对象合并
const defaults = { theme: "light", language: "en" };
const userSettings = { theme: "dark" };
const settings = { ...defaults, ...userSettings }; // { theme: "dark", language: "en" }

// 对象冻结
Object.freeze(person);          // 完全冻结，不能修改
Object.seal(person);            // 可修改属性值，但不能添加/删除属性
```

### 5.3 解构赋值

```
// 数组解构
const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest] = numbers;
// first = 1, second = 2, rest = [3, 4, 5]

// 对象解构
const user = {
    id: 1,
    name: "Alice",
    age: 25,
    address: {
        city: "New York",
        country: "USA"
    }
};

const { name, age, address: { city } } = user;
// name = "Alice", age = 25, city = "New York"

// 重命名和默认值
const { name: userName, email = "default@example.com" } = user;

// 函数参数解构
function printUser({ name, age, email = "N/A" }) {
    console.log(`Name: ${name}, Age: ${age}, Email: ${email}`);
}

// 交换变量
let a = 1, b = 2;
[a, b] = [b, a]; // a = 2, b = 1
```

## 第六部分：现代 JavaScript 特性

### 6.1 ES6+ 新特性

```
// 模板字符串
const name = "Alice";
const message = `Hello, ${name}!
Welcome to our application.
Today is ${new Date().toLocaleDateString()}.`;

// 标签模板
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + (values[i] ? `${values[i]}` : '');
    }, '');
}

const highlighted = highlight`Hello, ${name}!`;

// 符号 (Symbol)
const uniqueKey = Symbol("description");
const obj = {
    [uniqueKey]: "secret value",
    regularKey: "normal value"
};

// 全局符号
Symbol.for("sharedKey"); // 创建或获取全局符号

// 迭代器和可迭代对象
const iterableObject = {
    [Symbol.iterator]() {
        let step = 0;
        return {
            next() {
                step++;
                if (step 

### 6.2 模块系统

```
// math.js - 命名导出
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

// 默认导出
export default class Calculator {
    static square(x) {
        return x * x;
    }
}

// app.js - 导入
import Calculator, { PI, add, multiply } from './math.js';
import * as MathUtils from './math.js'; // 命名空间导入

// 动态导入
async function loadModule() {
    const module = await import('./math.js');
    console.log(module.PI);
}
```

### 6.3 代理和反射

```
// 代理 (Proxy)
const target = {
    message: "hello",
    number: 42
};

const handler = {
    get(obj, prop) {
        if (prop in obj) {
            return obj[prop];
        }
        return `属性 ${prop} 不存在`;
    },
    
    set(obj, prop, value) {
        if (prop === "age" && typeof value !== "number") {
            throw new Error("年龄必须是数字");
        }
        obj[prop] = value;
        return true; // 表示成功
    },
    
    has(obj, prop) {
        return prop in obj;
    }
};

const proxy = new Proxy(target, handler);

// 反射 (Reflect)
const obj = { a: 1 };
Reflect.set(obj, 'b', 2); // obj.b = 2
Reflect.has(obj, 'a');    // true
Reflect.ownKeys(obj);     // ['a', 'b']
```

## 第七部分：错误处理与调试

### 7.1 错误处理策略

```
// 自定义错误类型
class ValidationError extends Error {
    constructor(field, message) {
        super(`字段 "${field}" 验证失败: ${message}`);
        this.name = "ValidationError";
        this.field = field;
        this.timestamp = new Date();
    }
}

class NetworkError extends Error {
    constructor(url, status) {
        super(`请求 ${url} 失败，状态码: ${status}`);
        this.name = "NetworkError";
        this.url = url;
        this.status = status;
    }
}

// 错误处理函数
function handleError(error) {
    if (error instanceof ValidationError) {
        console.warn(`验证错误: ${error.message}`);
        // 显示给用户的友好消息
        showUserMessage(`请输入有效的 ${error.field}`);
    } else if (error instanceof NetworkError) {
        console.error(`网络错误: ${error.message}`);
        // 重试逻辑或显示离线模式
        if (error.status === 401) {
            redirectToLogin();
        }
    } else if (error instanceof SyntaxError) {
        console.error(`语法错误: ${error.message}`);
    } else {
        console.error(`未知错误: ${error.message}`);
        // 发送到错误监控服务
        sendToErrorMonitoring(error);
    }
    
    // 记录错误详情
    logErrorDetails(error);
}

// 错误边界 (React 概念，但在纯 JS 中也可模拟)
function errorBoundary(componentFunction) {
    try {
        return componentFunction();
    } catch (error) {
        handleError(error);
        return fallbackUI();
    }
}

// 全局错误处理
window.addEventListener('error', (event) => {
    handleError(event.error);
    // 阻止默认错误处理
    event.preventDefault();
});

window.addEventListener('unhandledrejection', (event) => {
    handleError(event.reason);
    event.preventDefault();
});
```

### 7.2 调试技巧

```
// 条件调试
function complexCalculation(data) {
    // 只在开发环境显示调试信息
    if (process.env.NODE_ENV === 'development') {
        console.group('复杂计算调试');
        console.log('输入数据:', data);
        console.trace('调用栈');
    }
    
    try {
        // 计算逻辑
        const result = data.process();
        
        if (process.env.NODE_ENV === 'development') {
            console.log('计算结果:', result);
            console.groupEnd();
        }
        
        return result;
    } catch (error) {
        if (process.env.NODE_ENV === 'development') {
            console.error('计算错误:', error);
            console.groupEnd();
        }
        throw error;
    }
}

// 性能调试
function measurePerformance(operation, label = 'Operation') {
    const startTime = performance.now();
    
    const result = operation();
    
    const endTime = performance.now();
    console.log(`${label} 耗时: ${(endTime - startTime).toFixed(2)}ms`);
    
    return result;
}

// 内存使用监控
function checkMemoryUsage() {
    if (performance.memory) {
        const used = performance.memory.usedJSHeapSize;
        const limit = performance.memory.jsHeapSizeLimit;
        console.log(`内存使用: ${(used / 1048576).toFixed(2)} MB / ${(limit / 1048576).toFixed(2)} MB`);
    }
}
```

## 第八部分：性能优化

### 8.1 代码级优化

```
// 防抖和节流
function debounce(func, wait, immediate = false) {
    let timeout;
    
    return function executedFunction(...args) {
        const later = () => {
            timeout = null;
            if (!immediate) func(...args);
        };
        
        const callNow = immediate && !timeout;
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
        
        if (callNow) func(...args);
    };
}

function throttle(func, limit) {
    let inThrottle;
    
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// 使用示例
const handleResize = debounce(() => {
    console.log('窗口大小改变');
}, 250);

window.addEventListener('resize', handleResize);

// 记忆化 (Memoization)
function memoize(fn) {
    const cache = new Map();
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

// 使用记忆化
const expensiveCalculation = memoize(function(n) {
    console.log('执行复杂计算...');
    let result = 0;
    for (let i = 0; i 

### 8.2 内存管理

```
// 避免内存泄漏
class EventManager {
    constructor() {
        this.handlers = new Map();
        this.weakHandlers = new WeakMap();
    }
    
    // 使用 WeakMap 避免内存泄漏
    addWeakListener(element, event, handler) {
        if (!this.weakHandlers.has(element)) {
            this.weakHandlers.set(element, new Map());
        }
        
        const elementHandlers = this.weakHandlers.get(element);
        elementHandlers.set(event, handler);
        
        element.addEventListener(event, handler);
    }
    
    // 清理资源
    dispose() {
        // 清理普通事件监听器
        for (const [element, events] of this.handlers) {
            for (const [event, handler] of events) {
                element.removeEventListener(event, handler);
            }
        }
        this.handlers.clear();
        
        // WeakMap 不需要手动清理
    }
}

// 对象池模式
class ObjectPool {
    constructor(createFn, resetFn = (obj) => obj) {
        this.createFn = createFn;
        this.resetFn = resetFn;
        this.free = [];
        this.used = [];
    }
    
    acquire() {
        let object;
        
        if (this.free.length > 0) {
            object = this.free.pop();
        } else {
            object = this.createFn();
        }
        
        this.used.push(object);
        return object;
    }
    
    release(object) {
        const index = this.used.indexOf(object);
        if (index !== -1) {
            this.used.splice(index, 1);
            this.resetFn(object);
            this.free.push(object);
        }
    }
}

// 使用对象池
const vectorPool = new ObjectPool(
    () => ({ x: 0, y: 0, z: 0 }),
    (vec) => { vec.x = 0; vec.y = 0; vec.z = 0; }
);
```

## 第九部分：测试与质量保证

### 9.1 单元测试

```
// 简单的测试框架
class TestFramework {
    constructor() {
        this.tests = [];
        this.passed = 0;
        this.failed = 0;
    }
    
    test(name, fn) {
        this.tests.push({ name, fn });
    }
    
    expect(value) {
        return {
            toBe(expected) {
                if (value !== expected) {
                    throw new Error(`期望 ${expected}，但得到 ${value}`);
                }
            },
            toEqual(expected) {
                if (JSON.stringify(value) !== JSON.stringify(expected)) {
                    throw new Error(`期望 ${JSON.stringify(expected)}，但得到 ${JSON.stringify(value)}`);
                }
            },
            toThrow() {
                let threw = false;
                try {
                    value();
                } catch {
                    threw = true;
                }
                if (!threw) {
                    throw new Error('期望函数抛出错误，但没有抛出');
                }
            }
        };
    }
    
    run() {
        console.log('运行测试...\n');
        
        for (const test of this.tests) {
            try {
                test.fn();
                console.log(`✅ ${test.name}`);
                this.passed++;
            } catch (error) {
                console.log(`❌ ${test.name}`);
                console.log(`   错误: ${error.message}`);
                this.failed++;
            }
        }
        
        console.log(`\n结果: ${this.passed} 通过，${this.failed} 失败`);
    }
}

// 使用测试框架
const test = new TestFramework();

test('加法测试', () => {
    test.expect(1 + 1).toBe(2);
});

test('数组测试', () => {
    test.expect([1, 2, 3]).toEqual([1, 2, 3]);
});

test.run();
```

### 9.2 代码质量工具

```
// JSDoc 文档注释
/**
 * 计算两个数字的和
 * @param {number} a - 第一个数字
 * @param {number} b - 第二个数字
 * @returns {number} 两个数字的和
 * @throws {TypeError} 当参数不是数字时
 * @example
 * // 返回 3
 * add(1, 2);
 */
function add(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new TypeError('参数必须是数字');
    }
    return a + b;
}

// 类型检查 (使用 JSDoc 类型)
/**
 * @typedef {Object} User
 * @property {string} name - 用户名
 * @property {number} age - 年龄
 * @property {string} [email] - 邮箱（可选）
 */

/**
 * 创建用户问候语
 * @param {User} user - 用户对象
 * @returns {string} 问候语
 */
function createGreeting(user) {
    return `Hello, ${user.name}!`;
}

// 配置 ESLint 规则示例 (配置文件)
/*
module.exports = {
    env: {
        browser: true,
        es2021: true
    },
    extends: [
        'eslint:recommended'
    ],
    parserOptions: {
        ecmaVersion: 12,
        sourceType: 'module'
    },
    rules: {
        'no-unused-vars': 'warn',
        'no-console': 'off',
        'prefer-const': 'error'
    }
};
*/
```

## 第十部分：实际项目应用

### 10.1 项目结构组织

```
// utils/helpers.js - 工具函数
export function formatDate(date, format = 'YYYY-MM-DD') {
    const d = new Date(date);
    const year = d.getFullYear();
    const month = String(d.getMonth() + 1).padStart(2, '0');
    const day = String(d.getDate()).padStart(2, '0');
    
    return format
        .replace('YYYY', year)
        .replace('MM', month)
        .replace('DD', day);
}

export function generateId() {
    return Date.now().toString(36) + Math.random().toString(36).substr(2);
}

export function deepClone(obj) {
    return JSON.parse(JSON.stringify(obj));
}

// services/api.js - API 服务层
class ApiService {
    constructor(baseURL) {
        this.baseURL = baseURL;
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseURL}${endpoint}`;
        const config = {
            headers: {
                'Content-Type': 'application/json',
                ...options.headers
            },
            ...options
        };
        
        try {
            const response = await fetch(url, config);
            
            if (!response.ok) {
                throw new Error(`HTTP错误! 状态: ${response.status}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error('API请求失败:', error);
            throw error;
        }
    }
    
    get(endpoint) {
        return this.request(endpoint);
    }
    
    post(endpoint, data) {
        return this.request(endpoint, {
            method: 'POST',
            body: JSON.stringify(data)
        });
    }
}

// store/state-management.js - 状态管理
class Store {
    constructor(reducer, initialState = {}) {
        this.state = initialState;
        this.reducer = reducer;
        this.listeners = [];
        this.dispatch({ type: '@@INIT' });
    }
    
    getState() {
        return this.state;
    }
    
    dispatch(action) {
        this.state = this.reducer(this.state, action);
        this.listeners.forEach(listener => listener());
    }
    
    subscribe(listener) {
        this.listeners.push(listener);
        return () => {
            this.listeners = this.listeners.filter(l => l !== listener);
        };
    }
}

// 使用示例
function counterReducer(state = { count: 0 }, action) {
    switch (action.type) {
        case 'INCREMENT':
            return { ...state, count: state.count + 1 };
        case 'DECREMENT':
            return { ...state, count: state.count - 1 };
        default:
            return state;
    }
}

const store = new Store(counterReducer);
```

这个完整的 JavaScript 教程涵盖了从基础到高级的所有重要概念。建议按照以下步骤学习：

- 
**初级阶段**：掌握 1-3 部分的基础知识

- 
**中级阶段**：深入学习 4-6 部分的异步编程和现代特性

- 
**高级阶段**：掌握 7-10 部分的性能优化和项目架构

每个概念都建议配合实际编码练习，以加深理解。
