# 深度解析 Linux 权限体系：从基础到进阶的完整指南

![ChatGPT Image 2025年11月30日 15_08_32-zdgpzmtq.png](assets/png/articles/3e8bde2b606a.png)

Linux 权限体系是整个系统安全模型的核心。无论是服务器运维、程序部署还是日常开发，如果对权限理解不到位，轻则导致脚本无法执行、服务无法启动，重则出现安全漏洞、越权访问甚至被恶意利用。

很多人对 Linux 的印象是“权限很麻烦”。但实际上，Linux 权限的设计非常清晰，只要理解了核心概念，就能从容应对几乎所有权限相关的问题。

本文将从基础到进阶完整讲解：

- 
文件权限到底是什么？

- 
`rwx` 三种权限真正的含义是什么？

- 
用户/用户组如何影响访问控制？

- 
为什么有的目录能进入、有的不能？

- 
`chmod 777` 为什么是大忌？

- 
ACL、SUID、SGID、Sticky bit 是什么？

- 
实际场景中权限如何设计最合理？

- 
如何快速定位“为什么我没有权限”？

文章将结合例子、图示和实战场景，力求让你彻底理解 Linux 权限体系。

---

## 一、Linux 权限的三层结构：用户、组、其他人

先看一个最典型的文件权限信息：

```
ls -l test.txt
-rw-r--r-- 1 root root 1209 Feb 1 12:07 test.txt

```

拆解：

```
-rw-r--r--   → 权限
1            → 硬链接数
root         → 文件所有者（user）
root         → 文件所属组（group）
1209         → 文件大小

```

重点是最前面的：

位数

含义

1

文件类型（- 文件，d 目录）

2-4

Owner 权限

5-7

Group 权限

8-10

Other 权限

一个典型的 Linux 系统中，一个文件有三类访问者：

- 
文件所有者（user）

- 
文件所属组（group）

- 
其他所有用户（other）

简单来说：
Linux 权限体系是以“文件”为中心，而不是用户为中心。

---

## 二、rwX 权限的真正含义（很多人理解错）

### 1. 对文件的 rwx

权限

文件含义

r

可以读取文件内容

w

可以修改文件内容

x

可以执行文件

文件的 `x` 执行权限很关键，例如：

```
chmod +x install.sh

```

否则你只能用：

```
bash install.sh

```

### 2. 对目录的 rwx（更关键）

很多人忽略目录权限的特殊性。

权限

目录含义

r

可以列出目录内容（ls）

w

可以在目录中新增/删除文件

x

可以进入目录（cd）

尤其是目录的 `x`：

**如果目录没有 x 权限，你无法进入，即便你拥有 r 权限。**

例如：

```
chmod 644 mydir

```

你可以看到目录列表，但是无法进入它：

```
ls mydir  # OK
cd mydir  # Permission denied

```

目录最合理的权限通常是：

```
rwxr-xr-x（755）

```

---

## 三、chmod、chown、chgrp：所有权限修改的三大基石

### 1. chmod：改权限

基本格式：

```
chmod 755 file
chmod u+x file
chmod g-w file
chmod o-rwx file

```

### 2. chown：改所有人

```
chown user file
chown user:group file

```

### 3. chgrp：改所属组

```
chgrp nginx /var/www

```

---

## 四、为什么 chmod 777 是大忌？

`777` 意味着：

- 
所有人都能读

- 
所有人都能写

- 
所有人都能执行

风险：

- 
任意用户可以覆盖文件

- 
任意用户可以替换脚本

- 
目录下可以植入恶意程序

- 
文件可能被修改、删除

尤其是：

```
chmod -R 777 /var/www

```

看似能让网站“正常运行”，实际上等于完全敞开门户。

正确做法应该是：

- 
给 nginx 用的文件，把所有者设为 nginx

- 
给应用程序目录设为特定用户

- 
给其他人只读或无权限

例如：

```
chown -R nginx:nginx /var/www
chmod -R 755 /var/www

```

---

## 五、进阶：ACL（细粒度权限控制）

传统权限体系只能指定：

- 
owner

- 
group

- 
other

但如果你要：

> 给一个特定用户访问某个文件的权限但不想把他加入文件所属组

这时用 ACL（Access Control List）。

### 1. 启用 ACL

大部分系统默认开启，可测试：

```
mount | grep acl

```

### 2. 赋予 ACL 权限

```
setfacl -m u:zhangsan:rwx test.txt

```

查看：

```
getfacl test.txt

```

ACL 非常适合企业环境、多人协作、共享目录等场景。

---

## 六、SUID、SGID、Sticky bit：高级权限控制

### 1. SUID（Set User ID）

用途：
当用户执行一个程序时，以程序“文件所有者”的身份运行。

最典型的例子：

```
/usr/bin/passwd

```

SUID 允许普通用户修改自己的密码（需要写 `/etc/shadow`）。

设置：

```
chmod u+s filename

```

标识：

```
-rwsr-xr-x

```

### 2. SGID（Set Group ID）

用途：

- 
使目录中新建的文件自动继承目录的用户组

- 
用于团队共享目录

设置：

```
chmod g+s folder

```

目录权限会变为：

```
rwxr-sr-x

```

### 3. Sticky bit（t）

用途：

- 
只有文件拥有者能删除文件

- 
目录常用：/tmp

权限标识：

```
rwxrwxrwt

```

非常适合公共可写目录。

---

## 七、实战：常见权限问题的排查方法

### 1. 脚本无法运行

```
bash: command not found

```

检查：

```
ls -l script.sh
chmod +x script.sh

```

### 2. Nginx 无法读取文件

Nginx 运行用户可查看：

```
ps aux | grep nginx

```

然后检查：

```
ls -l /var/www/site
chown nginx:nginx -R /var/www/site
chmod 755 /var/www/site

```

### 3. Docker 挂载目录没权限

修复：

```
chmod -R 755 /data

```

或创建专属用户组。

### 4. 新建文件所属组不对

使用 SGID：

```
chmod g+s /project

```

目录内创建的文件会自动属于该组。

---

## 八、最佳实践：如何设计一个安全又好用的权限体系？

### 1. 永远不要使用 777

除非是临时测试。

### 2. 应用文件夹的所有者应该是应用用户

例如：

```
/var/www → nginx用户  
/home/git → git用户  
/opt/app → app用户  

```

### 3. 普通用户不要直接使用 root

通过 sudo 授权特定命令：

```
visudo

```

配置：

```
zhangsan ALL=(ALL) NOPASSWD:/usr/bin/systemctl restart nginx

```

### 4. 多人协作用 SGID + ACL

适用于：

- 
企业项目目录

- 
团队共享资源

### 5. 定期扫描权限异常

可以用：

```
find / -perm 777

```

或：

```
find / -perm -4000

```

扫描 SUID 程序。

---

## 九、总结

Linux 权限体系一点都不难，只要理解：

- 
`rwx` 在“目录”与“文件”上的不同含义

- 
user/group/other 三层访问结构

- 
chmod/chown/chgrp 三件套

- 
ACL、SUID、SGID、Sticky bit 的进阶功能

你就能解决 90% 的权限问题。

真正难的不是命令本身，而是如何在不同场景中设计合理的权限架构，让系统既安全又高效。
