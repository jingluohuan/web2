# SSO vs OAuth2：登录和授权到底有什么区别？

# SSO 和 OAuth2 的区别：很多人以为是同一个东西，其实完全不同

在现代互联网系统中，经常会听到 **SSO（单点登录）** 和 **OAuth2（开放授权协议）** 这两个词。

很多开发者会认为：

> “OAuth2 不就是实现单点登录的吗？”

这个理解并不完全正确。

虽然 SSO 和 OAuth2 经常一起出现，但它们解决的问题不同：

- 
**SSO 解决的是：用户登录问题**

- 
**OAuth2 解决的是：授权访问问题**

本文将详细介绍两者的区别，以及它们在实际项目中的应用。

---

## 一、什么是 SSO？

SSO（Single Sign-On，单点登录）指的是：

> 用户只需要登录一次，就可以访问多个相互信任的系统。

例如：

你登录了公司内部账号：

flowchart TD
    A[用户访问系统A] --> B{是否已登录}
    B -- 否 --> C[跳转认证中心]
    C --> D[用户输入账号密码]
    D --> E[认证服务器验证身份]
    E --> F[返回登录凭证 Ticket / Token]
    F --> G[系统A验证凭证]
    G --> H[登录成功]

    B -- 是 --> H

    H --> I[访问其他系统]
    I --> J[系统B]
    I --> K[系统C]
    I --> L[系统D]

    style C fill:#4a90e2,color:white
    style E fill:#4a90e2,color:white
    style H fill:#67c23a,color:white

登录一次后：

- 
打开 OA，不需要重新输入密码

- 
打开代码管理平台，不需要再次登录

- 
打开内部管理系统，也无需重新认证

这就是单点登录。

---

## 二、SSO 的核心目标

SSO 主要解决：

### 1. 减少重复登录

传统系统：

```
用户
 |
 +-- 网站A 登录
 |
 +-- 网站B 登录
 |
 +-- 网站C 登录
```

每个系统都有自己的账号体系。

用户需要：

- 
多次输入账号密码

- 
记忆多个密码

- 
管理多个 Session

SSO：

```
              认证中心
                 |
        ------------------
        |        |        |
      网站A    网站B    网站C
```

用户只登录认证中心一次。

---

## 三、SSO 常见实现方式

### 1. Cookie 共享

早期企业内部系统经常使用。

例如：

```
a.example.com
b.example.com
```

如果两个系统共享：

```
.example.com
```

域名 Cookie：

```
token=xxxx
```

那么两个系统可以共享登录状态。

缺点：

- 
安全性较低

- 
不适合跨域

- 
不适合大型系统

---

### 2. CAS 单点登录

CAS 是经典 SSO 协议。

流程：

```
用户访问系统A

        ↓

系统A发现没有登录

        ↓

跳转认证服务器

        ↓

用户登录

        ↓

认证服务器返回 Ticket

        ↓

系统A验证 Ticket

        ↓

登录成功
```

很多高校、企业内部系统仍然使用 CAS。

---

### 3. SAML SSO

SAML（Security Assertion Markup Language）主要用于企业级身份认证。

例如：

- 
企业 SaaS

- 
企业员工登录

- 
云服务平台

典型：

```
企业身份中心

        ↓

SAML Assertion

        ↓

第三方服务
```

---

# 四、什么是 OAuth2？

OAuth2 全称：

> Open Authorization 2.0

中文：

> 开放授权协议

它解决的问题是：

> 如何让第三方应用访问用户资源，而不需要知道用户密码。

例如：

你使用：

- 
微信登录某网站

- 
GitHub 登录某工具

- 
Google 登录某应用

网站不会拿到你的密码。

而是：

```
第三方应用

       ↓

请求授权

       ↓

微信/GitHub/Google

       ↓

用户确认

       ↓

返回 Access Token

       ↓

访问用户信息
```

---

# 五、OAuth2 的核心概念

OAuth2 中主要有几个角色：

