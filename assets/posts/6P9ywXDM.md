# HTML 完整开发教程 第1章

- 

# HTML 完整开发教程 第1章

> 
> 从零基础到专业开发者的 HTML 系统教程
> 适合：初学者 / 前端开发者 / 网站开发者

---

# 第1章 Web 与互联网工作原理

在学习 HTML 之前，理解 Web

的工作方式非常重要。本章将介绍互联网的基本结构、浏览器如何访问网站、DNS

解析过程以及 HTTP/HTTPS 的工作原理。

学习完本章，你将理解：

互联网是如何连接全球计算机的

- 浏览器如何访问网站

- DNS 如何解析域名

- HTTP 协议如何传输网页

- 浏览器如何解析 HTML 并渲染页面

---

## 1.1 什么是互联网

互联网（Internet）是一个由全球计算机网络互相连接形成的巨大网络系统。

它允许不同国家、不同地区、不同设备之间进行信息交换和资源共享。

简单来说：

```
互联网 = 全球计算机网络的集合

```

互联网的核心功能包括：

- 信息传播

- 数据交换

- 在线服务

- 网络通信

常见的互联网服务包括：

- 网站（Web）

- 电子邮件

- 在线聊天

- 视频流媒体

- 云计算服务

---

## 1.2 网站的本质

网站本质上是一组存储在服务器上的网页文件。

例如一个简单网站可能包含：

```
index.html
about.html
blog.html
contact.html

```

这些文件通常使用以下技术构建：

技术         作用

---

HTML         页面结构

CSS          页面样式

JavaScript   页面交互

例如一个最简单的 HTML 页面：

```

我的第一个网页

# Hello World

这是我的第一个网页。

```

---

## 1.3 浏览器是什么

浏览器（Browser）是一种用于访问网页的软件。

常见浏览器包括：

浏览器    公司

---

Chrome    Google

Firefox   Mozilla

Edge      Microsoft

Safari    Apple

浏览器的主要任务是：

- 向服务器发送请求

- 下载网页资源

- 解析 HTML

- 渲染网页内容

---

## 1.4 URL 与网站地址

URL（Uniform Resource Locator）表示统一资源定位符。

一个 URL 的典型结构如下：

```
https://www.example.com/index.html

```

组成部分：

部分              含义

---

https             协议

[www.example.com](http://www.example.com)   域名

/index.html       资源路径

示例：

```
https://google.com
https://github.com
https://developer.mozilla.org

```

---

## 1.5 DNS 域名解析

计算机之间通信使用 IP 地址，例如：

```
142.250.72.14

```

但是 IP 地址难以记忆，因此出现了域名系统 DNS（Domain Name System）。

DNS 的作用：

```
域名 → IP地址

```

DNS 查询大致流程：

- 浏览器查询本地缓存

- 查询运营商 DNS

- 查询根域名服务器

- 查询顶级域名服务器

- 查询权威 DNS 服务器

- 返回最终 IP 地址

---

## 1.6 HTTP 协议

HTTP（HyperText Transfer Protocol）是浏览器与服务器之间通信的协议。

浏览器访问网页时会发送 HTTP 请求，例如：

```
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Chrome

```

服务器返回响应：

```
HTTP/1.1 200 OK
Content-Type: text/html

```

然后发送 HTML 内容给浏览器。

---

## 1.7 HTTPS

HTTP 是明文传输协议，存在安全风险，例如：

- 数据被监听

- 数据被篡改

HTTPS 在 HTTP 的基础上增加了 SSL/TLS 加密。

特点：

- 数据加密

- 身份验证

- 防止数据篡改

浏览器地址栏通常会显示 🔒 表示安全连接。

---

## 1.8 浏览器渲染流程

浏览器收到 HTML 后，会经过以下步骤：

```
HTML → DOM
CSS → CSSOM
DOM + CSSOM → Render Tree
Layout
Paint

```

阶段说明：

阶段          含义

---

DOM           HTML 文档对象模型

CSSOM         CSS 样式对象模型

Render Tree   渲染树

Layout        计算布局

Paint         页面绘制

---

## 1.9 HTML 的作用

HTML 是网页的结构语言。

HTML 负责：

- 页面结构

- 内容组织

- 文档语义

HTML **不负责**：

- 页面美观（CSS负责）

- 页面交互（JavaScript负责）

三者关系：

```
HTML  → 结构
CSS   → 样式
JavaScript → 交互

```

---

## 本章小结

在本章中，我们学习了：

- 互联网的基本结构

- 网站的本质

- 浏览器的作用

- URL 与 DNS 的工作方式

- HTTP / HTTPS 协议

- 浏览器如何渲染网页

- HTML 在 Web 中的作用

这些知识为后续学习 HTML 标签和网页开发打下了基础。

在下一章，我们将学习 **HTML 的历史与标准，以及 HTML5 的发展**。
