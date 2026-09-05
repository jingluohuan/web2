# Python 基础教程

## 1. Python 基础语法

### 1.1 变量和数据类型

```
# 基本数据类型
name = "Alice"          # 字符串类型
age = 25               # 整数类型  
height = 1.75          # 浮点数类型
is_student = True      # 布尔值类型

# 类型转换
str_age = str(age)     # 整数转换为字符串
int_height = int(height) # 浮点数转换为整数（向下取整）
float_age = float(age) # 整数转换为浮点数
bool_name = bool(name) # 字符串转换为布尔值（非空字符串为True）
```

### 1.2 输入输出

```
# 输入函数
name = input("请输入你的名字：")  # 获取用户输入，返回字符串
age = int(input("请输入你的年龄："))  # 将输入转换为整数

# 输出函数
print("你好，", name)  # 打印多个值，默认用空格分隔
print(f"你今年{age}岁")  # f-string格式化字符串，推荐用法
print("姓名：{}，年龄：{}".format(name, age))  # format方法格式化
print("姓名：%s，年龄：%d" % (name, age))  # %格式化，旧式写法
```

### 1.3 运算符

```
# 算术运算符
a = 10
b = 3
print(a + b)  # 加法，输出13
print(a - b)  # 减法，输出7  
print(a * b)  # 乘法，输出30
print(a / b)  # 除法，输出3.333...
print(a // b) # 整除，输出3
print(a % b)  # 取余，输出1
print(a ** b) # 幂运算，输出1000

# 比较运算符
print(a == b)  # 等于，输出False
print(a != b)  # 不等于，输出True
print(a > b)   # 大于，输出True
print(a = b)  # 大于等于，输出True
print(a 

## 2. 数据结构

### 2.1 列表 (List)

```
# 创建列表
fruits = ["apple", "banana", "orange"]  # 字符串列表
numbers = [1, 2, 3, 4, 5]  # 整数列表
mixed = [1, "hello", 3.14, True]  # 混合类型列表

# 列表操作
fruits.append("grape")      # 在末尾添加元素
fruits.insert(1, "pear")    # 在索引1位置插入元素
fruits.remove("banana")     # 删除第一个匹配的元素
last_fruit = fruits.pop()   # 删除并返回最后一个元素
second_fruit = fruits.pop(1) # 删除并返回索引1的元素

# 列表切片
print(fruits[1:3])     # 切片，索引1到3（不包括3）
print(fruits[:2])      # 从开始到索引2（不包括2）
print(fruits[2:])      # 从索引2到结束
print(fruits[-1])      # 最后一个元素
print(fruits[::2])     # 步长为2，每隔一个取一个

# 列表推导式
squares = [x**2 for x in range(10)]  # 生成0-9的平方列表
even_squares = [x**2 for x in range(10) if x % 2 == 0]  # 带条件的列表推导式
```

### 2.2 字典 (Dictionary)

```
# 创建字典
student = {
    "name": "Alice",
    "age": 20,
    "major": "Computer Science"
}  # 键值对集合，键必须是不可变类型

# 字典操作
print(student["name"])           # 通过键访问值
student["grade"] = "A"          # 添加新的键值对
student["age"] = 21             # 修改已有的键值对
del student["major"]            # 删除键值对
value = student.get("name")     # 安全获取值，键不存在返回None
value = student.get("phone", "N/A")  # 键不存在返回默认值

# 遍历字典
for key in student:             # 遍历所有键
    print(key)
    
for value in student.values():  # 遍历所有值
    print(value)
    
for key, value in student.items():  # 遍历所有键值对
    print(f"{key}: {value}")
```

### 2.3 元组 (Tuple) 和集合 (Set)

```
# 元组（不可变序列）
coordinates = (10, 20)  # 创建元组
x, y = coordinates      # 元组解包
single_tuple = (42,)    # 单元素元组，注意逗号不能省略
empty_tuple = ()        # 空元组

# 集合（无序、不重复元素）
set1 = {1, 2, 3, 4}    # 创建集合
set2 = {3, 4, 5, 6}    # 另一个集合
print(set1 | set2)      # 并集，输出{1,2,3,4,5,6}
print(set1 & set2)      # 交集，输出{3,4}
print(set1 - set2)      # 差集，输出{1,2}
print(set1 ^ set2)      # 对称差集，输出{1,2,5,6}
```

## 3. 控制流

### 3.1 条件语句

```
# if-elif-else 结构
score = 85

if score >= 90:          # 如果条件成立
    grade = "A"          # 执行这个代码块
