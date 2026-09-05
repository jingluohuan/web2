# C++ 编程完全指南

**第一部分：环境搭建**

- 
编译器安装

Windows: 安装MinGW或Visual Studio

- 
Linux: `sudo apt-get install g++`

- 
Mac: 安装Xcode Command Line Tools

- 
IDE推荐

Visual Studio Code + C++扩展

- 
CLion

- 
Code::Blocks

## 第二部分：基础语法

### 1. Hello World 程序

```
#include 
using namespace std;

int main() {
    cout 
- 
`#include`：头文件包含指令

- 
`main()`：程序入口函数

- 
`cout`：标准输出流对象

### 2. 变量与数据类型

**类型**

**说明**

**示例**

int

整型

42

double

双精度浮点

3.14159

char

字符

'A'

bool

布尔值

true/false

string

字符串

"Hello"

### 3. 输入输出

```
int age;
cout > age;
cout 

### 4. 控制结构

#### 条件语句

```
if (score >= 90) {
    cout = 80) {
    cout 

#### 循环结构

```
// for循环
for(int i=0; i

### 5. 函数

```
int add(int a, int b) {
    return a + b;
}

// 函数调用
int result = add(3, 4);
```

### 6. 数组

```
int numbers[5] = {1,2,3,4,5};
cout 

### 7. 指针与引用

```
int var = 20;
int* ptr = &var;  // 指针
int& ref = var;   // 引用

*ptr = 30;  // 通过指针修改值
ref = 40;   // 通过引用修改值
```

## 第三部分：面向对象编程

### 1. 类与对象

```
class Rectangle {
private:
    int width, height;

public:
    // 构造函数
    Rectangle(int w, int h) : width(w), height(h) {}

    // 成员函数
    int area() {
        return width * height;
    }
};

// 使用类
Rectangle rect(3,4);
cout 

### 2. 继承

```
class Shape {
public:
    virtual void draw() = 0; // 纯虚函数
};

class Circle : public Shape {
public:
    void draw() override {
        cout 

### 3. 多态

```
Shape* shape = new Circle();
shape->draw(); // 调用Circle的draw方法
```

## 第四部分：高级特性

### 1. 模板

```
template 
T max(T a, T b) {
    return (a > b) ? a : b;
}

// 使用
cout (3,5);   // 输出5
cout (2.1,3.14); // 输出3.14
```

### 2. 标准模板库（STL）

#### vector容器

```
#include 
vector vec = {1,2,3};
vec.push_back(4);  // 添加元素
for(int num : vec){
    cout 

#### map容器

```
#include 
map ages;
ages["Alice"] = 25;
cout 

### 3. 异常处理

```
try {
    int* arr = new int[1000000000000]; // 可能抛出异常
} catch (const bad_alloc& e) {
    cerr 

### 4. 文件操作

```
#include 

// 写入文件
ofstream outFile("data.txt");
outFile 

## 第五部分：实战项目

### 学生管理系统

实现功能：

- 
添加学生信息

- 
查询学生成绩

- 
修改学生信息

- 
数据持久化存储

核心数据结构：

```
struct Student {
    string name;
    int id;
    vector scores;
};
```

## 学习建议

- 
坚持每天编码练习

- 
阅读优秀开源代码

- 
掌握调试工具（gdb）

- 
学习现代C++特性（C++11/14/17）

- 
参与编程社区讨论

推荐进阶学习：

- 
《C++ Primer》

- 
《Effective C++》

- 
C++标准库文档

- 
算法与数据结构实现

通过系统学习和实践，你可以逐步掌握C++的强大功能，开发高性能应用程序。编程能力的提升需要时间和实践积累，保持学习热情是关键！
