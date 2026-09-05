# Stable Diffusion 从零开始完整教程

本教程适合所有新手用户，通过一篇文章了解 Stable Diffusion 的原理、下载方式、安装流程、模型配置、基础使用方法，以及如何通过[秋葉 aaaki ](https://space.bilibili.com/12566101)的 sd-webui-aki v4.10 整合包实现最快捷的本地 AI 绘图体验。

---

# 一、Stable Diffusion 基本认识

Stable Diffusion 是一个本地运行的 AI 生成图像模型，具备以下优势：

- 
支持离线运行，无需联网

- 
可安装各种模型（如真实风、二次元风、写实风）

- 
可加载 LoRA、VAE、ControlNet 等增强插件

- 
自由控制分辨率、光影、姿势、构图等

本地部署后可以完全摆脱在线平台的限制，自由生成高质量图片。

---

# 二、Stable Diffusion 电脑配置要求

最低要求：

- 
显卡：6GB 显存（如 1660Ti）

- 
内存：16GB

- 
系统：Windows / macOS / Linux

推荐配置：

- 
显卡：NVIDIA 8GB 显存以上（如 3060 / 4060 / 4070）

- 
内存：32GB 以上

- 
CPU：i5 / R5 或以上

---

# 三、安装方式一：官方 WebUI（手动安装）

本节适合想要了解 SD 原理、手动控制环境的用户。

## 3.1 下载 WebUI

仓库地址：

```
https://github.com/AUTOMATIC1111/stable-diffusion-webui

```

点击 Code → Download ZIP 下载。

## 3.2 解压到任意目录

示例：

```
D:\AI\stable-diffusion-webui\

```

目录不能含中文、空格或特殊字符。

## 3.3 启动 WebUI

双击：

```
webui-user.bat

```

初次运行会自动：

- 
安装 Python

- 
安装依赖

- 
初始化虚拟环境

- 
安装 WebUI

完成后浏览器会自动打开：

```
http://127.0.0.1:7860/

```

---

# 四、安装方式二：秋葉 aaaki Stable Diffusion 整合包 v4.10（推荐新手）

如果你想要最快捷、最简便的方式，那么秋葉 aaaki 的 sd-webui-aki v4.10 是最适合新手的方案。它属于 Stable Diffusion 的一键整合包，无需手动配置 Python 或 Git。

## 4.1 秋葉整合包优势

- 
解压即可启动

- 
一键安装环境，无需懂技术

- 
内置 WebUI、插件、模型管理工具

- 
可选安装 ControlNet、LoRA 支持

- 
内置显卡优化参数

适合零基础用户。

## 4.2 下载与解压

- 
前往[秋葉整合包发布页](https://bxel2m5tvh.feishu.cn/wiki/I5qCwlPXMiFOn7khNZScAOTCnoc)下载 v4.10（通常为 7z 压缩包）

- 
下载地址：

- 
本站存储可免费直接下载，不限速。下载前请先登录，登录后即可享受免费高速下载服务下载链接：

下载链接：[https://blogoss.itxiaohui.top/sd-webui-aki-v4.10.zip](https://blogoss.itxiaohui.top/sd-webui-aki-v4.10.zip)

 

下载地址

sd-webui-aki整合包

                夸克网盘
            

sd-webui-aki整合包

                夸克网盘
            

sd-webui-aki整合包

                百度云网盘  ·  提取码: aaki
            

sd-webui-aki整合包

                阿里云网盘
            

- 将文件解压到：

```
D:\AI\sd-webui-aki-v4.10\

```

注意：路径不能有中文或空格。

部分版本有解压密码，例如：

```
bilibili-秋葉aaaki
```

## 4.3 安装依赖

整合包内有一个“运行环境安装程序”，双击执行即可自动完成依赖安装。

## 4.4 启动 WebUI

双击：

```
A绘世启动器.exe
```

![截屏2025-12-07 19.54.06-bumxgfhp.png](assets/png/articles/c8dc0103036d.png)

在启动器界面选择：

```
一键启动
```

首次启动会完成环境部署，随后自动打开 SD WebUI 界面，即可使用。

SD WebUI 界面截图：

![截屏2025-12-07 19.57.48-gyztqckw.png](assets/png/articles/de86d0d3a9aa.png)

驱动器截图：

![截屏2025-12-07 19.56.34-hngrjyyb.png](assets/png/articles/e192e334ce8c.png)

---

# 五、模型与资源安装

无论手动安装版还是秋葉整合包，模型放置方式相同。

## 5.1 下载模型资源网站

推荐：

- 
Civitai：[https://civitai.com](https://civitai.com)

- 
HuggingFace：[https://huggingface.co](https://huggingface.co)

## 5.2 基础模型（Checkpoint）放置位置

将 `.safetensors` 或 `.ckpt` 放入：

```
stable-diffusion-webui/models/Stable-diffusion/

```

推荐基础模型：

- 
Realistic Vision（写实）

- 
DreamShaper（通用）

- 
Anything V6（二次元）

## 5.3 LoRA 放置路径

```
models/Lora/

```

加载方式：

```

```

## 5.4 VAE 放置路径

```
models/VAE/

```

## 5.5 ControlNet 模型路径

```
extensions/sd-webui-controlnet/models/

```

---

# 六、Stable Diffusion WebUI 使用教程

WebUI 打开后主要功能如下：

- 
txt2img：文生图

- 
img2img：图生图（重绘）

- 
Extras：放大/修复

- 
Lora：加载风格模型

- 
Extensions：插件管理

---

# 七、Prompt 提示词写法（核心）

Prompt 决定图像的内容和质量。

## 7.1 Prompt 基本结构

```
主体 + 风格 + 光影 + 镜头 + 画质 + 细节增强

```

示例：

```
a beautiful girl, ultra detailed, 8k resolution, soft lighting, cinematic, masterpiece, high quality

```

## 7.2 Negative Prompt（反向提示）

用于排除错误和不想要的内容。

```
low quality, bad anatomy, extra fingers, blurry, watermark, distorted face

```

---

# 八、常用参数解释

参数含义如下：

参数

用途

Sampling method

采样器，推荐 DPM++ 2M Karras

Sampling steps

步数，20-30 常用

CFG Scale

文本遵循度，6-8 合适

Width/Height

分辨率，建议 512 或 768 基础使用

Seed

随机种子，-1 表示随机

---

# 九、图生图（img2img）

适合：

- 
重绘

- 
生成不同风格

- 
换服装、换姿势

- 
修图

关键参数 **denoising strength**：

- 
0.2 - 0.4：贴近原图

- 
0.5 - 0.7：中度变化

- 
0.7 - 0.9：大幅改动

---

# 十、常见插件

推荐安装：

插件

功能

ControlNet

姿势、线稿、深度控制

OpenPose Editor

人体姿势编辑

Dynamic Prompts

随机 prompt

ultimate-upscale

高清放大

Tiled Diffusion

超大图无缝生成

秋葉 v4.10 已内置大部分常用插件，可直接使用。

---

# 十一、启动优化参数

编辑 `webui-user.bat`：

```
set COMMANDLINE_ARGS=--xformers --medvram

```

常用参数：

- 
--xformers：更快更省显存

- 
--medvram：适用于显存小于 6GB

- 
--api：启用 API

---

# 十二、常见问题

## 1. CUDA 报错

解决方法：

- 
更新显卡驱动

- 
重新运行启动器安装依赖

## 2. 图像质量差

检查：

- 
使用高质量模型

- 
增强 Negative prompt

- 
使用合适的 VAE

## 3. 显存不足

解决：

- 
降低分辨率到 512

- 
使用 --medvram

- 
禁用不必要插件

---

# 十三、总结

通过本教程你可以：

- 
完成 Stable Diffusion 安装

- 
使用秋葉 aaaki 整合包快速上手

- 
下载与安装模型、LoRA、ControlNet

- 
写出高质量 Prompt

- 
掌握 txt2img / img2img 的基本操作

- 
解决常见问题和优化性能

# 十四、参考资料

[【AI绘画·25年最新】Stable Diffusion整合包v4.10发布！解压即用 防爆显存 三分钟入门AI绘画 ☆更新 ☆训练 ☆汉化 秋叶整合包](https://www.bilibili.com/video/BV1iM4y1y7oA)

[Stable Diffusion超详细教程！从0-1入门到进阶](https://zhuanlan.zhihu.com/p/622238031)

[https://chata.itxiaohui.top/10002](https://chata.itxiaohui.top/10002)
[
](https://www.bilibili.com/v/popular/rank/all)
