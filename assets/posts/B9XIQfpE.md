# 为什么你的 Nginx 配置了 HTTPS，网站依然显示“不安全”？

很多站长在部署网站时，第一件事就是申请 SSL 证书并开启 HTTPS。

看着浏览器地址栏上的小锁图标，似乎一切都已经安全了。

然而实际情况却并非如此。

有时候明明已经成功配置 HTTPS，浏览器依然提示：

- 
连接不完全安全

- 
存在混合内容（Mixed Content）

- 
部分资源未加密

- 
网站不安全

这究竟是什么原因？

今天就来聊聊这个很多新手站长都会踩到的坑。

## HTTPS 不等于绝对安全

很多人认为：

```
安装 SSL 证书 = 网站安全
```

实际上：

```
SSL 证书 ≠ 网站完全安全
```

HTTPS 只能保证客户端与服务器之间的数据传输被加密。

如果网页内部仍然加载 HTTP 资源，那么浏览器依然会发出安全警告。

例如：

```

- 
```

这些资源通过 HTTP 加载。

即使页面本身是 HTTPS，也会触发 Mixed Content（混合内容）问题。

---

## 如何检查混合内容？

以 Chrome 浏览器为例：

按下：

```
F12
```

打开开发者工具。

切换到：

```
Console
```

如果存在问题，通常会看到类似提示：

```
Mixed Content:
The page at 'https://example.com'
was loaded over HTTPS,
but requested an insecure resource
'http://example.com/logo.png'
```

说明页面引用了未加密资源。

---

## 常见问题一：数据库中的旧链接

这是 WordPress、Halo、Typecho 等 CMS 最常见的问题。

例如数据库里保存着：

```
http://example.com/image.jpg
```

即使网站已经开启 HTTPS。

页面渲染后依然会输出：

```

```

解决方法：

批量替换数据库内容。

例如 MySQL：

```
UPDATE post
SET content = REPLACE(
content,
'http://example.com',
'https://example.com'
);
```

执行前务必备份数据库。

---

## 常见问题二：第三方资源未启用 HTTPS

有些资源站仍然只提供 HTTP。

例如：

```

```

浏览器会直接阻止加载。

解决方案：

优先寻找支持 HTTPS 的资源地址。

例如：

```

```

如果第三方不支持 HTTPS。

建议直接更换服务商。

---

## 常见问题三：图片链接仍是 HTTP

很多老站迁移 HTTPS 后。

文章中的图片仍然保持原来的链接。

例如：

```
![](http://img.example.com/test.jpg)
```

这也是混合内容最常见来源之一。

解决方法：

批量替换文章内容

- 
使用 HTTPS 图床

- 
开启 CDN HTTPS

---

## 常见问题四：CDN 配置错误

部分站长会遇到：

```
用户 → HTTPS → CDN
CDN → HTTP → 源站
```

如果 CDN 设置不正确。

可能导致：

- 
循环跳转

- 
浏览器报错

- 
SSL 失效

建议开启：

```
Full SSL
```

或：

```
Strict SSL
```

确保全链路加密。

---

## 开启 HSTS

解决混合内容后。

还可以进一步提高安全性。

在 Nginx 中添加：

```
add_header Strict-Transport-Security
"max-age=31536000; includeSubDomains"
always;
```

作用：

- 
强制浏览器使用 HTTPS

- 
防止 SSL Strip 攻击

- 
提升网站安全等级

重载配置：

```
nginx -t
systemctl reload nginx
```

即可生效。

---

## 如何验证配置是否正确？

推荐使用以下方式检测：

### 浏览器开发者工具

查看：

```
Console
```

是否存在 Mixed Content 报错。

### SSL Labs

检测：

- 
SSL 等级

- 
TLS 配置

- 
证书链完整性

### Security Headers

检测：

- 
HSTS

- 
CSP

- 
安全响应头

---

## 总结

网站开启 HTTPS 后仍然显示不安全。

绝大多数情况并不是 SSL 证书的问题。

而是：

- 
页面存在 HTTP 资源

- 
数据库保存旧链接

- 
第三方资源未启用 HTTPS

- 
CDN SSL 配置错误

很多站长折腾了半天证书。

最后发现问题只是文章里的一张图片还在使用 HTTP 地址。

因此在排查 HTTPS 问题时。

建议优先检查浏览器控制台输出。

通常几分钟就能找到真正原因。

希望本文能够帮助你少踩几个坑。
