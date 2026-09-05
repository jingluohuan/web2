# 雨云服务器搭建与使用全教程

如果你正在找一款**价格便宜、性能不错、国内访问速度快**的云服务器，那么「**雨云（Rainyun）**」绝对值得一试。
无论是搭建个人博客、部署网站、跑 Docker、做下载站，还是当作学习 Linux 的练手服务器，雨云都非常合适。

本文将带你 **从 0 到 1 完成雨云服务器的购买、配置、连接和常见用途部署**，即使是第一次接触云服务器，也能照着一步步来。

---

## 一、什么是雨云？适合谁用？

雨云是一家国内云服务商，主打：

- 
💰 **价格低**（学生/个人用户友好）

- 
🚀 **国内节点，访问速度快**

- 
🧠 **面板简单，上手快**

- 
🔧 支持 Linux / Windows 云服务器

- 
📦 支持按需付费、灵活配置

### 适合人群

- 
想搭建 **个人博客 / 技术博客**

- 
想做 **下载站 / 镜像站**

- 
学习 Linux、Docker、Nginx

- 
轻量级 Web 项目部署

- 
需要一台稳定的长期在线服务器

---

## 二、注册雨云账号（含优惠码）

👉 **推荐通过下面这个链接注册，可享受官方优惠：**
**🔗 **[**https://www.rainyun.com/OTg3Mjk5**](https://www.rainyun.com/OTg3Mjk5_)[**_**](https://www.rainyun.com/OTg3Mjk5_)

> 使用该链接注册并购买云服务器时，可获得额外优惠（以雨云当前活动为准）。

注册流程非常简单：

- 
打开上面的链接

- 
使用手机号或邮箱注册

- 
完成登录即可进入控制台

---

## 三、购买雨云云服务器（VPS）

登录后，进入 **控制台 → 云服务器**，开始创建实例。

### 1️⃣ 选择服务器配置

新手推荐配置（够用又省钱）：

- 
CPU：1 核

- 
内存：1GB / 2GB

- 
硬盘：20GB SSD

- 
系统：**Ubuntu 22.04 LTS**（强烈推荐）

- 
带宽：按需选择

> 如果只是博客、个人网站，1C2G 完全够用。

### 2️⃣ 选择系统

推荐顺序：

- 
✅ Ubuntu 22.04（最稳、资料最多）

- 
Debian 12

- 
CentOS（不再推荐新手）

### 3️⃣ 设置登录方式

- 
设置 **root 密码**

- 
或选择 **SSH Key（进阶用户）**

创建完成后，雨云会分配给你一个 **公网 IP**。

---

## 四、连接服务器（SSH 登录）

### Windows 用户

推荐工具：

- 
**Xshell**

- 
**FinalShell**

- 
**PowerShell（Windows 10+ 自带）**

使用命令登录：

```
ssh root@你的服务器IP
```

输入密码后即可登录。

### Linux / macOS 用户

终端直接执行：

```
ssh root@你的服务器IP
```

---

## 五、服务器基础初始化（必做）

登录后，先做一些基础操作。

### 1️⃣ 更新系统

```
apt update && apt upgrade -y
```

### 2️⃣ 安装常用工具

```
apt install -y curl wget git unzip vim
```

---

## 六、常见用途一：搭建个人博客（Nginx）

### 1️⃣ 安装 Nginx

```
apt install -y nginx
```

启动并设置开机自启：

```
systemctl start nginx
```

systemctl enable nginx

浏览器访问：

```
http://你的服务器IP
```

看到 **Welcome to nginx**，说明成功 🎉

### 2️⃣ 放置网站文件

默认网站目录：

```
/var/www/html
```

你可以上传：

- 
HTML 静态网站

- 
Hexo / Hugo 生成的博客文件

---

## 七、常见用途二：Docker 环境部署（强烈推荐）

雨云非常适合跑 Docker。

### 1️⃣ 安装 Docker

```
curl -fsSL https://get.docker.com | bash
```

启动 Docker：

```
systemctl start docker
```

systemctl enable docker

### 2️⃣ 测试 Docker

```
docker run hello-world
```

---

## 八、常见用途三：搭建下载站 / 服务端程序

你可以用雨云服务器来部署：

- 
下载站（Nginx + PHP）

- 
API 服务

- 
Python / Node.js 项目

- 
镜像站

- 
内网穿透中转节点

雨云国内网络延迟低，非常适合国内用户访问。

---

## 九、服务器安全建议（新手必看）

- 
❗ 修改 SSH 默认端口

- 
❗ 设置防火墙（ufw）

- 
❗ 不要随意暴露服务端口

- 
❗ 定期更新系统

简单开启防火墙示例：

```
ufw allow ssh
```

ufw allow 80

ufw allow 443

ufw enable

---

## 十、总结：为什么推荐雨云？

✔ 价格亲民
 ✔ 国内访问速度快
 ✔ 新手友好
 ✔ 非常适合个人站长和学习使用

如果你正准备入手第一台云服务器，或者想找一个性价比高的 VPS，雨云是一个非常不错的选择。

👉 **再次附上我的优惠注册链接：**
 [**https://www.rainyun.com/OTg3Mjk5**](https://www.rainyun.com/OTg3Mjk5_)

[https://chata.itxiaohui.top/10002](https://chata.itxiaohui.top/10002)
