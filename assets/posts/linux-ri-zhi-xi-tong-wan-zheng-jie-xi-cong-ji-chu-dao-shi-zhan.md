# Linux 日志系统完整解析：从基础到实战

在服务器环境中，日志是最重要的调试与运维依据之一。无论是查找故障、分析访问趋势还是监控系统状态，日志系统都发挥着关键作用。Linux 提供了一套灵活且强大的日志机制，包括系统日志、服务日志以及常见的应用程序日志。本文将从结构、工具到实践案例，系统介绍 Linux 的日志体系。

## 一、Linux 日志的分类

在 Linux 中，日志大体可分为以下几类：

- 
内核与系统日志

- 
服务与守护进程日志

- 
应用程序日志

- 
安全与认证日志

- 
自定义日志（开发者自行输出）

这些日志大多集中在 `/var/log` 目录下，并按功能分类存放。例如：

- 
`/var/log/syslog`：综合系统信息

- 
`/var/log/auth.log`：用户登录与权限相关

- 
`/var/log/kern.log`：内核相关信息

- 
`/var/log/nginx/`：Nginx 日志

- 
`/var/log/mysql/`：MySQL 日志

理解目录结构是学习日志系统的第一步。

## 二、rsyslog：Linux 默认日志系统

大多数 Linux 发行版采用 rsyslog 作为默认日志管理服务。
它的职责包括：

- 
接收系统与应用程序输出的日志

- 
按规则分类

- 
输出到文件、网络或数据库

- 
支持日志格式化与过滤

配置文件通常位于：

```
/etc/rsyslog.conf
/etc/rsyslog.d/*.conf

```

### rsyslog 的日志级别

从高到低依次为：

- 
emerg（系统不可用）

- 
alert

- 
crit

- 
err

- 
warn

- 
notice

- 
info

- 
debug

例如以下规则表示将错误级别及以上日志写入指定文件：

```
*.err /var/log/error.log

```

这套语法类似过滤器，用于将不同来源的日志分类写入不同文件。

## 三、journalctl：systemd 的日志管理工具

使用 systemd 的 Linux 发行版（如 Ubuntu、CentOS 7+）会使用 `journal` 保存服务日志。
`journalctl` 是查看和管理日志的主要工具。

常用命令包括：

查看全部日志：

```
journalctl

```

查看实时日志：

```
journalctl -f

```

按服务查看日志：

```
journalctl -u nginx

```

查看指定时间段：

```
journalctl --since "2024-01-01" --until "2024-01-02"

```

按优先级过滤：

```
journalctl -p err

```

journalctl 可以直接读取 systemd 管理的服务输出，是排查服务启动失败、崩溃和异常行为的核心工具。

## 四、应用日志：以 Nginx 为例

多数应用程序会将日志独立存放在自己的目录中。例如 Nginx：

```
/var/log/nginx/access.log
/var/log/nginx/error.log

```

- 
access.log 包含请求来源、响应状态、耗时等信息

- 
error.log 用于定位程序内部错误

Nginx 日志格式可以在配置文件中自定义：

```
log_format main '$remote_addr - $remote_user [$time_local] '
                '"$request" $status $body_bytes_sent '
                '"$http_referer" "$http_user_agent"';

access_log /var/log/nginx/access.log main;

```

配置合适的日志格式对于分析访问量、性能瓶颈与攻击行为非常重要。

## 五、日志轮换（logrotate）

随着系统长期运行，日志文件会不断变大。logrotate 用于定期压缩、分割日志，防止占用过多磁盘空间。

配置目录为：

```
/etc/logrotate.conf
/etc/logrotate.d/

```

示例配置：

```
/var/log/nginx/*.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
}

```

含义：

- 
每周轮换

- 
保留 4 个历史文件

- 
使用 gzip 压缩

- 
如果文件不存在则跳过

- 
空文件不处理

logrotate 是保持系统长期稳定的重要组件。

## 六、日志分析实践

下面给出几个典型的日志排查场景。

### 1. 服务器 CPU 占用异常

查看内核与系统日志：

```
journalctl -p err -b

```

可能定位到某个进程频繁崩溃或硬件异常。

### 2. 被大量恶意扫描

Nginx access.log 会出现集中访问 `/wp-login.php`、`/phpmyadmin/` 的记录。可以用命令过滤：

```
grep "wp-login.php" /var/log/nginx/access.log

```

结合 fail2ban 可实现自动封禁。

### 3. 登录暴力破解

查看 auth.log：

```
grep "Failed password" /var/log/auth.log

```

若短时间内大量失败说明正在遭受暴力破解。

### 4. 服务启动失败

直接查看服务日志：

```
journalctl -u your-service -xe

```

可以看到具体错误信息，例如端口冲突、权限拒绝等。

## 七、总结

Linux 提供了完整的日志体系，通过系统日志、服务日志与应用程序日志配合，可以较全面地掌握系统运行情况。掌握 rsyslog、journalctl、logrotate 等工具是服务器运维与开发者的必备基础。
良好的日志管理不仅用于故障排查，还能帮助优化性能、提升安全性、分析访问趋势，是构建稳定系统的重要环节。