elif score >= 80:        # 否则如果这个条件成立
    grade = "B"          # 执行这个代码块
elif score >= 70:        # 否则如果这个条件成立
    grade = "C"          # 执行这个代码块
else:                    # 以上条件都不成立
    grade = "D"          # 执行这个代码块

print(f"成绩等级：{grade}")  # 输出：成绩等级：B

# 嵌套if语句
if score >= 60:
    if score >= 90:
        print("优秀")
    else:
        print("及格")
else:
    print("不及格")
```

### 3.2 循环

```
# for 循环
for i in range(5):        # 循环5次，i从0到4
    print(i)              # 打印0,1,2,3,4

for i in range(2, 10, 2): # 从2开始，到10结束（不包括10），步长为2
    print(i)              # 打印2,4,6,8

# 遍历列表
fruits = ["apple", "banana", "orange"]
for fruit in fruits:      # 遍历列表中的每个元素
    print(fruit)          # 依次打印每个水果

# while 循环
count = 0
while count 

## 4. 函数

### 4.1 函数定义

```
def greet(name, greeting="Hello"):
    """简单的问候函数
    参数:
        name: 姓名
        greeting: 问候语，默认为'Hello'
    返回:
        问候字符串
    """
    return f"{greeting}, {name}!"  # 返回格式化字符串

# 调用函数
print(greet("Alice"))              # 使用默认参数
print(greet("Bob", "Hi"))          # 提供所有参数

# 返回多个值
def calculate(a, b):
    return a + b, a - b, a * b  # 返回元组，可以解包

sum_result, diff_result, product_result = calculate(10, 5)  # 解包返回值
print(f"和：{sum_result}, 差：{diff_result}, 积：{product_result}")
```

### 4.2 参数类型

```
# 位置参数
def func1(a, b, c):      # a, b, c是位置参数
    return a + b + c     # 必须按顺序传递

# 关键字参数
def func2(name, age, city):
    return f"{name} from {city}, age {age}"
    
result = func2(name="Alice", age=25, city="Beijing")  # 使用参数名传递

# 默认参数
def func3(message, times=1):  # times有默认值
    return message * times    # 可以只传一个参数

# 可变位置参数
def func4(*args):             # *args接收任意数量的位置参数
    return sum(args)          # args是元组

# 可变关键字参数  
def func5(**kwargs):          # **kwargs接收任意数量的关键字参数
    for key, value in kwargs.items():  # kwargs是字典
        print(f"{key}: {value}")
```

## 5. 文件操作

```
# 写入文件
with open("example.txt", "w", encoding="utf-8") as f:  # 以写入模式打开文件
    f.write("Hello, World!\n")    # 写入内容，\n表示换行
    f.write("这是第二行\n")        # 继续写入

# 读取文件
with open("example.txt", "r", encoding="utf-8") as f:  # 以读取模式打开文件
    content = f.read()           # 读取整个文件内容
    print(content)

# 逐行读取
with open("example.txt", "r", encoding="utf-8") as f:
    for line in f:               # 逐行迭代
        print(line.strip())      # strip()去除首尾空白字符

# 读取所有行到列表
with open("example.txt", "r", encoding="utf-8") as f:
    lines = f.readlines()        # 返回包含所有行的列表
    for line in lines:
        print(line.strip())
```

## 6. 异常处理

```
try:
    num = int(input("请输入一个数字："))  # 可能引发ValueError
    result = 10 / num                   # 可能引发ZeroDivisionError
    print(f"结果是：{result}")
except ValueError:                      # 捕获数值转换错误
    print("输入的不是有效数字！")
except ZeroDivisionError:               # 捕获除零错误
    print("不能除以零！")
except Exception as e:                  # 捕获所有其他异常
    print(f"发生未知错误：{e}")
else:                                   # 没有异常时执行
    print("计算成功！")
finally:                                # 无论是否异常都会执行
    print("程序执行完毕")

# 手动抛出异常
def check_age(age):
    if age  150:
        raise ValueError("年龄必须在0-150之间")  # 主动抛出异常
    return age
```

## 7. 面向对象编程

### 7.1 类和对象

