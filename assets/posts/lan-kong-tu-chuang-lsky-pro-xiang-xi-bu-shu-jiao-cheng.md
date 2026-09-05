# 兰空图床（Lsky Pro）详细部署教程

## 兰空图床（Lsky Pro）详细部署教程

### 部署方式选择

兰空图床提供两种主要部署方式：**Docker部署**（推荐）和**传统安装**。Docker方式集成了所有依赖环境，避免了复杂的配置过程，更适合新手。

### 方法一：Docker部署（推荐）

#### 环境准备

- 
**安装Docker**

```
# 更新软件包索引
sudo apt update

# 安装Docker依赖
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# 添加Docker官方GPG密钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 添加Docker仓库
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 安装Docker引擎
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# 启动Docker并设置开机自启
sudo systemctl start docker
sudo systemctl enable docker
```

- 
**安装Docker Compose**

```
# 下载Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 授予执行权限
sudo chmod +x /usr/local/bin/docker-compose

# 验证安装
docker-compose --version
```

#### 创建持久化数据目录

```
# 创建数据目录
mkdir -p ~/lsky-pro-data
mkdir -p ~/lsky-pro-data/themes

# 确保目录权限
sudo chmod -R 755 ~/lsky-pro-data
```

#### 单容器部署

对于简单需求，可以使用单容器部署：

```
docker run -d --name lsky-pro -p 8000:8000 \
    -v ~/lsky-pro-data:/app/storage/app \
    -v ~/lsky-pro-data/themes:/app/themes \
    0xxb/lsky-pro:latest
```

**参数说明：**

- 
`-p 8000:8000`：将宿主机的8000端口映射到容器内的8000端口

- 
`-v ~/lsky-pro-data:/app/storage/app`：持久化存储图片和配置文件

- 
`-v ~/lsky-pro-data/themes:/app/themes`：主题文件存储

#### Docker Compose部署（推荐用于生产）

创建 `docker-compose.yml` 文件：

```
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: lsky-pro-mysql
    environment:
      - MYSQL_ROOT_PASSWORD=your_secure_root_password
      - MYSQL_DATABASE=lsky
      - MYSQL_USER=lsky
      - MYSQL_PASSWORD=your_secure_password
    volumes:
      - mysql-data:/var/lib/mysql
    restart: unless-stopped
    command: 
      - --default-authentication-plugin=mysql_native_password
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_unicode_ci

  redis:
    image: redis:7-alpine
    container_name: lsky-pro-redis
    command: redis-server --appendonly yes --requirepass "your_redis_password"
    volumes:
      - redis-data:/data
    restart: unless-stopped

  lsky-pro:
    image: 0xxb/lsky-pro:latest
    container_name: lsky-pro
    depends_on:
      - mysql
      - redis
    ports:
      - "8000:8000"
    volumes:
      - storage:/app/storage/app
      - themes:/app/themes
    environment:
      - DB_CONNECTION=mysql
      - DB_HOST=mysql
      - DB_PORT=3306
      - DB_DATABASE=lsky
      - DB_USERNAME=lsky
      - DB_PASSWORD=your_secure_password
      - REDIS_HOST=redis
      - REDIS_PASSWORD=your_redis_password
      - REDIS_PORT=6379
    restart: unless-stopped

volumes:
  mysql-data:
    driver: local
  redis-data:
    driver: local
  storage:
    driver: local
  themes:
    driver: local
```

启动服务：

```
docker-compose up -d
```

### 方法二：传统安装（宝塔面板）

#### 环境要求

- 
PHP 8.0.2 或更高版本

- 
MySQL 5.7 或更高版本

- 
Nginx 或 Apache

#### 安装步骤

- 
**安装PHP扩展**

Fileinfo PHP 扩展

- 
Imagick 拓展

- 
**解除PHP函数限制**

exec、shell_exec 函数

- 
readlink、symlink 函数

- 
putenv、getenv 函数

**下载程序**

```
cd /www/wwwroot/your_domain
wget https://github.com/lsky-org/lsky-pro/releases/download/2.1/lsky-pro-2.1.zip
unzip lsky-pro-2.1.zip
chmod -R 755 storage/
chmod -R 755 bootstrap/cache/
chown -R www:www .
```

