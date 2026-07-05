---
title: "OpenClaw 本地部署完整教程：Ollama + OpenClaw 免费 AI 助手"
date: 2026-06-29T15:00:00+08:00
draft: false
tags: ["OpenClaw", "Ollama", "AI助手", "本地部署", "教程", "免费AI", "本地AI"]
categories: ["AI 工具"]
summary: "通过 Ollama + OpenClaw 实现 100% 本地运行的 AI 助手，免费、断网可用、多模型切换，支持 GPT-OSS / Qwen 3 / GLM 4.7 等主流大模型。"
description: "OpenClaw 本地部署完整教程：Ollama + OpenClaw 免费 AI 助手。通过 Ollama + OpenClaw 实现 100% 本地运行的 AI 助手，免费、断网可用、多模型切换，支持 GPT-OSS / Qwen 3 / GLM 4.7 等主流大模型。"
keywords: ["openclaw 本地部署教程", "openclaw ollama", "openclaw 免费", "本地 ai 助手", "ollama 部署"]
---

## 前言

今天我们就来系统讲清楚一件很多人关心、但网上一直说不明白的事：**如何通过本地模型部署 OpenClaw，实现真正 100% 本地运行的 AI 助手**。

整个过程无需付费、无需任何 API Key，并且在完全断网的情况下依然可以正常使用。更关键的是，OpenClaw 不仅支持部署，还支持自由切换不同的本地大模型——无论是 20B / 120B 的 GPT-OSS 开源模型，还是目前主流的 Qwen 3、GLM 4.7，甚至 Kimi 等模型，都可以在同一套环境中灵活使用。

![OpenClaw 部署效果](/images/openclaw/1.webp)

接下来我们就通过 **Ollama + OpenClaw 本地 AI 实战**：免费、断网可用、多模型切换！支持 GPT-OSS / Qwen 3 / GLM 4.7 等主流大模型！

![实战演示](/images/openclaw/2.webp)

## 一、前期环境准备

### 1、安装 Git

以管理员身份打开 PowerShell，执行下方的安装命令，或者你可以直接去官网下载安装包：

```powershell
winget install git.git
```

如果你执行命令后出现任何错误，可以通过下方的命令进行解决：

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### 2、安装最新版 Ollama 客户端

下载地址：[https://ollama.com](https://ollama.com)

最新版 Ollama 已经完全适配运行 OpenClaw。

![Ollama 安装](/images/openclaw/3.webp)

## 二、官方推荐的本地模型

OpenClaw 需要更大的上下文长度才能完成任务。**建议使用至少 64k 个 token 的上下文长度**。

以下是一些与 OpenClaw 兼容性良好的模型：

| 模型 | 描述 |
|------|------|
| qwen3-coder | 针对编码任务进行了优化 |
| glm-4.7 | 强大的通用模型 |
| glm-4.7-flash | 性能与速度兼顾 |
| gpt-oss:20b | 性能与速度兼顾 |
| gpt-oss:120b | 能力提升 |

模型下载命令：

```bash
ollama run gpt-oss:20b
```

## 三、安装最新版 OpenClaw

### 通用安装命令（macOS / Linux）：

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Windows 版安装命令：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

安装完成后，您可以使用 Ollama 直接启动 OpenClaw 来连接本地模型：

```bash
ollama launch openclaw
```

如果您想配置 OpenClaw 而不立即启动服务：

```bash
ollama launch openclaw --config
```

如果网关已经在运行，它将自动重新加载。

![启动界面](/images/openclaw/4.webp)

## 四、对接 Telegram 电报机器人

### 第一步：创建 Telegram Bot

打开你的 Telegram，搜索 `@BotFather`，发送 `/newbot`，来创建一个新的机器人，按提示设置：

- 给 Bot 起个名字，比如我设置为 `lingduopenclaw`
- 设置用户名（必须以 `bot` 结尾，比如 `lingduopenclawbot`）
- 最后会给你一串 Token：`8123121125:***`

![创建 Bot](/images/openclaw/5.webp)

### 第二步：输入 Token 进行对接

输入 token 进行对接，并进入到刚才创建的机器人里。第一次打开会显示还未正式对接，但是会在里面提供配对码，比如我的是 `Pairing code: DLW7HQ69`

![配对码](/images/openclaw/6.webp)

### 第三步：完成配对

现在只需重新打开一个新的 PowerShell 窗口，然后在里面输入配对命令即可：

```bash
openclaw pairing approve telegram 这里填写你的配对码
```

当你看到这个界面的话说明已经和 Telegram 配对成功了！

![配对成功](/images/openclaw/7.webp)

## 五、常用命令

### 重启后启动的命令：

```bash
ollama launch openclaw
```

### 彻底卸载并删除 OpenClaw：

```bash
openclaw gateway stop
openclaw uninstall
npm uninstall -g openclaw
```

## 总结

| 功能 | 说明 |
|------|------|
| **免费使用** | 无需付费，无需 API Key |
| **断网可用** | 100% 本地运行 |
| **多模型切换** | GPT-OSS / Qwen 3 / GLM 4.7 等 |
| **多平台支持** | Windows / macOS / Linux |
| **消息平台对接** | Telegram、微信、QQ 等 |
| **上下文长度** | 建议至少 64k token |

OpenClaw 是一个非常强大的开源 AI 助手项目，通过本地部署可以实现完全免费、断网可用的 AI 助手。如果你对 AI 感兴趣，不妨试试这个方案。

## 参考资料

- [OpenClaw 官网](https://openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [OpenClaw 文档](https://docs.openclaw.ai)
- [Ollama 官网](https://ollama.com)
