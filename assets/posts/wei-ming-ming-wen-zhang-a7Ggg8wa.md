# 使用cdnfly自建CDN并配置CC防护(更新了漏洞)

**更新：**

2023.3.30 修正了第三节”自建云端的错误”，伪装授权站需要一直存在

2023.4.02 修复了国内机器无法连接github的问题，即增加了国内机器可访问的安装脚本

2023.4.14 新增了cdnfly设置回源host实现反代的介绍

2023.4.24 修复了伪装站点监控失效的情况，更新了cdnfly的节点监控功能说明

2023.5.21 **cdnfly5.1.13存在重大漏洞，普通用户生成的API通过某些方法可以拿到管理员权限 点击此处查看如何设置**

2023.7.23 删除了国内机器安装脚本 请自行挂代理

2023.7.24 新增 备份和恢复 记录

2023.7.25 更新主控&节点安装命令

## 一、前言

本文记录一下使用 [Cdnfly – 自建cdn|防CC攻击|cdn软件|cdn系统] 搭建CDN系统，保姆级教程 适合小白食用

个人感觉`自建CDN`和`反代网站`效果都差不多，都可以实现加速 缓存 防攻击等功能，但我暂时还不是很懂nginx相关配置，相较而言 自建CDN更容易批量维护和管理各节点

官方安装文档：安装说明 · Cdnfly使用文档

## 二、准备

> 主控和被控均不能在 已安装nginx的情况下 执行安装命令，必须确保80 443端口未被占用!!!系统必须为centos7或ubuntu16.04 !!! debian11 ubuntu20 centos8 centos6等系统都不支持主控需开放80 88 443 9200端口节点需要开放 80 443 5000端口

- 
一台最小4GB内存的服务器(vps)做主控(cdn面板控制台)

- 
若干服务器做CDN节点

- 
系统：cdnfly主控和被控节点暂时仅支持Centos-7和Ubuntu 16.04系统

> 本文以一台centos7系统的hetzner 4GB内存vps `167.235.134.18` 做主控一台任何系统的vps `63.251.217.137` 搭建伪装授权站点，即cdnfly自建云端一台centos7系统的DO纽约 512MB内存vps `192.241.156.51` 做节点2 接管中国大陆移动以及海外等其他流量一台centos7系统的DO新加坡 512MB内存vps `157.245.154.23` 做节点2 接管中国大陆的电信和联通流量为什么用DO 主要是我就知道这一家有0.5GB小内存，虽然这款比一些4GB内存的都贵 但为了测试512MB内存可用 还是选择了DO为什么CDN节点机器用0.5GB内存小鸡 大部分廉价的国内优化机器只有0.5GB内存

## 三、cdnfly自建云端

> 有cdnfly官方授权的可跳过这一步，需要购买正版授权请前往 官网购买因为我就是临时个人测试使用，官方那个有些贵，就采用 ccclt 大佬分享的的 绕过授权方法盗版可耻 开发不易 有条件请用正版

**cdnfly自建云端搭建方法：**

用 [这个源码](https://www.wucuoym.com/goto/i5o4) 搭建一个站点，绑定域名auth.cdnfly.cn、monitor.cdnfly.cn ,并根据文件中的nginx伪静态配置设置伪静态。

建议自己搭建，不过也可以用我的，但是我的站点哪一天没了，你的机器会提示授权过期 到时候还是需要你自己搭建

我用宝塔搭建大概为这样：

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/0f2dcd9e0b2d.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/50461a431715.jpg)

我的只能使用http监控，tcp ping监控都无法正常工作，可能和宝塔有关

## 四、主控服务器设置

主控需要开放 80 88 443 9200端口，并且主控和节点机不能安装在同一机器上，会导致80端口冲突

主控若未开启9200 88端口 会导致程序大量占用CPU直至死机，如有任何问题 建议重置elasticsearch

#### 1、首先更新centos7源

1

yum update -y

#### 2、修改主控vps的hosts文件

修改主控vps的hosts文件，将 auth.cdnfly.cn、monitor.cdnfly.cn 这2个域名指向刚才的自建云端服务器IP `63.251.217.137`

修改hosts意义是 主控vps请求 `auth.cdnfly.cn` `monitor.cdnfly.cn` 这两个网站的内容会直接去 `63.251.217.137` 这个服务器请求，这样就绕过了官方服务器的验证