- 
**配置网站**

运行目录设置为 `public`

- 
设置伪静态：

```
location / {
  try_files $uri $uri/ /index.php?$query_string;
}
```

### 安装配置

- 
**访问安装页面**
在浏览器中访问 `http://你的服务器IP:8000` 或你配置的域名，进入安装界面。

- 
**环境检测**
系统会自动检测环境需求，确保所有检查项都通过。

- 
**数据库配置**

数据库地址：`lsky-pro-mysql`（Docker Compose方式）或 `localhost`（传统方式）

- 
数据库名：`lsky`

- 
用户名：`lsky`

- 
密码：你在配置文件中设置的密码

- 
**管理员账户设置**
设置管理员邮箱和密码，务必使用强密码。

### 基本配置和使用

#### 存储策略配置

- 
登录管理员账户

- 
进入"存储策略"设置

- 
可以配置本地存储或第三方云存储：

本地存储：直接存储在服务器磁盘

- 
第三方存储：支持阿里云OSS、腾讯云COS、七牛云等

#### 用户组管理

- 
进入"角色组管理"

- 
可以创建不同的用户组并设置权限：

上传限制

- 
存储策略权限

- 
图片管理权限

#### 图片上传设置

- 
**格式转换**：可设置上传图片自动转换为WebP格式

- 
**图片水印**：支持文字和图片水印

- 
**原图保护**：防止原图被直接访问

- 
**鉴黄功能**：自动检测违规图片

### 高级配置

#### 域名配置（可选）

如果你拥有域名，可以：

- 
在DNS解析中添加A记录指向服务器IP

2.配置Nginx反向代理：

```
server {
    listen 80;
    server_name your_domain.com;
    
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

#### SSL证书配置

使用Let's Encrypt免费SSL证书：

```
# 安装Certbot
sudo apt install certbot python3-certbot-nginx

# 获取并安装证书
sudo certbot --nginx -d your_domain.com
```

### 维护和备份

#### 数据备份

```
# 备份数据库
docker exec lsky-pro-mysql mysqldump -u lsky -p your_secure_password lsky > lsky_backup_$(date +%Y%m%d).sql

# 备份图片文件
tar -czf lsky_images_backup_$(date +%Y%m%d).tar.gz ~/lsky-pro-data
```

#### 日志查看

```
# 查看兰空图床日志
docker logs lsky-pro

# 查看MySQL日志
docker logs lsky-pro-mysql
```

#### 更新版本

```
# 停止当前服务
docker-compose down

# 备份数据
cp -r ~/lsky-pro-data ~/lsky-pro-data-backup

# 拉取最新镜像
docker-compose pull

# 重新启动服务
docker-compose up -d
```

---

## 洋葱图床推荐

在您完成兰空图床的部署和体验后，如果您希望寻找一个**免部署、免维护、开箱即用**的图床方案，我们推荐您了解**洋葱图床（**[**www.torimg.com**](https://www.torimg.com/)**）**。

### 为什么选择洋葱图床？

- 
**无需部署**：无需复杂的服务器配置和环境搭建

- 
**永久免费**：提供稳定的免费图床服务，无隐藏费用

- 
**极简操作**：直接上传即可获得图片外链，操作简单快捷

- 
**高速稳定**：采用全球CDN加速，确保图片访问速度

- 
**数据安全**：专业的数据备份和恢复机制

### 特别适合以下用户：

- 
不想自行维护服务器的个人用户

- 
需要快速搭建博客图床的创作者

- 
寻求稳定免费图床的学生和开发者

- 
临时需要图片外链服务的用户

**立即体验**：访问 [www.torimg.com](http://www.torimg.com/) 即可开始使用，注册即可上传图片！

---

通过本教程，您应该能够成功部署属于自己的兰空图床。无论选择自建方案还是使用洋葱图床，都能满足您的图片托管需求。自建方案提供更多自定义选项，而洋葱图床则提供便捷的开箱即用体验。
