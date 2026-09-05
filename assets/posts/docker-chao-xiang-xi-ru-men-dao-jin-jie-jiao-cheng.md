# Docker 超详细入门到进阶教程

# Docker 超详细入门到进阶教程（从零开始，生产可用）

> 适合对象：完全新手、运维/后端/全栈开发者、想把项目容器化的人阅读目标：学完即可**独立编写 Dockerfile、构建镜像、运行容器、使用 Docker Compose、理解生产实践**
> ![Docker-Symbol-njqigrsm.png](assets/png/articles/499ffa62ff1d.png)

---

## 一、Docker 是什么？为什么要用 Docker

### 1.1 传统部署的痛点

在没有 Docker 之前，软件部署通常是这样的：

- 
在服务器上安装操作系统

- 
手动安装运行环境（JDK / Python / Node / Nginx / MySQL）

- 
拷贝代码

- 
配置环境变量

- 
启动服务

**常见问题：**

- 
「我电脑上能跑，服务器跑不了」

- 
环境版本不一致（Python 3.8 / 3.10）

- 
迁移服务器成本高

- 
回滚困难

- 
多项目依赖冲突

### 1.2 Docker 的核心思想

Docker 的核心思想只有一句话：

> **把应用 + 运行环境 + 依赖 + 配置，打包成一个“镜像”，一次构建，到处运行。**

Docker 并不是虚拟机，它是：

- 
基于 Linux 内核

- 
使用 **容器（Container）隔离进程**

- 
启动速度极快（秒级甚至毫秒级）

- 
占用资源极少

### 1.3 Docker 和虚拟机的区别

对比项

Docker

虚拟机

启动速度

秒级

分钟级

资源占用

低

高

是否包含完整 OS

否

是

隔离级别

进程级

系统级

部署效率

极高

较低

Docker 更适合：

- 
微服务

- 
CI/CD

- 
云原生

- 
快速部署

---

## 二、Docker 的核心概念（非常重要）

### 2.1 镜像（Image）

**镜像 = 只读模板**

- 
包含：

基础系统（如 Alpine / Ubuntu）

- 
运行环境

- 
应用代码

- 
启动命令

镜像本身**不能运行**，只能用来创建容器。

类比：

> 镜像 ≈ 安装包 / 模板

### 2.2 容器（Container）

**容器 = 镜像的运行实例**

- 
镜像是静态的

- 
容器是动态的

- 
容器可启动 / 停止 / 删除

类比：

> 容器 ≈ 正在运行的程序

### 2.3 仓库（Registry）

存放镜像的地方：

- 
Docker Hub（官方）

- 
阿里云镜像仓库

- 
私有仓库

常见命令：

- 
`docker pull`

- 
`docker push`

### 2.4 Docker 架构

Docker 由三部分组成：

- 
Docker Client（客户端）

- 
Docker Daemon（守护进程）

- 
Docker Registry（镜像仓库）

工作流程：

> Client → Daemon → Registry

---

## 三、Docker 安装（Windows / Linux）

### 3.1 Windows 安装 Docker

推荐方式：

- 
安装 **Docker Desktop**

- 
需要开启：

WSL2

- 
虚拟化

安装完成后验证：

```
docker version
docker info
```

### 3.2 Linux 安装 Docker（以 Ubuntu 为例）

```
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

验证：

```
docker --version
```

非 root 用户使用 Docker：

```
sudo usermod -aG docker $USER
newgrp docker
```

---

## 四、Docker 常用命令大全（必须掌握）

### 4.1 镜像相关命令

```
docker images            # 查看本地镜像
docker pull nginx        # 拉取镜像
docker rmi nginx         # 删除镜像
docker tag               # 镜像打标签
```

### 4.2 容器相关命令

```
docker ps                # 运行中的容器
docker ps -a             # 所有容器
docker run               # 运行容器
docker start/stop        # 启动/停止容器
docker restart           # 重启容器
docker rm                # 删除容器
```

### 4.3 查看日志与进入容器

```
docker logs 容器名
docker exec -it 容器名 /bin/bash
```

---

## 五、Docker run 详解（核心）

示例：

```
docker run -d -p 8080:80 --name my-nginx nginx
```

参数解释：

- 
`-d`：后台运行

- 
`-p 8080:80`：端口映射

- 
`--name`：容器名称

- 
`nginx`：镜像名

端口映射规则：

> 宿主机端口 : 容器端口

---

## 六、数据卷（Volume）与数据持久化

### 6.1 为什么需要数据卷

容器删除后：

- 
容器内数据默认会丢失

解决方案：

- 
数据卷（Volume）

- 
目录挂载（Bind Mount）

### 6.2 使用数据卷

```
docker volume create mydata

docker run -v mydata:/data busybox
```

### 6.3 目录挂载（最常用）

```
docker run -v /host/data:/container/data nginx
```

生产环境 **强烈推荐目录挂载**。

---

## 七、Dockerfile（镜像构建核心）

### 7.1 Dockerfile 是什么

Dockerfile 是一个文本文件，用来**描述如何构建镜像**。

### 7.2 常用指令详解

```
FROM        # 基础镜像
WORKDIR     # 工作目录
COPY        # 拷贝文件
RUN         # 执行命令
EXPOSE      # 暴露端口
CMD         # 容器启动命令
```

### 7.3 示例：Python Web 应用

```
FROM python:3.10-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

构建镜像：

```
docker build -t myapp:1.0 .
```

---

## 八、Docker Compose（多容器编排）

### 8.1 为什么需要 Compose

项目通常包含：

- 
Web

- 
数据库

- 
缓存

Compose 可以：

- 
一个文件管理多个容器

- 
一条命令启动所有服务

### 8.2 docker-compose.yml 示例

```
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:5000"
    depends_on:
      - redis

  redis:
    image: redis:7
```

启动：

```
docker-compose up -d
```

停止：

```
docker-compose down
```

---

## 九、Docker 网络基础

Docker 默认网络类型：

- 
bridge（默认）

- 
host

- 
none

查看网络：

```
docker network ls
```

容器之间通过 **服务名通信**（Compose 中非常重要）。

---

## 十、生产环境 Docker 实践建议

### 10.1 镜像优化

- 
使用 `alpine` 或 `slim`

- 
多阶段构建

- 
减少层数

### 10.2 安全建议

- 
不使用 root 运行

- 
限制端口暴露

- 
定期更新镜像

### 10.3 常见错误

- 
容器一启动就退出（CMD 写错）

- 
端口未映射

- 
数据未持久化

---

## 十一、Docker 常见问题解答

### Q1：Docker 会很占内存吗？

不会，Docker 比虚拟机轻得多。

### Q2：Docker 能跑在 Windows 吗？

可以，通过 WSL2。

### Q3：生产环境能用 Docker 吗？

不仅能，而且**强烈推荐**。

---

## 十二、学习路线建议

- 
熟练 docker run

- 
精通 Dockerfile

- 
使用 Docker Compose

- 
理解网络与数据卷

- 
进阶 Kubernetes

---

## 结语

Docker 是现代开发与运维的**必备技能**。

一旦掌握，你将：

- 
告别环境地狱

- 
提升部署效率

- 
拥抱云原生世界