1
2
3
4
5
6
7
8
9
10
11
12

vi /etc/hosts

#添加以下内容，记得替换为你自己搭建的IP 你就用这个IP也行
#63.251.217.137 此IP属于DDP 该服务商暂停运营
#请将下文图片中的 63.251.217.137 替换为 51.81.222.246
51.81.222.246 auth.cdnfly.cn monitor.cdnfly.cn

#2023.6.17更新云端IP 此IP可以tcp监控
#51.81.222.246 auth.cdnfly.cn monitor.cdnfly.cn

#### 3、检查hosts是否生效

1
2
3

ping auth.cdnfly.cn
ping monitor.cdnfly.cn
#如果都返回 51.81.222.246 说明hosts已经生效，主控vps请求 `auth.cdnfly.cn` `monitor.cdnfly.cn` 这两个网站的内容会直接去 `51.81.222.246` 这个服务器请求，这样就绕过了官方服务器的验证

#### 4、安装cdnfly控制面板

这里参考 [Steady-WJ](https://www.wucuoym.com/goto/r0dl) 整理的

1
2
3

#直接在主控vps执行这条命令即可，控制面板占用3GB内存，内存不足会安装失败

curl -fsSL https://github.com/Steady-WJ/cdnfly-kaixin/raw/main/master.sh -o master.sh && chmod +x master.sh && ./master.sh –es-dir /home/es

出现下图页面即安装成功，通过 IP `http://167.235.134.18` 直接访问即可

> 如果出现报错等中止安装，大多数情况是你的系统不是centos7或ubuntu16.04，cdnfly开心版只支持centos7或ubuntu16.04系统，其它系统肯定无法正常运行

管理员账号和密码： wenjian/wenjian
普通用户账号和密码： ceshi/ceshi

> 5.1.12版本的主控有严重的 安全漏洞普通用户生成的API通过某些方法可以拿到管理员权限，请自行根据 此方法 修复

## 五、C
![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/adf6faccc823.jpg)用了两台做CDN节点，节点需要开放80 443 5000端口

一台centos7系统的DO纽约 512MB内存vps `192.241.156.51` 做节点2 接管中国大陆移动以及海外等其他流量

一台centos7系统的DO新加坡 512MB内存vps `157.245.154.23` 做节点2 接管中国大陆的电信和联通流量

以节点1 `192.241.156.51` 为例，节点2 `157.245.154.23` 同理

#### 1、更新centos7的源

1

yum update -y

#### 2、添加SWAP虚拟内存

> 1GB内存及以下的小鸡必须添加SWAP否则会报错，2GB以上内存的vps可自行决定是否添加SWAP

centos7添加SWAP我参考的 [这篇文章](https://www.wucuoym.com/goto/90f8)

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24

#先用 free -m 查看一下swap的大小

#1、使用dd命令创建/home/swap这么一个分区文件。文件的大小是1024000个block，一般情况下1个block为1K，所以这里空间是1000MB

dd if=/dev/zero of=/home/swap bs=1024 count=1024000

#2、接着再把这个分区变成swap分区

/sbin/mkswap /home/swap

#3、再接着使用这个swap分区。使其成为有效状态

/sbin/swapon /home/swap

#现在再用 free -m 命令查看一下内存和swap分区大小，就发现增加了512M的空间了

#4、修改/etc/fstab文件，在文件末尾增加如下一行，实现开机自动挂载
#vi /etc/fstab

/home/swap swap swap defaults 0 0

#### 3、安装被控服务

进入cd
![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/3d3ef0a94bda.jpg)

1
2
3
4
5
6
7

curl -fsSL https://github.com/Steady-WJ/cdnfly-
![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/a8ffefa447df.jpg)
y面板查看
###例如我执行的脚本为
curl -fsSL https://github.com/Steady-WJ/cdnfly-kaixin/raw/main/agent.sh -o agent.sh && chmod +x agent.sh && ./agent.sh –master-ver v5.1.13 –master-ip 167.235.134.18 –es-ip 167.235.134.18 –es-pwd OPpESBo9mF

下图即为安装成功，cdnfly被控服务会自己安装nginx等

#### 4、安装BBR

从下图就可以看出丢包对BBR的影响有多大

centos安装BBR我用的以下这条命令

1

wget “https://github.com/cx9208/bbrplus/raw/master/ok_bbrplus_centos.sh” && chmod +x ok_bbrplus_centos.sh && ./ok_bbrplus_centos.sh

参考：[从流量控制算法谈网络优化 – 从 CUBIC 到 BBRv2 算法](https://www.wucuoym.com/goto/7dye)

[TCP BBR – Exploring TCP congestion control (toonk.io)](https://www.wucuoym.com/goto/02fj)

#### 5、卸载节点

如果节点想换到另一台主控或者不想占用80 443 ，可以执行这条命令卸载节点

1

cd /tmp/ && curl -m 5 http://dl2.cdnfly.cn/cdnfly/agent_uninstall.sh -o agent_uninstall.sh || curl -m 5 http://us.centos.bz/cdnfly/agent_uninstall.sh && chmod +x agent_uninstall.sh && ./agent_uninstall.sh

## 六、cdnfly控制台必要配置

> 官方安装文档：安装说明 · Cdnfly使用文档

#### 1、添加CDN节点

即主控与被控服务建立连接，添加方法如下图

会提示先修改密码，修改密码在：系统管理>>用户管理

1GB内存小鸡通常会出现 `同步cc_filter nginx openresty` 以下报错，这是因为内存不够 解决办法是添加虚拟内存即可：[centos7添加SWAP](https://www.wucuoym.com/goto/uxjq)

部分服务商的centos7系统会报错 “重载resty配置失败：set-dict接口错误，可能节点未初始化成功，请尝试禁用启用节点”，一直是配置中，如果重启仍报错的话就是系统问题，这时候就需要 [自行dd centos7系统](https://www.wucuoym.com/goto/eba8)

#### 2、DNS设置

DNS设置可配置通过API来与第三方DNS提供商无缝对接，来生成网站的CNAME
目前支持aliyun，国内版dnspod，dns.com、dns.la 和cloudflare.com

> 获取密钥具体参考官方 DNS设置

#### 3、线路分组 设置分线路解析

> 只有 DNS服务商 支持分线路解析才可以设置分线路解析例如cloudflare这类国外DNS服务就不支持分线路解析

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/0b66d85bed39.jpg)

还可以为IP设置权重，权重越大用户访问该IP相对越多

#### 4、添加套餐

- 
名称 – 套餐名称，用户端也会显示这个

- 
描述 – 套餐的说明，也会显示到用户端的套餐购买列表

- 
分配给用户 – 输入用户的id，表示指定该套餐为该用户的专属套餐，只有这个用户能购买此套餐

- 
线路分组 – 上一步添加的线路组，或其它分组，决定网站绑定此套餐后网站配置分发到的节点，以及cname解析的IP

- 
套餐分组 – 为套餐分组，分组也会显示到用户端的套餐购买页面，方便归类购买

- 
CNAME域名 – 生成网站cname使用的域名，默认为之前dns设置中的主域名，可以输入其它的域名，但此域名必须跟主域名在同一个dns账号下。

- 
月流量 – G为单位，限制该套餐一个自然月内使用流量的上限

- 
域名数 – 该套餐允许的域名数量，域名数量包括裸域名及各级域名，[如www.cdnfly.cn](https://www.wucuoym.com/goto/kim6) cdnfly.cn算两域名

- 
HTTP非80,443端口数 – http可以输入非80,443的端口，此项可以限制非标端口的数量，不允许的话设置为0

- 
四层转发端口数 – 四层转发允许的端口数

- 
自定义CC规则 – 如果此项为允许，用户则可以创建自己的cc规则，并绑定到网站使用。如果为禁止，则用户只允许选择系统内置的规则

- 
排序 – 默认100，小值排在前面

- 
带宽为负值表示无限制

- 
连接数为负值表示不限制连接数

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/b4f49290e88c.jpg)

#### 5、添加网站

用管理员权限在 系统管理>>用户管理 创建一个用户 test

以 test 用户登录 然后购买免费的高级套餐，在网站管理>>分组管理>>新增分组后就可以添加网站了

#### 6、节点监控介绍

> 此节内容于2023.4.24补充，我的云端伪装站点只可以使用http监控，tcp ping监控均无法生效，目前不知到什么原因#2023.6.17更新 此IP可以tcp监控，看来是宝塔等问题#51.81.222.246 auth.cdnfly.cn monitor.cdnfly.cn

**cdnfly节点自动切换的规则：**

cdnfly的节点监控支持主节点宕机自动暂停 并删除该条DNS解析记录

如果设置了备用节点会用一个备用节点替换已失效的主节点 并同步DNS解析记录

当主节点再次恢复正常，cdnfly自动启用主节点并且取消使用备用节点，同时DNS会增加主节点DNS记录删去备用节点的解析记录

以上操作均为系统自动执行无需人工干预，可以选择配置SMTP发信服务，可以收到 节点失效/节点恢复/切换操作 等通知

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/9698a6f9788f.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/8b997d59edd7.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/d20e0ddc45ed.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/b0f532fff641.jpg)

#### 7、单节点设置

可以限制节点机器的文件缓存大小(不要超过vps硬盘本身大小)

可以设置节点出站速度（100Mbps=12.5MB/s）

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/61583227e7aa.jpg)

> 这里的代理设置应该不是指CDN节点到用户这段可以通过代理加速代理似乎也只支持http代理因为我给cdnfly节点添加代理后，我在云端监控(云端伪装站点)的服务器监听到了它与代理IP进行通信我猜测是：系统执行定时任务检查时，如果给CDN节点配置了代理，云端监控通过代理去访问CDN节点
> ![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/1c6ffcd6b1db.jpg)
> 
> ![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/7de4bcd9f6f7.jpg)
> 当http代理无效或非http代理时，还会报错，此时同步等功能会失效
> ![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/9f92ba41910b.jpg)
> 
> ![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/dc33ff94c2d7.jpg)

#### 8、禁用API(多用户必设置)(重要重要重要！！！)

**2023.5.21 cdnfly5.1.13存在重大漏洞，普通用户生成的API通过某些方法可以拿到管理员权限，解决方法就是禁用API，首先在 **`cdnfly控制台`** > **`系统管理`** > **`系统设置`** > **`用户相关`** > **`限制普通用户只能从此域名登录`** 和**`限制管理员只能从此域名登录`

*再依次在这两个登录域名设置 禁止 /v1/ 的所有访问，5.1.13版本的 *`/v1`* 路径是执行api的必要路径，封禁就可以避免被黑了**

**限制cdnfly从域名登录是避免从源站ip访问到了api**

**限制管理员是二次防火墙，如果黑客获取了权限，但是不知道你的管理员登录地址，他就只能对普通用户更改**

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/dc33ff94c2d7.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/dc33ff94c2d7.jpg)

## 七、cdnfly控制台推荐配置

#### 1、设置CC防护

这个根据自己需要设置就好了，还可以自定义CC规则

比如设置单IP 10s内最大访问 329.tanglu.cf 资源数为100，单个资源最大次数为20

> 这个防一般的单IP CC攻击很有效，推荐设置需要自己CC测压 可以参考 这篇记录

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/dc33ff94c2d7.jpg)

一些发卡站避免被机器人检查可以设置访问该域名需要点击验证

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/acb5ba397d97.jpg)

