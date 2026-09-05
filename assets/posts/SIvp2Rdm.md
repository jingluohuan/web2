# CloudFlare 白嫖 OSS 流量秘籍

# 带宽联盟

**Cloudflare 带宽联盟（Bandwidth Alliance）** 是一个由 Cloudflare 发起的合作计划，联合多家云服务商和网络公司，目的是**降低甚至免除数据传输（带宽 / 出站流量）费用**具体可以查看官方页[**Cloudflare云服务_数据传输_高速云数据传输服务_|Cloudflare中国官网 | Cloudflare**](https://www.cloudflare-cn.com/bandwidth-alliance/)

阿里云关于带宽联盟免流的回应

![阿里云对于免流](assets/png/articles/46a817169500.png)

## 实测

本文耗费两台服务器一台美国，一天香港，耗时约1天，测试结果cloudflare 800G流量，oss储存桶700+，未扣费也无账单。

可以放心开嫖。

下面看图

![](assets/png/articles/b47153ba9a13.jpg)

在凌晨开刷

![](assets/png/articles/7ac7dafdfdbd.jpg)

等结果，就是下面的，时间过去1天，阿里并没有任何的账单。

![](assets/png/articles/c6a77f1a0ee7.jpg)

![](assets/png/articles/8f864cb438eb.webp)

cf 800G流量有700多G都回源，也没有产生账单。

注意：只针对阿里oss海外地域的储存桶

## 正文

本文章教你使用cloudflare的带宽联盟免去oss香港地区的流量费用，再也不用担心第二天起来破产了。

看一下效果图

![Oss流量](assets/png/articles/93eb1a02e61e.png)

这么多流量，都没有流量费的，我们来配置一下。

### 准备工作

1.一个阿里云香港地区的储存桶，公共读权限，配置跨域。

2.一个在CloudFlare上已经托管的域名

### OSS控制台

首先创建一个储存桶，来到阿里OSS控制台

![创建bucket](assets/png/articles/e2ec70fa3b15.png)

![](assets/png/articles/c8d29ee95d9e.png)

创建完成后进入bucket，来到权限控制，将阻止公共访问关闭

![](assets/png/articles/ecc85f09fa0a.png)

在来到读写权限，设置为公共读

![权限设置](assets/png/articles/fde0eea39ef5.png)

在来到下方数据安全中的跨域设置，设置跨域

![跨域设置](assets/png/articles/65571f1c17ab.png)

在来到域名管理绑定自己的自定义域名

![域名绑定](assets/png/articles/bb265bf9a1c3.png)

### CloudFlare解析

然后来到CloudFlare设置域名解析，必须开启小黄云

![域名解析](assets/png/articles/688561173f12.png)

### SSL绑定

记得申请一张SSL证书，绑定在oss控制台

![域名绑定](assets/png/articles/bce296b389ad.png)

### 测试

下面上传文件测试即可

### 进阶白嫖方法

目前CloudFlare已经成功获取到了oss文件，但是CloudFlare由于一些原因在国内访问并不流畅，所以我们可以使用阿里esa加速CloudFlare的域名，在通过Dns全球分流实现全球的免流。仅有储存费用和访问费用。可以自己去配置。

看图

![](assets/png/articles/d2f4df490b9f.png)

**本文由“**[**慕托小记**](https://www.tortb.com/)**”授权发布，原文链接：《**[**CloudFlare 带宽联盟免阿里 OSS 流量教程**](https://www.tortb.com/posts/freeoss/)**》**

[https://chata.itxiaohui.top/10002](https://chata.itxiaohui.top/10002)
