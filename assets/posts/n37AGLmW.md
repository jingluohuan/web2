# ComfyUI 超详细入门教程：从下载安装到工作流实战

AI 绘画这几年发展非常快，很多人接触最多的是 Stable Diffusion WebUI，但如果你想真正理解 AI 绘画的运行逻辑、搭建复杂工作流，甚至做视频、换脸、角色一致性生成，那么 **ComfyUI** 基本是绕不开的工具。

这篇文章会从 **ComfyUI 是什么、下载安装、界面介绍、基础工作流、模型加载、插件安装、常见问题** 等方面，带你完整入门。

---

## 一、ComfyUI 是什么？

ComfyUI 是一个基于 **节点（Node）工作流** 的 Stable Diffusion 图形界面。

简单来说：

普通 WebUI：

> 你填参数 → 点生成 → 出图

ComfyUI：

> 你自己搭建 AI 生成流程 → 控制每一步 → 出图

它有点像：

- 
Blender 节点

- 
UE 蓝图

- 
Photoshop 图层逻辑

所以很多人第一次打开会觉得：

> 这啥？怎么全是方块？

但实际上，一旦学会，你会发现它比传统 WebUI 强太多。

ComfyUI 的优势：

### 1、工作流可视化

整个 AI 绘图过程全部可见。

例如：

- 
模型加载

- 
提示词

- 
采样

- 
放大

- 
面部修复

- 
ControlNet

- 
视频生成

全部通过节点连接。

---

### 2、资源占用更友好

很多用户实测：

ComfyUI：

- 
显存利用率更高

- 
崩溃率较低

- 
大模型运行更稳定

尤其是：

- 
SDXL

- 
Flux

- 
视频工作流

优势更明显。

---

### 3、插件生态非常强

现在很多 AI 新技术：

- 
Flux 工作流

- 
视频生成

- 
换脸

- 
IPAdapter

- 
InstantID

- 
AnimateDiff

基本都会优先适配 ComfyUI。

所以不少人会说：

> WebUI 是用 AI 画图ComfyUI 是玩 AI 生产线

---

## 二、ComfyUI 下载与安装

目前常见有两种方式。

---

## 方法一：Windows 整合包（推荐新手）

最简单。

特点：

- 
解压即用

- 
自带 Python

- 
不需要配环境

步骤：

### 第一步：下载 ComfyUI

下载官方版或整合包。

解压后目录一般如下：

```
ComfyUI
├─ models
├─ custom_nodes
├─ output
├─ input
├─ ComfyUI_windows_portable
```

---

### 第二步：启动

进入：

```
ComfyUI_windows_portable
```

双击：

```
run_nvidia_gpu.bat
```

如果是：

- 
NVIDIA 显卡 → run_nvidia_gpu

- 
CPU → run_cpu

启动后会出现：

```
Running on local URL:
http://127.0.0.1:8188
```

浏览器打开即可。

---

## 方法二：Python 环境安装

适合：

- 
Linux

- 
服务器

- 
Docker

- 
自定义环境

先安装：

```
Python 3.10+
Git
```

然后：

```
git clone https://github.com/comfyanonymous/ComfyUI
```

进入目录：

```
cd ComfyUI
```

安装依赖：

```
pip install -r requirements.txt
```

运行：

```
python main.py
```

---

## 三、ComfyUI 界面认识

第一次打开，一般长这样：

左边：

节点区

右边：

生成结果

底部：

队列和状态

主要区域：

### Node 节点区

核心区域。

所有工作流都在这里搭建。

---

### Queue Prompt

生成按钮。

相当于：

> 开始绘图

---

### Save

保存工作流。

格式：

```
json
```

以后可以直接导入。

---

### Load

加载工作流。

很多大神分享的：

> 工作流文件

其实就是：

```
json
```

---

## 四、ComfyUI 基础工作流原理

先理解核心逻辑。

AI 出图其实就是：

模型 + 提示词 + 采样器 + 解码

最基础工作流：

