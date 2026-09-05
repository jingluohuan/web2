# Java 打包成 APK 超详细教程

很多人都会问：

> ❓ Java 项目怎么打包成 APK？❓ Windows 和 Mac 有区别吗？❓ Linux 服务器能自动打包吗？

这篇文章会从 0 开始，带你完整走一遍：

![](assets/png/articles/a6f4866bc7a2.webp)

适用于：

- 
Windows

- 
macOS

- 
Linux（含服务器）

---

# 一、Java 可以直接打包成 APK 吗？

答案是：

❌ 不能直接打包
 ✅ 必须通过 Android 构建系统编译

Android 的编译流程是：

![](assets/png/articles/e1b20163baae.webp)

这个过程由：

![](assets/png/articles/842d77279952.webp)

自动完成。

---

# 二、全平台环境准备

---

# 1️⃣ 安装 JDK（Windows / Mac / Linux）

推荐版本：

- 
JDK 8

- 
JDK 11

下载：

- 
[Oracle JDK](https://www.oracle.com/java/technologies/downloads/)

- 
[OpenJDK](https://openjdk.org/)

---

## Windows 安装

- 
下载 `.exe`

- 
双击安装

- 
配置环境变量：

![](assets/png/articles/cf5c565a2f2a.webp)

测试：

```
java -version
javac -version
```

---

## macOS 安装

推荐使用 brew：

```
brew install openjdk@11
```

或者官网下载 `.dmg`

测试：

```
java -version
```

---

## Linux 安装（Ubuntu 示例）

```
sudo apt update
sudo apt install openjdk-11-jdk
```

测试：

```
java -version
```

---

# 2️⃣ 安装 Android Studio（全平台一致）

官方下载：

- 
[Android Studio](https://developer.android.google.cn/studio?hl=zh-cn)

支持：

- 
Windows

- 
macOS

- 
Linux

安装时务必勾选：

- 
Android SDK

- 
Android SDK Platform

- 
Android SDK Build Tools

安装完成后首次启动会自动下载依赖。

---

# 三、创建 Java Android 项目

> ⚠️ 注意：语言必须选择 Java

### 1️⃣ 新建项目

```
File → New → New Project
```

选择：

```
Empty Activity
```

点击 Next

---

### 2️⃣ 项目配置

选项

建议

Name

MyJavaApp

Package

com.example.myjavaapp

Language

Java

Minimum SDK

API 21+

点击 Finish，等待 Gradle 构建完成。

---

# 四、编写 Java 示例代码

打开：

```
app/src/main/java/你的包名/MainActivity.java
```

示例代码：

```
package com.example.myjavaapp;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        TextView tv = new TextView(this);
        tv.setText("Hello Java APK!");
        tv.setTextSize(24);

        setContentView(tv);
    }
}
```

点击运行可以在模拟器或真机测试。

---

# 五、生成 APK（图形界面方式）

---

## 方法一：生成调试 APK（测试用）

菜单：

```
Build → Build APK(s)
```

路径：

```
app/build/outputs/apk/debug/app-debug.apk
```

特点：

- 
可直接安装

- 
使用 debug 签名

- 
不能上架应用商店

---

## 方法二：生成正式签名 APK（发布用）

点击：

```
Build → Generate Signed Bundle / APK
```

选择：

```
APK
```

点击 Next

---

### 创建 keystore

点击：

```
Create new
```

填写：

- 
Key store path

- 
Password

- 
Alias

- 
Validity（建议 25 年以上）

---

### 选择构建类型

```
release
```

勾选：

- 
V1

- 
V2

完成后生成：

```
app/build/outputs/apk/release/app-release.apk
```

---

# 六、命令行打包（Windows / Mac / Linux 通用）

适合：

- 
自动化构建

- 
服务器打包

- 
CI/CD

---

## 1️⃣ 使用 Gradle 打包

进入项目目录：

### Windows：

```
gradlew assembleRelease
```

### Mac / Linux：

```
./gradlew assembleRelease
```

生成路径：

```
app/build/outputs/apk/release/
```

---

## 2️⃣ 服务器 Linux 自动打包流程

安装：

- 
OpenJDK

- 
Android SDK Command-line Tools

设置环境变量：

```
export ANDROID_HOME=~/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

然后执行：

```
./gradlew assembleRelease
```

即可生成 APK。

---

# 七、纯 Java 项目如何转为 APK？

如果你已有普通 Java 项目：

### 方式一（推荐）

- 
创建 Android Java 项目

- 
把业务逻辑复制到：

```
app/src/main/java/
```

- 
UI 部分改成 Android 组件

---

### 方式二（高级）

手动流程：

```
javac → dx/d8 → aapt → apksigner
```

这种方式复杂，一般不推荐。

---

# 八、APK vs AAB

格式

用途

APK

直接安装

AAB

上架 Google Play 推荐

生成 AAB：

```
Build → Generate Signed Bundle
```

---

# 九、常见问题

---

## ❓ Windows 和 Mac 打包有区别吗？

没有

区别只在：

- 
命令格式（./gradlew）

- 
JDK 安装方式

---

## ❓ Linux 可以不装 Android Studio 吗？

可以

只安装：

- 
Android SDK Command-line Tools

- 
JDK

即可命令行打包。

---

## ❓ Java 和 Kotlin 打包方式一样吗？

是的

都通过：

- 
Gradle

构建生成 APK。

---

# 十、完整流程总结

![](assets/png/articles/e9f30790422d.webp)

---

# 十一、进阶玩法

- 
自动签名脚本

- 
多渠道打包

- 
混淆 Proguard

- 
服务器自动构建

- 
GitHub Actions 自动生成 APK

- 
Docker 打包环境