## 1. Resource Owner

资源拥有者。

通常就是：

> 用户

例如：

你的 GitHub 账号。

---

## 2. Client

客户端。

也就是：

想访问资源的应用。

例如：

某个代码统计工具。

---

## 3. Authorization Server

授权服务器。

负责：

- 
用户登录

- 
用户授权

- 
发放 Token

例如：

GitHub OAuth 服务。

---

## 4. Resource Server

资源服务器。

保存用户数据。

例如：

GitHub API：

```
api.github.com/user
```

---

# 六、OAuth2 登录流程

以 GitHub 登录网站为例：

```
用户

 ↓

网站点击 GitHub 登录

 ↓

跳转 GitHub

 ↓

用户授权

 ↓

GitHub 返回 Code

 ↓

网站交换 Access Token

 ↓

调用 GitHub API

 ↓

获取用户信息

 ↓

创建本站账号
```

注意：

OAuth2 本身并不是登录协议。

它只是：

> 授权协议。

---

# 七、SSO 和 OAuth2 最大区别

对比

SSO

OAuth2

中文

单点登录

开放授权

目的

登录一次访问多个系统

授权第三方访问资源

核心

身份认证

权限授权

关注

你是谁

你允许别人访问什么

返回

登录状态

Access Token

常见场景

企业内部系统

第三方应用授权

协议

CAS、SAML、OIDC

OAuth2

---

# 八、为什么很多人觉得 OAuth2 是 SSO？

因为现代互联网经常组合：

```
OAuth2 + OpenID Connect
```

形成：

> 登录系统

例如：

使用 Google 登录：

实际上流程：

```
OAuth2
+
OIDC身份认证层

=
第三方登录
```

其中：

OAuth2：

负责：

```
授权
```

OIDC：

负责：

```
身份认证
```

---

# 九、SSO 和 OAuth2 如何一起使用？

大型系统通常这样设计：

```
                 用户

                  |
                  ↓

          身份认证中心

                  |
       ----------------------
       |                    |
    企业系统A             企业系统B

                  ↑

              OAuth2 Client

                  ↑

          第三方应用
```

例如：

企业内部：

- 
使用 SSO 管理员工登录

开放平台：

- 
使用 OAuth2 给合作伙伴授权

---

# 十、实际项目应该怎么选择？

## 企业内部多个系统

推荐：

```
SSO + OIDC
```

例如：

- 
公司 OA

- 
CRM

- 
ERP

- 
工单系统

---

## 开放 API 给第三方

推荐：

```
OAuth2
```

例如：

- 
开放平台

- 
开发者 API

- 
第三方插件

---

## 做网站第三方登录

推荐：

```
OAuth2 + OIDC
```

例如：

- 
GitHub 登录

- 
Google 登录

- 
企业微信登录

---

# 十一、常见误区

## 误区1：

> OAuth2 就是登录协议

错误。

OAuth2 负责授权。

登录通常需要：

```
OAuth2 + OIDC
```

---

## 误区2：

> SSO 就一定使用 OAuth2

错误。

SSO 可以使用：

- 
CAS

- 
SAML

- 
OIDC

- 
Kerberos

---

## 误区3：

> Access Token 等于用户密码

错误。

Token：

- 
可以限制权限

- 
可以设置过期时间

- 
可以撤销

---

# 十二、总结

简单理解：

一句话：

> **SSO 是让用户“一次登录，到处访问”；OAuth2 是让应用“经过用户授权访问资源”。**

两者关系：

```
SSO
 |
 +-- CAS
 |
 +-- SAML
 |
 +-- OIDC

OAuth2
 |
 +-- Access Token
 |
 +-- API授权
```

在现代 Web 开发中，最常见方案：

```
身份认证：
OIDC

授权：
OAuth2

多系统登录：
SSO
```

理解三者关系，可以帮助开发者正确设计企业级账号体系，而不是把所有“登录”问题都简单归类为 OAuth2。
