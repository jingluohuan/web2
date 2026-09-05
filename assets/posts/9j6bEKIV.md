# 中国大陆安装 Claude Code 并接入 DeepSeek API 教程

## 前言

最近 Claude Code 在开发者圈非常火，它可以直接在终端中帮助我们编写代码、修改项目、执行命令、分析代码库，体验非常接近拥有一个真正的 AI 编程助手。

不过对于中国大陆用户来说，直接使用 Claude Code 会遇到网络和支付等问题。因此我选择了一个更适合国内开发者的方案：

**安装 Claude Code 客户端 + 使用 DeepSeek API 驱动。**

这样既能体验 Claude Code 强大的 Agent 能力，又能使用国内可直接充值的 DeepSeek API。

---

## 第一步：安装 Claude Code

首先前往 Claude Code 中文站：

**安装教程：**

[https://claude-zh.cn/guide/getting-started.html](https://claude-zh.cn/guide/getting-started.html)
找到自己对应的系统安装命令。

例如：

### Windows PowerShell

```
& ([scriptblock]::Create((New-Object Net.WebClient).DownloadString("https://claude-zh.cn/scripts/install.ps1")))
```

### Linux / macOS

```
source 
安装成功后可以在终端执行：

```
claude --version
```

如果能够正常显示版本号，说明安装成功。

---

## 第二步：获取 DeepSeek API Key

打开 DeepSeek 开放平台：

[https://platform.deepseek.com/](https://platform.deepseek.com/)

注册并登录账号。

进入：

```
API Keys
```

![](assets/png/articles/2950d895e774.webp)

创建一个新的 API Key。

创建后请妥善保存，因为关闭页面后可能无法再次查看完整密钥。

---

## 第三步：配置 Claude Code 使用 DeepSeek

DeepSeek 官方已经提供了 Claude Code 的适配方案。

官方文档：

[https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code)
根据官方文档，需要配置以下环境变量。Claude Code 会将请求发送到 DeepSeek 的 Anthropic 兼容接口。

### Windows PowerShell

```
$env:ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
$env:ANTHROPIC_AUTH_TOKEN=""
$env:ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
$env:ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro[1m]"
$env:ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro[1m]"
$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_EFFORT_LEVEL="max"
```

![](assets/png/articles/96639d28f275.png)

### Linux / macOS

```
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=
export ANTHROPIC_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
```

以上配置来自 DeepSeek 官方文档。

---

## 第四步：启动 Claude Code

进入你的项目目录：

```
cd 项目路径
```

然后启动：

```
claude
```

如果配置正确，Claude Code 就会通过 DeepSeek API 工作。

例如：

```
claude
```

然后输入：

```
帮我分析整个项目结构
```

或者：

```
修复这个报错
```

Claude Code 就会自动读取项目文件并开始工作。

---

## 我的使用体验

我目前主要使用：

- 
Claude Code 客户端

- 
DeepSeek API

- 
VS Code

这种组合有几个优点：

### 成本较低

相比直接使用 Anthropic 官方服务，DeepSeek API 的成本更加友好。

### 国内充值方便

无需折腾海外支付方式。

### Agent 能力完整

Claude Code 最强大的地方不是模型本身，而是它的工作流能力：

- 
自动读取项目

- 
自动修改代码

- 
自动执行命令

- 
自动分析报错

- 
自动生成文件

这些能力依然可以正常使用。

### 适合个人开发者

无论是：

- 
Python

- 
Java

- 
Spring Boot

- 
Vue

- 
React

- 
Halo 插件开发

都能获得非常不错的体验。

---

## 常见问题

### 提示找不到 claude 命令

重新打开终端，或者检查安装是否成功：

```
claude --version
```

---

### API Key 无效

检查：

```
ANTHROPIC_AUTH_TOKEN
```

是否填写正确。

---

### 模型无法调用

检查：

```
ANTHROPIC_BASE_URL
```

是否为：

```
https://api.deepseek.com/anthropic
```

---

## 结语

对于中国大陆开发者来说，使用「Claude Code + DeepSeek API」是一种非常实用的方案。

既能体验 Claude Code 强大的 AI Agent 工作流，又能使用 DeepSeek 提供的接口服务，在成本和体验之间取得不错的平衡。

如果你经常开发 Halo 插件、Spring Boot 项目、Python 工具或者前端应用，值得尝试一下这种组合。
