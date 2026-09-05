# 宝塔面板使用教程

# 宝塔面板（BT Panel）超详细使用教程

**——从下载安装到服务器环境配置，一篇搞定服务器运维**

---

## 一、什么是宝塔面板？

宝塔面板（BT Panel）是一款 **Linux / Windows 服务器可视化管理工具**，通过 Web 界面即可完成：

- 
网站部署（Nginx / Apache）

- 
PHP、MySQL、Redis 环境管理

- 
域名绑定、SSL 证书

- 
文件管理

- 
数据库管理

- 
防火墙与安全配置

- 
定时任务、备份

- 
服务器监控

![btmblogo-ioiyuhtl.webp](assets/png/articles/ca669e340889.webp)

宝塔官网：

[https://www.bt.cn/](https://www.bt.cn/)
一句话总结：

> **宝塔 = 把复杂的 Linux 命令，变成点鼠标就能完成**

---

## 二、宝塔面板适合哪些人？

- 
🧑‍💻 **新手站长 / 博客主**

- 
🧑‍🎓 学习 Linux / Web 运维的学生

- 
🏢 中小网站管理员

- 
⚙️ 不想天天敲命令的程序员

- 
🧠 想快速部署 WordPress / Halo / Typecho / Laravel 的用户

---

## 三、宝塔面板支持哪些系统？

### 1️⃣ Linux（最常用）

- 
CentOS 7 / 8

- 
Ubuntu 18 / 20 / 22

- 
Debian 10 / 11 / 12

- 
Rocky Linux / AlmaLinux

### 2️⃣ Windows Server

- 
Windows Server 2012 / 2016 / 2019 / 2022

> ⚠️ **生产环境强烈推荐 Linux + 宝塔 Linux 面板**

---

## 四、安装宝塔面板前的准备

### 1️⃣ 一台服务器（VPS / 云服务器）

常见选择：

- 
[阿里云](https://www.aliyun.com/minisite/goods?userCode=wc9agrae)

- 
腾讯云

- 
华为云

- 
Vultr

- 
搬瓦工

- 
AWS / Azure

### 2️⃣ 最低配置建议

项目

最低

推荐

CPU

1 核

2 核

内存

1GB

2GB+

硬盘

20GB

40GB+

系统

Linux

Linux

### 3️⃣ 需要准备的工具

- 
SSH 客户端

Windows：Xshell / FinalShell / PowerShell

- 
macOS / Linux：终端（Terminal）

---

## 五、宝塔 Linux 面板下载安装（超详细）

### 1️⃣ 使用 SSH 连接服务器

```
ssh root@你的服务器IP

```

输入 root 密码即可登录。

---

### 2️⃣ 选择对应系统的安装命令

#### ▶ 通用安装脚本（推荐）

```
if [ -f /usr/bin/curl ];then curl -sSO https://download.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://download.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ssl251104

```

**电信线路：**

```
if [ -f /usr/bin/curl ];then curl -sSO https://cmcc1-node.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://cmcc1-node.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

**香港线路：**

```
if [ -f /usr/bin/curl ];then curl -sSO https://hk1-node.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://hk1-node.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

**欧美线路：**

```
if [ -f /usr/bin/curl ];then curl -sSO https://cf1-node.aapanel.com/install/install_panel.sh;else wget -O install_panel.sh https://cf1-node.aapanel.com/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

> 若无法访问，可使用国内镜像（宝塔官方备用）

---

宝塔**安装网页：**

[https://www.bt.cn/new/download.html](https://www.bt.cn/new/download.html)

### 3️⃣ 安装过程说明

安装过程中会看到：

- 
检测系统环境

- 
自动安装依赖

- 
下载面板文件

- 
创建面板服务

整个过程约 **2–5 分钟**

---

### 4️⃣ 安装完成后的重要信息（一定要保存）

安装完成后终端会显示：

```
Bt-Panel: http://服务器IP:8888
username: xxxxxx
password: xxxxxx

```

📌 **请立即复制保存：**

- 
面板地址

- 
用户名

- 
密码

---

## 六、首次登录宝塔面板

### 1️⃣ 浏览器访问面板

```
http://服务器IP:8888
或完成安装后终端显示的
```

### 2️⃣ 放行端口（若无法访问）

云服务器需放行端口：

- 
8888（面板）

- 
80（HTTP）

- 
443（HTTPS）

---

### 3️⃣ 登录成功后的初始化界面

首次登录会提示：

> **请选择运行环境**

---

![](assets/png/articles/fbafdc6fac9b.png)

## 七、宝塔环境配置（最核心部分）

### 1️⃣ 运行环境选择推荐

组件

推荐

Web 服务器

Nginx

PHP

7.4 / 8.0

数据库

MySQL 5.7

缓存

Redis（可选）

FTP

Pure-FTPd

📌 **新手推荐组合：**

```
Nginx + PHP 7.4 + MySQL 5.7

```

---

### 2️⃣ 安装 LNMP 环境

点击【一键安装】后：

- 
自动下载

- 
自动编译

- 
自动配置

⏱️ 时间：10–30 分钟（取决于服务器性能）

---

### 3️⃣ 环境安装完成后检查

进入：

【软件商店】

确认以下状态为 **运行中**：

- 
Nginx

- 
PHP

- 
MySQL

![截屏2025-12-26 17.04.20-nazgrsyr.png](assets/png/articles/5b70dc06f3e7.png)

---

## 八、宝塔面板界面详解

### 1️⃣ 左侧菜单说明

![截屏2025-12-26 17.05.55-fbmjmqqm.webp](assets/png/articles/b1db213cf972.webp)

菜单

作用

仪表盘

服务器状态

网站

站点管理

FTP

FTP 用户

数据库

MySQL 管理

文件

在线文件管理

软件商店

安装组件

安全

防火墙 / 端口

计划任务

定时任务

设置

面板设置

---

## 九、创建第一个网站（详细）

### 1️⃣ 新建站点

进入【网站】 → 【添加站点】

填写：

- 
域名：`example.com`

- 
根目录：自动生成

- 
PHP 版本：7.4

- 
数据库：勾选

- 
FTP：可选

点击【提交】

![](assets/png/articles/293b5764e700.png)

![](assets/png/articles/6de1fabdbbf5.webp)

---

### 2️⃣ 网站目录结构说明

```
/www/wwwroot/example.com
├── index.html
├── index.php

```

---

### 3️⃣ 访问测试

浏览器访问：

```
http://example.com

```

看到默认页面说明配置成功。

---

## 十、数据库管理（MySQL）

### 1️⃣ 创建数据库

【数据库】→【添加数据库】

- 
数据库名

- 
用户名

- 
密码

---

### 2️⃣ 使用 phpMyAdmin

点击【管理】即可进入图形化数据库管理。

---

## 十一、上传网站文件

### 方法一：宝塔文件管理器（推荐新手）

【文件】→ 进入网站目录 → 上传

![](assets/png/articles/a91129366e77.png)

### 方法二：FTP

- 
创建 FTP 账号

- 
使用 FileZilla 连接

---

## 十二、配置 PHP 详细说明

### 1️⃣ PHP 版本切换

【网站】→ 设置 → PHP 版本

### 2️⃣ 常用 PHP 配置

- 
`upload_max_filesize`

- 
`post_max_size`

- 
`memory_limit`

- 
`max_execution_time`

📌 推荐值：

```
upload_max_filesize = 100M
post_max_size = 100M
memory_limit = 256M

```

---

## 十三、SSL 证书（HTTPS）

### 1️⃣ 免费 SSL（Let's Encrypt）

【网站】→ 设置 → SSL → Let's Encrypt

勾选域名 → 申请

![](assets/png/articles/b2749f76ce67.png)

---

### 2️⃣ 强制 HTTPS

开启：

✅ 强制 HTTPS
✅ HTTP 重定向 HTTPS

![](assets/png/articles/686a0034d885.png)

---

## 十四、安全配置（非常重要）

### 1️⃣ 修改面板端口

【设置】→ 修改默认 8888

![](assets/png/articles/890bc5a445b3.png)

### 2️⃣ 面板安全入口

开启：

```
http://IP:端口/随机入口

```

---

### 3️⃣ 防火墙设置

【安全】→ 放行端口

仅保留必要端口：

- 
22

- 
80

- 
443

- 
面板端口

---

## 十五、计划任务（定时器）

常见用途：

- 
网站备份

- 
数据库备份

- 
清理日志

- 
定时脚本

### 示例：每天备份数据库

【计划任务】→ 备份数据库 → 每天 03:00

![](assets/png/articles/bb6baf19ac0d.png)

---

## 十六、备份与恢复

### 1️⃣ 站点备份

【网站】→ 备份

![](assets/png/articles/10b0a5997de0.png)

![](assets/png/articles/1ec443a966b0.png)

### 2️⃣ 数据库备份

【数据库】→ 备份

📌 **强烈建议开启自动备份**

---

## 十七、宝塔常见优化建议

- 
启用 OPcache

- 
开启 Nginx Gzip

- 
使用 Redis 缓存

- 
禁用不必要服务

- 
定期更新组件

---

## 十八、常见问题汇总

### Q1：宝塔安全吗？

- 
改端口

- 
开启安全入口

- 
不暴露 root

即可安全使用。

---

### Q2：生产环境能用吗？

可以，但建议：

- 
不装太多插件

- 
定期备份

- 
关注安全更新

---

## 十九、宝塔适合长期使用吗？

✔ 适合中小站点
✔ 学习成本低
✔ 运维效率高

不适合：

- 
超大型高并发集群

- 
强定制 DevOps 场景

---

## 二十、总结

宝塔面板的核心价值在于：

> **降低服务器运维门槛，让普通人也能轻松搭建网站**

如果你是：

- 
博客站长

- 
独立开发者

- 
学生

- 
运维入门者

**宝塔几乎是必学工具之一。**
