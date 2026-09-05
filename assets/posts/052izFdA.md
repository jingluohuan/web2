# Nginx 报错码解析与解决方案

在使用 Nginx 构建网站或代理服务时，我们经常会遇到各种报错码。这些报错码通常以 HTTP 状态码形式呈现，它们不仅提示了请求是否成功，也帮助开发者快速定位问题。本文将系统梳理常见 Nginx 报错码、原因及对应的解决方案。

---

## 一、Nginx 常见客户端错误（4xx）

客户端错误通常表示请求有问题，服务器无法处理。常见的有：

### 1. 400 Bad Request

**原因：**
客户端请求语法错误，服务器无法理解。
**解决方案：**

- 
检查 URL 是否有非法字符

- 
检查请求头是否过大，Nginx 默认 `client_header_buffer_size` 与 `large_client_header_buffers` 有限制

```
server {
    client_max_body_size 10M; # 防止请求体过大
    client_header_buffer_size 1k;
    large_client_header_buffers 4 8k;
}
```

---

### 2. 401 Unauthorized

**原因：**
 请求未经授权，需要提供认证信息。
 **解决方案：**

- 
如果使用 HTTP Basic Auth，检查用户名密码是否正确

- 
检查认证配置是否覆盖了对应的 location

```
location /admin {
    auth_basic "Admin Area";
    auth_basic_user_file /etc/nginx/.htpasswd;
}
```

---

### 3. 403 Forbidden

**原因：**
 服务器理解请求，但拒绝执行。例如文件权限或 Nginx 配置限制。
 **解决方案：**

- 
检查目录或文件权限，确保 Nginx 用户可读

- 
检查 `deny` 或 `allow` 配置

- 
确保 `index` 文件存在

```
location / {
    index index.html;
    autoindex off;
}
```

---

### 4. 404 Not Found

**原因：**
 请求的资源不存在。
 **解决方案：**

- 
检查文件路径和根目录配置是否正确

- 
配置 `try_files` 避免无效请求

```
location / {
    try_files $uri $uri/ /index.html;
}
```

---

## 二、Nginx 常见服务器错误（5xx）

服务器错误表示请求是合法的，但服务器在处理时出现问题。

### 1. 500 Internal Server Error

**原因：**
 服务器内部错误，多出现在后端程序或配置错误。
 **解决方案：**

- 
检查 Nginx 错误日志 `/var/log/nginx/error.log`

- 
检查 PHP-FPM、Node.js 或其他后端服务是否启动

```
tail -f /var/log/nginx/error.log
systemctl status php-fpm
```

---

### 2. 502 Bad Gateway

**原因：**
 Nginx 作为反向代理时，无法从上游服务器获取有效响应。
 **解决方案：**

- 
检查上游服务是否启动

- 
检查端口是否正确

- 
调整 `proxy_read_timeout` 或 `fastcgi_read_timeout`

```
location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_connect_timeout 5s;
    proxy_read_timeout 30s;
}
```

---

### 3. 503 Service Unavailable

**原因：**
 服务器暂时无法处理请求，可能是服务停止或超载。
 **解决方案：**

- 
检查上游服务状态

- 
如果使用 PHP-FPM，确保进程池未满

- 
可配置 Nginx 返回自定义维护页面

```
error_page 503 /maintenance.html;
location = /maintenance.html {
    root /usr/share/nginx/html;
}
```

---

### 4. 504 Gateway Timeout

**原因：**
 上游服务器无响应或响应超时。
 **解决方案：**

- 
检查后端服务健康状态

- 
增加 Nginx 超时时间

```
proxy_connect_timeout 10s;
proxy_read_timeout 60s;
```

---

## 三、Nginx 错误排查技巧

- 
**查看错误日志**

```
tail -f /var/log/nginx/error.log
```

- 
**检查访问日志**

```
tail -f /var/log/nginx/access.log
```

- 
**测试 Nginx 配置**

```
nginx -t
systemctl reload nginx
```

- 
**使用 curl 或浏览器调试**

```
curl -I http://yourdomain.com/path
```

---

## 四、总结

Nginx 错误码不仅是问题的信号，也是排查问题的重要工具。

- 
4xx 错误一般是客户端请求问题

- 
5xx 错误一般是服务器配置或后端服务问题

通过查看日志、调整配置、排查后端服务，大多数 Nginx 错误都可以快速定位并解决。
