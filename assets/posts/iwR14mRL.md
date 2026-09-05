# HTML 完整开发教程 第3章

- 

# HTML 完整开发教程 第3章

## 第3章 HTML 开发环境与工具

在学习 HTML 编写网页之前，我们需要先准备好开发环境。

与许多编程语言相比，HTML

的开发环境非常简单------只需要一个文本编辑器和一个浏览器即可。

但在实际开发中，使用专业工具可以大大提高效率。

本章将介绍：

HTML 开发所需的基本环境

- 常见代码编辑器

- 浏览器开发者工具

- 本地服务器

- 前端开发工作流程

---

## 3.1 HTML 开发需要什么

开发 HTML 页面只需要三个基本工具：

- 文本编辑器

- 浏览器

- 文件系统

最简单的 HTML 开发流程：

```
写 HTML 文件 → 保存 → 浏览器打开

```

例如：

创建文件：

```
index.html

```

写入代码：

```

我的网页

# Hello HTML

这是我的第一个页面。

```

然后用浏览器打开即可。

---

## 3.2 选择代码编辑器

虽然系统自带的记事本也可以写 HTML，但专业编辑器会更方便。

常见代码编辑器：

编辑器         特点

---

VS Code        最流行的前端编辑器

Sublime Text   速度非常快

WebStorm       专业前端IDE

Notepad++      轻量级编辑器

推荐使用：

```
VS Code

```

因为：

- 免费

- 插件丰富

- 前端支持很好

---

## 3.3 安装 VS Code

安装步骤：

- [打开官网](https://code.visualstudio.com)

```
https://code.visualstudio.com

```

- 下载对应系统版本

- 安装软件

安装完成后启动 VS Code。

---

## 3.4 VS Code 常用功能

VS Code 对 HTML 开发非常友好。

主要功能包括：

### 自动补全

例如输入：

```
html

```

可以自动生成 HTML 模板。

### Emmet

输入：

```
ul>li*5

```

自动生成：

```

- 

- 

- 

- 

- 

```

### 多光标编辑

按住：

```
Alt

```

可以同时编辑多行代码。

---

## 3.5 推荐 VS Code 插件

安装插件可以大幅提高开发效率。

推荐插件：

插件               作用

---

Live Server        实时刷新网页

Prettier           代码格式化

HTML CSS Support   CSS 提示

Auto Rename Tag    自动修改标签

例如安装 Live Server 后：

右键 HTML 文件 →

```
Open with Live Server

```

浏览器会自动打开网页，并且修改代码会自动刷新。

---

## 3.6 浏览器开发者工具

现代浏览器都提供强大的开发工具。

打开方法：

```
F12

```

或者

```
右键 → 检查

```

开发者工具主要包括：

- Elements

- Console

- Network

- Sources

---

### Elements 面板

用于查看 HTML 结构。

你可以：

- 查看 DOM

- 修改 HTML

- 修改 CSS

示例：

```

# Hello

```

可以在浏览器中实时修改。

---

### Console 面板

Console 用于运行 JavaScript。

例如：

```
console.log("Hello World")

```

---

### Network 面板

Network 可以查看网页加载资源，例如：

- HTML

- CSS

- JavaScript

- 图片

这对于性能优化非常重要。

---

## 3.7 本地服务器

很多 HTML 页面可以直接打开，但有些功能需要服务器环境，例如：

- Ajax 请求

- API 调用

- 模块加载

因此通常需要启动本地服务器。

---

### 使用 VS Code Live Server

安装插件：

```
Live Server

```

然后：

右键 HTML 文件 →

```
Open with Live Server

```

浏览器会访问：

```
http://127.0.0.1:5500

```

---

### 使用 Python 启动服务器

如果电脑安装了 Python，可以使用：

```
python -m http.server

```

然后访问：

```
http://localhost:8000

```

---

## 3.8 项目目录结构

在开发网站时，建议使用规范目录结构。

例如：

```
project/
│
├─ index.html
├─ about.html
│
├─ css/
│   └─ style.css
│
├─ js/
│   └─ main.js
│
└─ images/
    └─ logo.png

```

这样可以让项目更加清晰。

---

## 3.9 HTML 调试技巧

开发网页时经常需要调试。

常见方法包括：

### 1 使用浏览器开发者工具

查看 HTML 和 CSS。

### 2 查看 Console 错误

如果 JavaScript 出错，Console 会显示错误信息。

### 3 查看 Network 请求

检查资源是否加载成功。

---

## 本章小结

本章介绍了 HTML 开发所需的工具和环境：

- HTML 开发基础环境

- VS Code 编辑器

- 常用插件

- 浏览器开发者工具

- 本地服务器

- 项目目录结构

掌握这些工具后，我们就可以正式开始编写 HTML 页面。

下一章我们将学习：

```
HTML 文档结构

```

这是 HTML 最基础也是最重要的内容。