#### 2、设置CDN缓存文件类型

这个也是根据自己网站的资源类型，以下是我博客的设置

缓存设置建议参考官方文档：[缓存配置 · Cdnfly使用文档](https://www.wucuoym.com/goto/l4b1)

默认为不缓存任何文件

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/e46d8587e3b9.jpg)

#### 3、设置CDN节点服务器缓存大小和最大带宽

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/4f3531608687.jpg)

#### 4、申请证书

cdnfly的证书是自动续签的

cdnfly申请证书很简单有两种方法

一种是一键申请

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/47200500d970.jpg)

一种是在 `网站管理` > `证书管理` > `我的证书` 中申请证书

当然也可以上传自己的证书

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/b0654dd60b23.jpg)

如果没有申请成功，可以在 `我的网站` > `高级配置` 中开启 `/.well-known/acme-challenge/请求回源`

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/205764735ce6.jpg)

## 八、cdnfly控制台其他配置

#### 1、短信/邮箱提示

这里就是设置SMTP服务，QQ/gmail等都支持SMT发信，可以参考篇记录获取你QQ/gmail的SMTP专用密码：获取QQ/gmail等邮箱的SMTP账号密码

也可以用自己的域名邮箱发件，我在这里演示过使用scaleway的邮件推送服务：邮件推送

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/6b1bbd715fc7.jpg)

