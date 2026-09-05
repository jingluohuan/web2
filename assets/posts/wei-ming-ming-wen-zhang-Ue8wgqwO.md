# VSCode 配置 C/C++ 环境

# 1. 安装

## 1.1 VSCode

**官网下载：**

[Visual Studio Code](https://code.visualstudio.com/Download)

**联想应用商店下载:**

[联想应用商店](https://lestore.lenovo.com/detail/22856)

推荐选择联想应用商店下载，更新及时，下载速度快。

## 1.2 插件

**VSCode 中文语言包安装：**

引用链接

适用于 VS Code 的中文（简体）语言包

microsoft.com

**C/C++ 插件安装：**

引用链接

C/C++ for Visual Studio Code

microsoft.com

安装完成 C/C++ 插件后，再将其降级到 v1.8.4 版本，因为这是最后一个支持自动创建 `launch.json` 文件 (调试配置文件) 的版本。
点击卸载按钮旁边的小箭头，选择 “安装特定版本” 后搜索”1.8.4” 进行安装即可。

## 1.3 编译器

推荐使用 TDM-GCC 安装 GCC 编译器，因为其支持自动配置环境变量并且同时支持 32 位和 64 位程序的编译。

[TDM-GCC](https://jmeubank.github.io/tdm-gcc/download/)

网速不好的话可以使用联想应用商店进行下载，但不是最新版：

[TDM-GCC](https://lestore.lenovo.com/detail/L101412)

安装路径随意，不一定非得 C 盘。

安装完成后，启动 CMD，输入以下命令进行验证：

```
gcc --version
g++ --version
gdb --version
```

[
![alt text](assets/png/articles/64d6ea91f571.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21.webp)

不推荐使用 msys2 的 gcc，因为在我的测试中，msys2 的 gcc 默认会将输出显示在调试控制台而非集成终端中，导致无法输入交互。切换为独立终端后，又无法使用 getchar () 函数以及 system (“pause”) 函数暂停终端，导致独立终端只能一闪而过，不能查看输出。

# 2. 配置

## 2.1 基础配置

打开 CMD，输入以下命令创建一个新的工作目录并启动 VSCode：

```
mkdir test
cd test
code .
```

创建一个名为 hello.c 的文件，输入以下代码：

```
#include 

int main(void) {
    printf("hello world\n");
    
    return 0;
}
```

**CTRL**+**S**保存文件后，按**F5**启动调试，弹出菜单中选择调试器为 "C++（GDB/LLDB）"：

[
![alt text](assets/png/articles/b8e4139788ee.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-1.webp)

接着选择配置为刚安装的 gcc 编译器：

[
![alt text](assets/png/articles/4d87faef89b2.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-2.webp)

选择后会自动编译运行程序，并将输出打印在 VSCode 的集成终端中：

[
![alt text](assets/png/articles/28d8b75f6f75.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-3.webp)

此时，VSCode 已经可以正常编译以及调试 C/C++ 程序了。但还可以进一步优化配置。

## 2.2 优化配置

### 2.2.1 头文件引用报错

引用头文件位置提示了错误（检测到 #include 错误。请更新 includePath。）：

[
![alt text](assets/png/articles/10a72b3f20eb.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-4.webp)

虽然不影响程序运行，但是还是建议解决一下：

**CTRL**+**Shift**+**P**打开命令面板，输入 "C/C++，选择" 编辑配置 (UI)"：

[
![alt text](assets/png/articles/4ae2359a8e53.png)
](https://blog.lololowe.com/posts/6da8/image.png)

在新窗口中设置” 编译器路径” 为 TDM-GCC 的 gcc 编译器，”IntelliSense 模式” 设置为”Windows-gcc-x64”：

[
![alt text](assets/png/articles/43e61e7b1004.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-5.webp)

返回 hello.c 文件，可以看到报错信息已经消失了：

[
![alt text](assets/png/articles/63c3466a00f5.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-6.webp)

### 2.2.2 独立窗口运行

运行结果默认是显示在 vscode 的集成终端中，但是如果想要在独立窗口中运行程序，可以在 `launch.json` 文件（编译任务配置文件）中修改 `externalConsole` 为 `true`：

[
![alt text](assets/png/articles/1f0cbaba1515.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-7.webp)

由于程序运行完会立即关闭，导致窗口一闪而过，无法看到运行结果。所以可以在代码中添加一个 `getchar()` 函数来保持程序运行：

```
#include 

int main(void) {
    printf("hello world\n");

    getchar();  // 等待用户输入一个字符，以保持程序运行
    return 0;
}
```

[
![alt text](assets/png/articles/49ce5381752f.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-8.webp)

### 2.2.3 中文乱码

代码中如果包含中文字符，可能会出现乱码问题：

[
![alt text](assets/png/articles/5a4efbc673ab.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-9.webp)

可以通过设置文件编码为 GBK 或者 GB2312 来解决，但是每次都要手动设置比较麻烦。可以在 `tasks.json` 文件中添加以下 `"-fexec-charset=GBK"` 选项来解决：

[
![alt text](assets/png/articles/c1fce86a7319.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-10.webp)

[
![alt text](assets/png/articles/ad6e2cb23c47.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-11.webp)

### 2.2.4 归纳编译文件

在 `tasks.json` 文件中修改 `-o` 选项的参数为 `"${workspaceFolder}\\a.exe"`，`launch.json` 文件的 `program` 也改为 `"${workspaceFolder}\\a.exe"`，
这样编译后的可执行文件就会被归纳到工作区的根目录中，避免了编译文件的分散：

[
![alt text](assets/png/articles/cf3074dceb9f.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-12.webp)

[
![alt text](assets/png/articles/08047807d18a.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-13.webp)

# 3. 补充：MSVC

如果电脑里面安装了 Visual Studio，也可以使用 Visual Studio 的 MSVC 编译器进行编译及调试。

MSVC 支持文件路径中包含中文，而 GCC 不支持：

[
![alt text](assets/png/articles/09dbb9fa568e.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-18.webp)

开始菜单搜索”vs”：

[
![alt text](assets/png/articles/7e3a0020955a.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-14.webp)

然后打开 VS 的开发者命令提示符，输入 `cl` 命令，可以查看 MSVC 的版本信息：

[
![alt text](assets/png/articles/0c4e348e33cf.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-15.webp)

VS 的 CMD 默认配置了 MSVC 相关的的环境变量，使用 `echo %PATH%` 命令可以查看环境变量。

在 VS 的 CMD 中启动 VSCode 能自动继承 MSVC 的环境变量，因此可就不需要配置 vscode 的相关环境了。

首先 `cd` 到代码目录，然后输入 `code .` 命令启动 VSCode。

新建 hello.c 文件，输入以下代码并保存：

```
#include 

int main(void)
{
    printf("hello world\n");

    return 0;
}
```

**CTRL**+**Shift**+**P**打开命令面板，输入 "C/C++，选择" 编辑配置 (UI)"。在新窗口中设置" 编译器路径 "为 VS 的 cl.exe 编译器，"IntelliSense 模式 "设置为"windows-msvc-x64"：

[
![alt text](assets/png/articles/c3beb38e0b2c.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-16.webp)

回到 hello.c 文件，按**F5** 开始编译调试，选择 “C++（Windows）” 作为调试器，编译器选择 cl.exe，随后会自动编译运行程序，打印输出会显示在独立终端中。

[
![alt text](assets/png/articles/9cfcd9e96616.webp)
](https://blog.lololowe.com/posts/6da8/PixPin_2025-09-21-17.webp)

# 4. 参考

[为 Microsoft C++ 配置 VS Code](https://vscode.js.cn/docs/cpp/config-msvc)

**免责声明**

本文部分内容整理或引用自公开的互联网资源。相关内容的原创权、著作权和商标权等知识产权均归原作者或合法权利人所有。

本站转载或引用旨在进行技术学习、交流与分享，**并非用于任何商业目的**。我们尊重知识产权，并致力于保护原创者的合法权益。
🛡️ **权利人如果认为本站内容存在侵权嫌疑，请通过以下方式与我们联系**，我们将立即核实并进行处理：

- 
**联系邮箱**：yunying@itxiaohui.top

我们在收到通知并核实后，将**七天内采取包括但不限于删除相关内容等措施**。

特此声明！[
](https://blog.lololowe.com/go.html?url=aHR0cHM6Ly92c2NvZGUuanMuY24vZG9jcy9jcHAvY29uZmlnLW1zdmM)
