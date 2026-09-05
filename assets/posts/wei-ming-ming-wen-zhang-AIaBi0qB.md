# 公网IP地址是什么？如何查看自己的公网IP地址？

## 什么是公网IP地址？

IP 地址是互联网上的设备的标识符。它是一个数字，用于标识网络上的设备。IP 地址分为公网 IP 地址和私有 IP 地址。

## 如何查看自己的公网IP地址？

如果你装宽带的时候，你的宽带运营商会给你一个公网IP地址。你就可以直接运行 `ipconfig` 命令来查看你的公网IP地址。

但公网IP地址数量有限，许多家庭和企业网络使用网络地址转换（NAT）技术，将多个私有IP地址（局域网内的IP地址）映射到一个公网IP地址上，以实现多台设备共享一个公网IP地址上网。

这个时候，IPConfig 命令只能查看到你的私有IP地址，无法查看到你的公网IP地址。所以，这个时候，你往往要访问一个外部服务来查看你的公网IP地址。**就像你忘记了你的电话号码，你可以打电话给你的朋友，让他告诉你你的手机号码。**

这类服务有很多，往往是一个简单的网页，你访问这个网页，它会告诉你你的公网IP地址。比如：

- 
[What is my public IP address - IP.ME](https://ip.me/)

- 
[What Is My IP Address - See Your Public Address - IPv4 & IPv6](https://whatismyipaddress.com/)

- 
[Check your IP address | MyIP.com](https://www.myip.com/)

- 
[What Is My IP? Best Way To Check Your Public IP Address](https://www.whatismyip.com/)

- 
[What Is My IP | Find my Public IP Address - IPv4 & IPv6](https://www.whatismypublicip.com/)

- 
[我的 IP 位址為何：搜尋公共的 IPv4 和 IPv6 | NordVPN](https://nordvpn.com/zh/what-is-my-ip/)

- 
[万网获取本地公网IP地址](http://www.net.cn/static/customercare/yourip.asp)

### 使用 curl 命令查看公网IP地址

你也可以在命令行中使用 `curl` 命令来查看你的公网IP地址：

```
curl ifconfig.me

```

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

本质上，也是访问 [What Is My IP Address? - ifconfig.me](https://ifconfig.me/) 这个网站。

## 构建简单的公网IP地址查询服务

如果你想自己构建一个简单的公网IP地址查询服务，你可以使用 Node.js 来构建一个简单的 Web 服务器，然后返回客户端的 IP 地址。

```
const http = require('http');

// 创建 HTTP 服务器
const server = http.createServer((req, res) => {
  // 获取客户端的 IP 地址
  const clientIp = req.socket.remoteAddress;

  // 设置响应头
  res.writeHead(200, { 'Content-Type': 'text/plain' });

  // 返回客户端的 IP 地址
  res.end(`Your IP address is ${clientIp}`);
});

// 服务器监听端口 3000
server.listen(3000, () => {
  console.log('Server is running at http://localhost:3000');
});

```

### 运行代码

- 
将上述代码保存到一个文件中，例如 `server.js`。

- 
打开终端，导航到保存文件的目录。

- 
运行以下命令启动服务器：

```
node server.js

```

- 
打开浏览器，访问 `http://localhost:3000`，你将看到返回的客户端 IP 地址。

这样，你就可以使用 Node.js 构建一个简单的 Web 服务器，并返回客户端的 IP 地址。

### 参考资料

- 
[curl 的用法指南 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2019/09/curl-reference.html)