主控必须开放相应端口(25/465)才可以连接发件服务器从而正常发件，如果确认cdnfly的SMTP配置没问题但是cdnfly显示无法连接，可以检查主控服务器相应端口是否开放

#### 2、对接支付

需要支付功能请参考 [充值设置](https://www.wucuoym.com/goto/90bo)

#### 3、转发

转发容易封禁端口，不建议用CDN服务器做转发

#### 4、面板通过域名访问

直接套cloudflare，或者套其它CDN，也可以用其他机器反代 但感觉没必要

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/1566cf05102f.jpg)

#### 5、节点监控和宕机自动切换

需要配置SMTP节点超时会提示和开启宕机自动切换自动切换

#### 6、cdnfly设置反代

> 这个功能就是指定回源host，实现反代的功能

比如想给R2套第三方CDN就需要在第三方CDN配置回源host指向R2的绑定域名

举个指定CDN回源host，实现反代的功能的例子：

源站 blog.tanglu.me，现在用 blog.tang.lu 反代源站 ，如果直接给blog.tang.lu配置普通CDN CDN设置回源站点是 blog.tanglu.me，CDN解析出blog.tanglu.me的IP 2.2.2.2 然后发送http请求为 ‘’域名blog.tang.lu IP2.2.2.2’,因为源站点没有配置blog.tang.lu这个站点 访问就会报错。但是CDN设置指定回源HOST为blog.tanglu.me,CDN设置回源请求就是 ‘IP是2.2.2.2 回源域名是 blog.tanglu.me’,这样就可以成功访问了

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/0a73378741ee.jpg)

