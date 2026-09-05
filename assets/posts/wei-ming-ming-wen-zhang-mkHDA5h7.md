# 1Panel 建站面板介绍：一键部署 WordPress 网站，支持容器管理

今天看到腾讯云后台重装系统的时候多了一个 1Panel 的选项，之前好像没看到过，简单搜了一下，记录如下。

1Panel 是杭州飞致云信息科技有限公司旗下产品，是一款现代化、开源的 Linux 服务器运维管理面板，于 2023 年 3 月推出。

- 
名称：1Panel 开源 Linux 面板

- 
所属公司：杭州飞致云信息科技有限公司

- 
编写语言：Golang

- 
上线时间：2023年3月20日

**文章目录** [隐藏](https://oldtang.com/14063.html#)

[1Panel 功能特点](https://oldtang.com/14063.html#1Panel_%E5%8A%9F%E8%83%BD%E7%89%B9%E7%82%B9)

[1Panel 安装教程](https://oldtang.com/14063.html#1Panel_%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B)

## 1Panel 功能特点

- 
快速建站：深度集成 WordPress 和 Halo，一键完成域名绑定、SSL 证书配置等操作，帮助用户实现快速建站。

- 
服务器管理：支持用户通过 Web 浏览器轻松管理 Linux 服务器，包括应用管理、主机监控、文件管理、数据库管理、容器管理等。

- 
安全审计：提供防火墙和安全审计等功能。

- 
一键备份：支持一键备份和恢复，备份数据可在云端存储。1Panel 支持添加本地服务器磁盘和第三方账号，支持的第三方账号包括阿里云 OSS、AWS S3 云存储、MinIO 云原生对象存储和 SFTP 文件传输协议。

官网地址：[https://1panel.cn](https://1panel.cn/)

开源地址：[https://github.com/1Panel-dev/1Panel](https://github.com/1Panel-dev/1Panel)

## 1Panel 安装教程

教程地址：[https://1panel.cn/docs/installation/online_installation/](https://1panel.cn/docs/installation/online_installation/)

针对 Ubuntu 系列服务器的安装命令：

```
curl -sSL https://resource.fit2cloud.com/1panel/package/quick_start.sh -o quick_start.sh && sudo bash quick_start.sh
```

安装成功后，控制台会打印面板访问信息，可通过浏览器访问 1Panel：

```
http://目标服务器 IP 地址:目标端口/安全入口
```

如果使用的是云服务器，请至安全组开放目标端口。

ssh 登录 1Panel 服务器后，执行 `1pctl user-info` 命令可获取安全入口（entrance）。

安装成功后，可使用 `1pctl` 命令行工具来维护 1Panel。