```
class Student:
    # 类属性（所有实例共享）
    school = "ABC University"
    
    def __init__(self, name, age, major):
        # 实例属性（每个实例独有）
        self.name = name      # 学生姓名
        self.age = age        # 学生年龄
        self.major = major    # 学生专业
    
    # 实例方法
    def introduce(self):      # self指向实例本身
        return f"我叫{self.name}，学习{self.major}专业"
    
    # 类方法
    @classmethod              # 装饰器，表示类方法
    def get_school_info(cls): # cls指向类本身
        return f"学校：{cls.school}"
    
    # 静态方法
    @staticmethod             # 装饰器，表示静态方法
    def is_adult(age):        # 不需要self或cls参数
        return age >= 18

# 创建对象
student1 = Student("Alice", 20, "计算机科学")  # 创建实例
print(student1.introduce())                    # 调用实例方法
print(Student.get_school_info())               # 调用类方法
print(Student.is_adult(20))                    # 调用静态方法
```

### 7.2 继承

```
class Person:                            # 父类/基类
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def speak(self):
        return f"我是{self.name}"

class Teacher(Person):                   # 子类/派生类，继承Person
    def __init__(self, name, age, subject):
        super().__init__(name, age)      # 调用父类的构造方法
        self.subject = subject           # 子类特有的属性
    
    def teach(self):                     # 子类特有的方法
        return f"我教{self.subject}"
    
    def speak(self):                     # 重写父类方法
        return f"我是老师{self.name}"

# 使用子类
teacher = Teacher("张老师", 35, "数学")
print(teacher.speak())    # 调用重写后的方法
print(teacher.teach())    # 调用子类特有方法

```

## 8. 常用模块

### 8.1 内置模块

```
import math                # 数学模块
import random              # 随机数模块  
import datetime            # 日期时间模块
import os                  # 操作系统接口模块

# math 模块
print(math.sqrt(16))       # 平方根，输出4.0
print(math.pi)             # 圆周率π
print(math.ceil(4.2))      # 向上取整，输出5
print(math.floor(4.8))     # 向下取整，输出4

# random 模块
print(random.randint(1, 10))     # 1-10的随机整数
print(random.choice(["a", "b", "c"]))  # 随机选择列表中的元素
print(random.random())            # 0-1的随机浮点数
random.shuffle([1,2,3,4,5])      # 随机打乱列表

# datetime 模块
now = datetime.datetime.now()           # 当前时间
print(now.strftime("%Y-%m-%d %H:%M:%S"))  # 格式化时间
today = datetime.date.today()           # 今天日期
one_week_later = today + datetime.timedelta(days=7)  # 一周后的日期
```

### 8.2 第三方模块

```
# 需要先安装：pip install requests
import requests

# 发送HTTP GET请求
response = requests.get("https://api.github.com")
print(response.status_code)    # 状态码，200表示成功
print(response.json())         # 解析JSON响应

# 发送带参数的请求
params = {"q": "python", "page": 1}
response = requests.get("https://api.github.com/search/repositories", params=params)
```

## 9. 实用技巧

### 9.1 列表排序和查找

```
numbers = [3, 1, 4, 1, 5, 9, 2]

# 排序
sorted_numbers = sorted(numbers)  # 返回新列表，原列表不变
numbers.sort()                   # 原地排序，修改原列表
numbers.sort(reverse=True)       # 降序排序

# 查找
if 5 in numbers:                 # 检查元素是否存在
    print("找到5")

index = numbers.index(4)         # 返回元素的第一个索引
count = numbers.count(1)         # 统计元素出现次数

# 复杂排序
students = [("Alice", 20), ("Bob", 18), ("Charlie", 22)]
students.sort(key=lambda x: x[1])  # 按年龄排序
```

### 9.2 字符串操作

```
text = " Hello, World! "

# 常用字符串方法
print(text.strip())           # 去除两端空白字符
print(text.lower())           # 转为小写
print(text.upper())           # 转为大写
print(text.replace("World", "Python"))  # 替换子串
print(text.split(","))        # 按逗号分割，返回列表
print(text.startswith(" Hello"))  # 检查是否以指定字符串开头
print(text.endswith("! "))        # 检查是否以指定字符串结尾
print(text.find("World"))         # 查找子串位置，返回索引

# 字符串连接
words = ["Hello", "World", "Python"]
result = " ".join(words)      # 用空格连接列表中的字符串
print(result)                 # 输出：Hello World Python
```

## 10. 学习建议

- 
**多练习**：编程是实践技能，多写代码才能熟练掌握

- 
**阅读文档**：学会查阅Python官方文档和库文档

- 
**调试技巧**：使用print调试，学习使用pdb调试器

- 
**项目驱动**：通过实际项目学习，解决真实问题

- 
**参与社区**：阅读他人代码，参与开源项目，学习最佳实践

- 
**理解原理**：不仅要会用，还要理解背后的原理

- 
**代码规范**：遵循PEP8编码规范，写出可读性高的代码