## 九、备份和恢复

cdnfly提供了非常简单的备份和恢复功能

#### 1、备份

主控默认会每两小时备份数据库，备份默认保留7天（可以自行修改保留天数），备份文件在 `/data/backup/cdn/`

除了备份数据库，还需要备份 `/opt/cdnfly/master/conf` 文件夹下的 `config.py` 文件(其实记住AES_KEY和LOG_PWD就行了)

恢复时只需要用到这两个文件，为了防止主控失联，建议每天上传备份

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/6a1eee65cb72.jpg)

每日自动上传备份

1
2
3
4

#我这里只是记录一下我的操作 可能不适合你
crontab -e

0 1,13 * * * rclone copy /data/backup/cdn/ r2:tanglu/cdnfly/backup/vps1

#### 2、恢复

此操作参考[cdnfly官网](https://www.wucuoym.com/goto/lxd0)，亲测有效

**旧主控：**

将旧主控 `/data/backup/cdn/` 下的某个时间的数据库备份包 如 `mysql-20230724-010931.sql.gz` 下载到本地，重命名为 `cdn.sql.gz`

将旧主控的 `/opt/cdnfly/master/conf/config.py` 文件也下载下来

关闭旧主控，这里记得关闭，如果不关闭的话新旧主控同时运行会出现后台任务只创建但不执行，导致新主控无法正常运行

**新主控：**

首先正常执行安装主控命令：

如果有需要自授权请先搭建云端 修改hosts

1

curl -fsSL https://github.com/Steady-WJ/cdnfly-kaixin/raw/main/master.sh -o master.sh && chmod +x master.sh && ./master.sh –es-dir /home/es

然后ssh连接新主控，恢复备份和config. py，初始化es

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23

#进入主控root目录
cd /root

#将上节备份的数据库文件 cdn.sql.gz 上传到root目录

#在root目录下执行以下两条命令 恢复数据库
curl http://us.centos.bz/cdnfly/restore_master.sh -o restore_master.sh && chmod +x restore_master.sh

./restore_master.sh

#将上节备份的旧主控 /opt/cdnfly/master/conf/config.py 上传新主控相同位置，实现替换掉新主控的config.py文件

#依次执行以下四条命令初始化elasticsearch
cd /tmp

wget us.centos.bz/cdnfly/int_es.sh -O int_es.sh && chmod +x int_es.sh

./int_es.sh /home/es

supervisorctl -c /opt/cdnfly/master/conf/supervisord.conf restart all

#至此主控设置完成，通过旧主控的账号密码进入可以发现 节点 用户 网站 日志 dns等都恢复了
#接下来只需要对旧节点更换IP即可

**旧节点**：

需要将旧节点的旧主控IP替换为新主控的IP

1
2
3
4
5
6

#依次在ssh登录每个节点并执行下面命令即可
#将 your_new_ip 替换为你自己的新主控IP

wget -qO change_ip.sh https://file.1323123.xyz/cdnfly/backup/shell/change_ip.sh && chmod +x change_ip.sh && bash change_ip.sh your_new_ip

**新节点：**

新节点安装方式和旧节点安装方式一样，只需要将旧主控IP修改为新主控IP 密码修改为旧主控密码即可

旧主控密码忘记了可以在 `/opt/cdnfly/master/conf/config.py` 可以找到

## 十、杂谈

① 你也可以像我博客一样将境外流量接入到cloudflare加速，国内走优化节点，具体教程可以参考 [cloudflare配置详解](https://www.wucuoym.com/goto/4wgu)

② [CC测压](https://www.wucuoym.com/goto/3ft7) 注意不要用源站机器去攻击，可能cdnfly为了回源请求成功 给源站IP加了白名单，我之前遇到这个问题纠结好久还以为是cdnfly规则有问题，最后才知道小丑是我自己…

③ 如果源站开启了防御记得将CDN加入白名单

④ 主控迁移可以参考官方 [FAQ](https://www.wucuoym.com/goto/ipw9)

⑤ cdnfly支持添加泛域名，只要待添加域名的DNS服务商支持泛域名解析即可

⑥ **cdnfly统计流量似乎只计算’CDN节点到用户(出站)’这个过程消耗的流量**，用户到CDN节点(入站)、CDN节点到源服务器(出站)、源服务器到CDN节点(入站)均不计入套餐流量。如果服务器为双向计费请注意设置价格
以下是我的测试记录：

我先通过套CDN在客户端跑测速(发包收包流量都大)，测得CDN入站流量 1362MB，出站总流量 371MB，CDN套餐显示流量消耗 233MB
我又通过客户端批量发包(发包流量小 收包流量大)，测得CDN入站流量 269MB，出站总流量 302MB，CDN套餐显示流量消耗 258MB

测试数据为：

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/50df672c06b6.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/f32a700414b3.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/883ac5dccb20.jpg)

![使用cdnfly自建CDN并配置CC防护(更新了漏洞)](assets/png/articles/0f3ff8458160.jpg)

⑦ 斯巴达等服务商提供的centos7系统无法安装，需要dd（新的斯巴达centos7镜像已支持）

服务器dd centos7的方法可以看我的这篇记录：[常见vps dd windows合集](https://www.wucuoym.com/goto/eba8)

⑧ 中文域名直接添加会报错，需要在 [punycoder.com](https://www.wucuoym.com/goto/699n) 将中文字符转换成 xn–sfj3b** 格式添加

⑨ 主控安装好后可以装宝塔吗？主控装完后会占用80 443端口，单纯装个宝塔是只占用8000端口，宝塔再装nginx会占用80端口，很明显你80端口需要给主控，所以你可以改一下宝塔nginx的端口 比如http改为8080 https改为4433，不建议新手尝试

⑩`Error： 添加节点失败:原因没有权限` `错误号： node-23` 应该是密码错了，请前往主控的 `系统管理` > `系统升级` 中查看密码

11、报错 `添加节点失败:原因没有权限 node-23` ,是因为节点密码没有换成自己的，去主控的系统升级查看

12、如果主控面板的任务可以自动添加，但是一直显示 `待执行` 那应该是你进行了主控迁移，将旧主控关机就好了

**免责声明**

本文部分内容整理或引用自公开的互联网资源。相关内容的原创权、著作权和商标权等知识产权均归原作者或合法权利人所有。

本站转载或引用旨在进行技术学习、交流与分享，**并非用于任何商业目的**。我们尊重知识产权，并致力于保护原创者的合法权益。
🛡️ **权利人如果认为本站内容存在侵权嫌疑，请通过以下方式与我们联系**，我们将立即核实并进行处理：

- 
**联系邮箱**：yunying@itxiaohui.top

我们在收到通知并核实后，将**七天内采取包括但不限于删除相关内容等措施**。

特此声明！