```
Checkpoint → Prompt → KSampler → VAE → Save Image
```

也就是：

- 
加载模型

- 
输入提示词

- 
采样生成

- 
解码图片

- 
保存结果

这是理解 ComfyUI 的关键。

---

## 五、创建第一个工作流

下面做一个最简单的。

右键空白区域：

Add Node。

添加以下节点。

---

### 1、Checkpoint Loader

作用：

加载模型。

例如：

```
anything
SDXL
Flux
```

模型位置：

```
models/checkpoints
```

---

### 2、CLIP Text Encode

两个。

分别：

- 
正向提示词

- 
反向提示词

正向：

```
best quality, girl
```

反向：

```
low quality, blur
```

---

### 3、KSampler

核心采样节点。

作用：

真正生成图片。

重要参数：

参数

作用

Steps

迭代次数

CFG

提示词权重

Sampler

采样算法

Seed

随机种子

推荐：

Steps：

```
20~30
```

CFG：

```
5~8
```

---

### 4、VAE Decode

作用：

潜空间转图片。

不接它：

看不到图。

---

### 5、Save Image

输出图片。

默认保存：

```
output
```

目录。

---

连接后：

点击：

Queue Prompt

即可生成第一张图。

---

## 六、模型怎么放？

很多新手不知道。

ComfyUI 模型都放：

```
models
```

里面。

常见目录：

### Checkpoints

大模型：

```
models/checkpoints
```

例如：

- 
SD1.5

- 
SDXL

- 
Flux

---

### LoRA

角色风格模型：

```
models/loras
```

---

### ControlNet

控制模型：

```
models/controlnet
```

---

### VAE

解码模型：

```
models/vae
```

---

放进去后：

刷新即可识别。

---

## 七、ComfyUI Manager 插件安装

这个插件几乎必装。

作用：

- 
一键装插件

- 
自动补依赖

- 
管理节点

安装方法：

进入：

```
custom_nodes
```

执行：

```
git clone https://github.com/ltdrdata/ComfyUI-Manager
```

重启。

然后菜单会出现：

```
Manager
```

之后安装插件就简单很多。

---

## 八、常用插件推荐

新手建议：

先装这些。

### 1、Impact Pack

功能：

- 
检测

- 
分割

- 
放大

- 
面部处理

万能包。

---

### 2、IPAdapter Plus

参考图生成。

例如：

> 按照某人物风格出图

很常用。

---

### 3、AnimateDiff

AI 动画。

支持：

- 
图转视频

- 
动画生成

---

### 4、Efficiency Nodes

简化节点。

让工作流更整洁。

---

## 九、为什么很多工作流导入后报错？

常见原因：

### 1、缺插件

最常见。

提示：

```
Missing Node Types
```

说明：

没有安装对应插件。

Manager 一键补。

---

### 2、模型路径错误

工作流引用：

- 
checkpoint

- 
lora

- 
controlnet

本地没有。

需要：

手动下载。

---

### 3、显存不足

报：

```
CUDA out of memory
```

解决：

- 
降分辨率

- 
降 Batch

- 
使用低显存模式

---

## 十、ComfyUI 值得学吗？

我的看法是：

如果只是：

- 
偶尔出图

- 
玩二次元图

WebUI 足够。

但如果你想：

- 
玩 AI 工作流

- 
做复杂生成

- 
学习 AI 图像流程

- 
做视频或自动化

ComfyUI 迟早都要学。

因为它不是简单的“绘图工具”，而更像一个：

> AI 图像工作流平台。

很多新技术，往往也是先在 ComfyUI 上出现。

---

## 结语

ComfyUI 一开始确实有学习门槛，但理解节点逻辑后，你会发现它的自由度远高于传统 WebUI。

建议新手：

不要一上来就导入几十个节点的大工作流。

先学会：

- 
模型加载

- 
Prompt

- 
KSampler

- 
基础出图

再慢慢进阶：

- 
LoRA

- 
ControlNet

- 
视频

- 
Flux

- 
自动化工作流

这样会轻松很多。
