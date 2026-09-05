# Cloudflare 源站 15 年 SSL 证书详解：到底有什么用？该不该开？

在使用 [Cloudflare](https://www.cloudflare.com/zh-cn/?utm_source=chatgpt.com) 的过程中，很多人第一次进入「SSL/TLS → 源服务器」页面时，都会看到一个非常离谱的东西：

> “创建一个有效期长达 15 年的源站证书（Origin Certificate）”

很多人第一反应：

- 
？？？

- 
正常 SSL 不都是 90 天或者 1 年吗？

- 
15 年不会不安全吗？

- 
浏览器能直接信任吗？

- 
这玩意到底是干嘛的？

今天就来详细讲一下 Cloudflare 的“15 年源站 SSL 证书”到底是什么，以及它适合什么场景。

---

# 一、什么是 Cloudflare 源站证书？

Cloudflare 的源站证书（Origin Certificate）本质上：

> 是 Cloudflare 专门给你的服务器使用的“内部 SSL 证书”。

它并不是给用户浏览器直接访问的。

真正访问你网站的用户：

```
用户浏览器
   ↓
Cloudflare CDN
   ↓
你的源服务器
```

浏览器信任的是：

- 
Let’s Encrypt

- 
DigiCert

- 
GlobalSign

这些公网 CA 颁发的证书。

而 Cloudflare Origin Certificate：

- 
只给 Cloudflare 与你的服务器通信使用

- 
浏览器并不会直接信任

- 
如果绕过 Cloudflare 直接访问源站，会提示不安全

---

# 二、为什么 Cloudflare 可以发 15 年？

因为它不是公网证书。

传统 HTTPS 证书：

- 
属于公开互联网信任体系

- 
需要严格限制有效期

- 
防止泄露长期滥用

而 Cloudflare 源站证书：

- 
只在 Cloudflare ↔ 服务器之间使用

- 
不参与浏览器信任链

- 
属于私有信任体系

所以 Cloudflare 可以直接给：

- 
10 年

- 
15 年

这种超长有效期。

官方文档也明确说明：

> Origin CA certificates are only trusted by Cloudflare.

---

# 三、Cloudflare 源站 SSL 的工作原理

正常 HTTPS：

```
浏览器 ←→ 服务器
```

Cloudflare CDN：

```
浏览器 ←→ Cloudflare ←→ 源站
```

这里实际上有两层 SSL：

## 第一层：用户到 Cloudflare

这一层：

- 
使用公网可信证书

- 
浏览器直接信任

- 
用户看到小锁

通常由 Cloudflare 自动管理。

---

## 第二层：Cloudflare 到你的服务器

这一层：

- 
用于保护回源流量

- 
防止机房内部被监听

- 
使用 Origin Certificate

也就是 15 年证书存在的地方。

---

# 四、为什么必须开 Full（Strict）

很多新手配置 Cloudflare 时：

直接开 Flexible SSL。

结果：

- 
死循环

- 
HTTPS 错误

- 
WordPress 无限跳转

- 
登录失效

因为：

```
用户 → HTTPS → Cloudflare
Cloudflare → HTTP → 源站
```

回源没有加密。

正确做法：

```
SSL/TLS → Full（Strict）
```

这样：

```
用户 → HTTPS → Cloudflare → HTTPS → 源站
```

整个链路才是真 HTTPS。

而 Full（Strict）模式：

必须要求源站有有效 SSL 证书。

这时候：

Cloudflare Origin Certificate 就派上用场了。

---

# 五、Origin Certificate 的优点

## 1. 超长有效期

15 年基本不用管。

不用：

- 
自动续签

- 
Certbot

- 
acme.sh

- 
定时任务

对很多小站长非常舒服。

---

## 2. 配置简单

Cloudflare 一键生成：

- 
证书

- 
私钥

直接复制即可。

Nginx：

```
ssl_certificate /etc/nginx/cert.pem;
ssl_certificate_key /etc/nginx/key.pem;
```

即可完成配置。

---

## 3. 防止源站被监听

即使：

- 
CDN 节点

- 
机房网络

- 
回源线路

被抓包。

数据依然是加密的。

---

## 4. 配合源站防火墙效果更好

很多人会：

- 
只允许 Cloudflare IP 访问

- 
屏蔽直接源站连接

这样：

```
只有 Cloudflare 能访问服务器
```

Origin Certificate 就非常适合这种模式。

---

# 六、它的缺点是什么？

## 1. 浏览器不信任

如果别人：

```
直接访问你的服务器IP
```

或者：

```
origin.example.com
```

没有经过 Cloudflare。

会看到：

```
NET::ERR_CERT_AUTHORITY_INVALID
```

因为浏览器不认识 Cloudflare Origin CA。

---

## 2. 不能替代公网证书

它不能：

- 
用于 API 公网服务

- 
用于邮件服务器

- 
用于直接 HTTPS 暴露

- 
用于客户端直连

它只是：

> Cloudflare 内部专用 SSL。

---

## 3. 一旦泄露风险时间更长

因为有效期 15 年。

如果：

- 
私钥泄露

- 
服务器被入侵

攻击者理论上能长期利用。

所以：

- 
权限一定要限制

- 
私钥别乱传

- 
不要打包到镜像里

---

# 七、适合哪些人？

非常适合：

## 小型博客

比如：

- 
Halo

- 
WordPress

- 
Typecho

---

## CDN 全站代理用户

只要：

```
所有流量都必须经过 Cloudflare
```

那 Origin Certificate 就很好用。

---

## 懒得折腾自动续签的人

尤其：

- 
PVE

- 
Docker

- 
多节点

- 
NAT VPS

有时候自动续签确实麻烦。

15 年证书真的省事。

---

# 八、Nginx 配置示例

Cloudflare 生成证书后：

## Nginx

```
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/nginx/cf.pem;
    ssl_certificate_key /etc/nginx/cf.key;

    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}
```

---

# 九、推荐的最佳实践

推荐组合：

```
Cloudflare CDN
+ Full (Strict)
+ Origin Certificate
+ 仅允许 Cloudflare IP
```

这样：

- 
安全性高

- 
不怕源站泄露

- 
不用频繁续签

- 
HTTPS 完整加密

属于目前很多博客与个人站长的主流方案。

---

# 十、总结

Cloudflare 的 15 年 SSL 证书：

并不是传统公网 HTTPS 证书。

它本质上是：

> Cloudflare 与源站之间的专用加密证书。

它最大的特点：

- 
有效期超长

- 
配置简单

- 
非公网信任

- 
专用于 CDN 回源

如果你的网站：

- 
全站挂 Cloudflare

- 
不允许绕过 CDN

- 
想减少 SSL 运维

那么：

Cloudflare Origin Certificate 确实非常香。

但如果你需要：

- 
用户直连源站

- 
公网 API

- 
邮件服务

- 
多客户端兼容

那还是建议使用：

- 
Let’s Encrypt

- 
ZeroSSL

- 
DigiCert

等公网可信 CA。
