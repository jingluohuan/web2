# DNS 劫持 (缓存投毒) 实验

DNS 劫持介绍 (维基百科): [**https://zh.wikipedia.org/zh-cn/%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%BC%93%E5%AD%98%E6%B1%A1%E6%9F%93**](https://zh.wikipedia.org/zh-cn/%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%BC%93%E5%AD%98%E6%B1%A1%E6%9F%93)

简单来说就是，DNS 劫持就是攻击者通过某种手段，使得受害者的 DNS 明文请求被攻击者截获 (请求还没到达真实的 DNS 服务器)，然后攻击者返回错误的 DNS 解析结果，使得受害者访问错误的网站。

# 1. 实验环境

## 1.1. 网络拓扑

主机

地址

网关

10.0.6.2

kali

10.0.6.10

Win7

10.0.6.20

为简化拓扑，隐藏了网关设备:

[
![alt text](assets/png/articles/4cd7e1b19da7.webp)
](https://blog.lololowe.com/posts/546a/image%20(1).webp)

## 1.2. 劫持目标

域名

劫持后 IP

ccb.com

10.0.6.10

[**www.icbc.com.cn**](http://www.icbc.com.cn/)

10.0.6.10

修改 [**ettercap**](https://www.kali.org/tools/ettercap/) 的 DNS 劫持配置文件:

```
mkdir -p /etc/ettercap && touch /etc/ettercap/etter.dns
cat >> /etc/ettercap/etter.dns 

> 配置支持使用通配符，例如 `* A 10.0.6.10` 表示将所有域名劫持到 10.0.6.10。

## 1.3. 伪装站点

在 kali 上使用 nginx 启动一个 http 服务器，用于伪装被劫持的站点。

```
# 生成ssl自签证书
openssl genrsa -out /tmp/ca.key 2048
openssl req -x509 -new -nodes -key /tmp/ca.key -sha256 -days 36500 -out /tmp/ca.crt

# 添加nginx配置文件
cat > /etc/nginx/conf.d/dns_spoof.conf 

创建劫持站点的首页:

```
cat > /tmp/index.html 

    DNS劫持状态
    

    

# DNS劫持已生效

EOF
```

# 2. ARP 欺骗

DNS 劫持之前，需要先进行 ARP 网关欺骗，好让 Win7 与网关的通信都经过 kali（让 Win7 误以为网关是 kali）。

> 注意：即使 Win7 手动设置了 DNS 服务器为网关以外的 IP，也同样会被 ARP 欺骗影响，因为和 DNS 服务器通信也是需要靠网关转发的。

在 kali 上使用 root 权限运行 `ettercap -G`，打开[ **ettercap** ](https://www.kali.org/tools/ettercap/)的图形化界面。

如果提示 ettercap 不存在，则使用以下命令安装 ettercap:

```
sudo apt update
sudo apt install ettercap-common
sudo apt install ettercap-graphical
sudo apt install ettercap-text-only
```

[
![alt text](assets/png/articles/6344e5f84844.png)
](https://blog.lololowe.com/posts/546a/image-1.png)

在”Primary Interface” 菜单中选择 kali 实际使用的网卡，然后点击右上角的对勾图标开启嗅探。开启后再点击左上角的放大镜扫描主机，扫描完成后再点击放大镜右边的按钮，查看主机扫描列表:

[
![alt text](assets/png/articles/d90c4309a18b.png)
](https://blog.lololowe.com/posts/546a/image-2.png)

接着将扫描结果中的网关 (10.0.6.2) 添加到 Target1，Win7 (10.0.6.20) 添加到 Target2 中，然后点击右上角的地球图标，在展开的菜单中选择”ARP poisoning…”（弹窗直接点”OK”），:

[
![alt text](assets/png/articles/071c438a7df6.png)
](https://blog.lololowe.com/posts/546a/image-3.png)

> 如果需要修改 target，可以点击右上角的 3 个点图标，在展开的菜单中选择”Targets”，点击”Current targets” 进行管理。

此时，ettercap 已经成功进行 ARP 网关欺骗，Win7 会把网关的 mac 地址修改成 kali 的 mac 地址，可以在 Win7 上使用 `arp -a` 命令查看:

[
![alt text](assets/png/articles/5702d525b447.png)
](https://blog.lololowe.com/posts/546a/image-4.png)

> 

# 3. DNS 劫持

点击 ettercap 右上角三个点图标，在展开的菜单中选择”Plugins”，然后点击”Manage plugins，最后双击”dns_spoof” 开始劫持 DNS:

[
![alt text](assets/png/articles/2b61d5dd3f4c.png)
](https://blog.lololowe.com/posts/546a/image-5.png)

接着回到 Win7 中验证 DNS 劫持是否成功，使用以下命令查询 DNS 返回结果:

```
:: 刷新DNS缓存
ipconfig /flushdns
:: 查询域名
nslookup ccb.com
nslookup www.icbc.com.cn
:: 查看系统DNS缓存
ipconfig /displaydns
```

[
![alt text](assets/png/articles/043c18c07d3f.png)
](https://blog.lololowe.com/posts/546a/image-6.png)

可以看到两个域名的 DNS 解析 IP 都被篡改成了 10.0.6.10，即 kali 的 IP 地址。

再到浏览器中访问被劫持的站点 ccb.com 和 [**www.icbc.com.cn**](www.icbc.com.cn) ，会看到浏览器提示连接步安全，其中 ccb.com 可以忽略危险继续访问，而 [**www.icbc.com.cn** ](www.icbc.com.cn)则不行:

[
![alt text](assets/png/articles/d490b26c0afb.png)
](https://blog.lololowe.com/posts/546a/image-7.png)

造成这种现象的原因是，在 DNS 劫持之前正常访问过这两个网站，浏览器缓存了这两个网站的信息，因此默认会使用历史记录中的 HTTPS 发起连接，而 kali 上面生成的 ssl 自签证书是不受 CA 机构信任的，所以浏览器会提示连接不安全 (证书不可信)。

ccb.com 可以忽略警告继续访问，或者在地址栏中修改 HTTPS 为 HTTP，然后访问成功：

[
![alt text](assets/png/articles/bc7608b0bfdf.png)
](https://blog.lololowe.com/posts/546a/image-8.png)

而 [**www.icbc.com.cn**](www.icbc.com.cn) 由于开启了[ **HSTS**](https://blog.renfei.net/posts/1626402130325676117)，在 DNS 劫持之前就已经缓存了 HSTS 信息 (时长默认一年):

[
![alt text](assets/png/articles/9c42a3085962.png)
](https://blog.lololowe.com/posts/546a/image-12.png)

因此浏览器后续会强制使用 HTTPS 连接此网站并且不能忽略证书错误，因此访问失败。可以在谷歌浏览器地址栏输入 **chrome://net-internals/#hsts** 查看 HSTS 缓存信息:

[
![alt text](assets/png/articles/ab990d1866e2.png)
](https://blog.lololowe.com/posts/546a/image-9.png)

通过清除浏览器缓存可以去除 HSTS:

[
![alt text](assets/png/articles/4c2b30014b18.png)
](https://blog.lololowe.com/posts/546a/image-10.png)

[
![alt text](assets/png/articles/6596dbcff67a.png)
](https://blog.lololowe.com/posts/546a/image-11.png)

或者可以结合 NTP 时间攻击，将 Win7 的时间篡改到 1 年以后，使 HSTS 缓存过期，从而绕过 HSTS 的防护，但这并不在本实验的范围内。

# 4. 总结

- 
通过 ARP 欺骗，kali 主机成功伪装成网关，使 Win7 与网关之间的通信全部经过 kali。这为后续的 DNS 劫持奠定了基础。

- 
通过 DNS 劫持，成功将目标域名 ccb.com 和 [**www.icbc.com.cn**](www.icbc.com.cn) 的解析 IP 篡改为 kali 主机的 IP（10.0.6.10）。

- 
ccb.com 可以通过忽略警告或使用 HTTP 访问，而 [**www.icbc.com.cn 因 HSTS 限制无法直接绕过，体现了 HSTS 的安全作用。**](https://blog.lololowe.com/go.html?url=aHR0cDovL3d3dy5pY2JjLmNvbS5jbuWboGhzdHPpmZDliLbml6Dms5Xnm7TmjqXnu5Xov4cs5L2T546w5LqGaHN0c-eahOWuieWFqOS9nOeUqC4v)

- 
**防御措施**：

在公共网络下，为了防止 DNS 劫持，建议使用加密 DNS，例如 DoT（DNS over TLS）、DoH（DNS over HTTPS）；

- 
对于一些重要的网站，可以在 hosts 文件中静态绑定域名和 ip 的映射关系；

- 
https 服务器建议开启 HSTS，阻止证书错误时还能继续访问站点；

- 
Windows 建议时常使用 [**Sigcheck**](https://learn.microsoft.com/zh-cn/sysinternals/downloads/sigcheck) 工具来检查系统是否存在异常的 CA 根证书，防止基于证书的中间人攻击。

# 5. 参考与延伸

- 
[**https://blog.renfei.net/posts/1626402130325676117**](https://blog.renfei.net/posts/1626402130325676117)

- 
[**https://program-think.blogspot.com/2018/10/Comparison-of-DNS-Protocols.html**](https://program-think.blogspot.com/2018/10/Comparison-of-DNS-Protocols.html)

- 
[**https://imlonghao.com/41.html**](https://imlonghao.com/41.html)
