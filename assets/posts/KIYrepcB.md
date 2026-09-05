# NapCat 安装与接入 OneBot 全流程教程

如果你想把 QQ 接入机器人框架（比如 LangBot、NoneBot、Koishi 等），那你一定会听到一个名字 —— **NapCat**。
这篇文章带你从 0 到 1 搭建 NapCat，并通过 WebSocket 接入 OneBot。

---

## 一、NapCat 是什么？

**NapCat** 是一个基于 QQ 协议实现的 OneBot 标准框架，主要用于：

- 
QQ 机器人对接

- 
群聊管理

- 
自动回复

- 
插件扩展

- 
与 LangBot / NoneBot 等机器人框架对接

它本质上是一个 QQ 协议实现层，让你的机器人“有号可用”。

---

## 二、准备环境

建议环境：

- 
Linux 服务器（推荐 Ubuntu 20+）

- 
Docker（强烈推荐）

- 
已注册 QQ 账号

如果你使用的是宝塔 Docker 面板（btDocker），也是完全可以的。

---

## 三、官方一键安装（推荐）

在服务器中执行：

```
curl -o napcat.sh \
https://nclatest.znin.net/NapNeko/NapCat-Installer/main/script/install.sh \
&& bash napcat.sh
```

安装完成后会自动拉取镜像并创建容器。

---

## 四、Docker 手动安装方式

如果你想自己控制容器：

### 1️⃣ 拉取镜像

```
docker pull napneko/napcat-docker:latest
```

### 2️⃣ 创建容器

```
docker run -d \
--name napcat \
-p 8082:8082 \
-p 3000:3000 \
-v /root/napcat:/app/config \
--restart=always \
napneko/napcat-docker:latest
```

说明：

端口

用途

3000

Web 管理面板

8082

WebSocket 端口（OneBot）

---

## 五、登录 QQ

Docker 手动安装的浏览器访问：

```
http://服务器IP:3000
```

官方一键安装的：

```
http://服务器IP:6099
```

打开 NapCat 管理后台。

使用手机 QQ 扫码登录即可。

> ⚠️ 建议使用小号测试，避免主号风控。

---

## 六、配置 WebSocket（OneBot 连接）

进入 NapCat 后台：

👉 添加 WebSocket 客户端

![](assets/png/articles/75c44c31a9aa.png)

示例配置：

```
名称: 1
URL: ws://localhost:8082
消息格式: Array
Token: pgsQPcXnW-GT42~b
心跳间隔: 30000
重连间隔: 30000
```

### 参数解释

参数

说明

URL

机器人框架连接地址

消息格式

建议 Array

Token

身份验证密钥

心跳间隔

建议 30000ms

重连间隔

建议 30000ms

---

## 七、对接 LangBot

如果你使用的是 **LangBot**：

在 LangBot 配置文件中：

```
onebot:
  ws_url: ws://服务器IP:8082
  access_token: pgsQPcXnW-GT42~b
```

然后启动 LangBot 即可。

---

## 八、常见问题

### 1️⃣ 连接不上 8082？

检查：

- 
Docker 端口是否映射

- 
服务器防火墙是否放行

- 
宝塔安全组是否放行端口

---

### 2️⃣ HTTP 403 / 风控？

可能原因：

- 
登录过于频繁

- 
同一 IP 多号登录

- 
使用主号

解决：

- 
更换 IP

- 
使用小号

- 
降低操作频率

---

### 3️⃣ 容器一直重启？

执行：

```
docker logs napcat
```

查看报错日志。

---

## 九、安全建议

- 
不要使用主 QQ

- 
不要开启危险 API

- 
定期更新镜像

- 
设置强 Token

- 
避免高频群发

---

## 十、进阶玩法

NapCat 可以结合：

- 
NoneBot 插件系统

- 
AI 聊天模型

- 
自动签到脚本

- 
群管机器人

你甚至可以搭建一个：

> AI + QQ 群 + 自动管理 + 自定义插件

完整自动化机器人系统。

---

## 总结

NapCat 是目前较成熟的 QQ OneBot 实现方案之一，部署简单、Docker 友好，非常适合个人开发者和机器人爱好者使用。

如果你正在做：

- 
QQ 群 AI 机器人

- 
自动管理机器人

- 
LangBot 接入 QQ

- 
私人智能助理

NapCat 是一个不错的选择。
