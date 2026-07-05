---
title: "白嫖 GPT-5.5！微软 Copilot 变成免费 API，本地 AI Agent 直接对接"
date: 2026-06-29T17:00:00+08:00
draft: false
tags: ["Copilot", "GPT-5", "免费API", "AI Agent", "OpenAI", "微软Copilot", "本地AI"]
categories: ["AI 工具"]
summary: "Windows-Copilot-API 开源项目将微软 Copilot 封装成 OpenAI 兼容 API，无需 API Key，免费调用 GPT-4o、GPT-5.5 等模型，直接对接本地 AI Agent。"
description: "白嫖 GPT-5.5！微软 Copilot 变成免费 API，本地 AI Agent 直接对接。Windows-Copilot-API 开源项目将微软 Copilot 封装成 OpenAI 兼容 API，无需 API Key，免费调用 GPT-4o、GPT-5.5 等模型，直接对接本地 AI Agent。"
keywords: ["copilot 免费 api", "微软 copilot api", "gpt-5.5 本地调用", "免费 ai api", "本地 ai agent"]
---

## 前言

微软的 Copilot 一直被认为是 Windows 未来最重要的 AI 产品之一。然而就在最近，GitHub 上出现了一个非常有意思的开源项目，它成功将 Windows Copilot 封装成了一个 **OpenAI 兼容 API**。

**可以直接白嫖 GPT-4o、GPT-5.5 等模型！甚至可以直接对接本地的 AI Agent 来执行自动化任务。**

![Copilot API 项目](/images/copilot-api/1.webp)

而且代码能力相当不错！下方是我让它一次性生成的 3D 场景：

![3D 场景效果](/images/copilot-api/2.webp)

这一切更令人意外的是：
- **无需 OpenAI API Key**
- **无需购买 ChatGPT Plus**
- 支持 OpenAI 标准接口
- 可直接接入 Cherry Studio、Open WebUI 等客户端

这个项目名为 **Windows-Copilot-API**，目前已经引起了大量 AI 开发者关注。

## 一、部署教程

### 环境要求

- Python 3.9+
- 一个微软账户（你用来注册 Copilot 的免费账户就可以）
- 可在 Windows、macOS 和 Linux 系统上运行

### 1、安装 Python

建议安装 Python 3.11 或以上版本。

下载地址：[https://www.python.org](https://www.python.org)

安装完成后验证：

```bash
python -V
```

### 2、下载开源项目

项目地址：[https://github.com/nicepkg/Windows-Copilot-API](https://github.com/nicepkg/Windows-Copilot-API)

### 3、部署命令

**第一步：开放 PowerShell 相应的权限**

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

**第二步：创建并激活虚拟环境**

Windows 系统（PowerShell）：

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

macOS / Linux 系统：

```bash
python3 -m venv venv
source venv/bin/activate
```

**第三步：安装依赖环境**

```bash
pip install -r requirements.txt
```

**第四步：安装浏览器自动化框架**

```bash
playwright install chromium
```

**第五步：登入 Copilot**

```bash
python -m copilot login
```

执行命令以后，它会自动弹出 Copilot 登入页面，你可以自行注册一个小号或直接登入都可以。

**注意**：登入以后**不要关闭**当前的弹窗页面，否则后面终端下无法自动授权。

![Copilot 登入](/images/copilot-api/3.webp)

登入 Copilot 以后，只需在终端下回车确认即可完成全部的登入授权。

![授权成功](/images/copilot-api/4.webp)

成功授权以后就可以免费调用 GPT-4o/5 模型了。

**第六步：正式启动**

```bash
python app.py
```

启动后就可以通过 `http://127.0.0.1:8000/v1` 来对接本地的 AI Agent。

![启动成功](/images/copilot-api/5.webp)

## 二、对接本地 AI Agent

传统情况下，如果你想调用 GPT 模型，通常需要：
- OpenAI API
- Azure OpenAI
- 第三方中转 API

而 Windows-Copilot-API 直接利用你已经登录的微软 Copilot 账户，将其转换成标准 OpenAI API。这意味着：**任何支持 OpenAI API 的软件，都可以直接连接到 Copilot**。

例如：
- Cherry Studio
- Open WebUI
- OpenClaw
- Hermes
- Python OpenAI SDK
- 各类 Agent 框架

几乎无需修改代码。这次我们就拿 **Cherry Studio** 来做测试，对新手来说非常友好，完全无代码部署，点点鼠标就能完成！

下载地址：[https://cherry-ai.com](https://cherry-ai.com)

![Cherry Studio](/images/copilot-api/6.webp)

对接的时候选择自定义其它服务提供商：
- **Base URL**：`http://127.0.0.1:8000/v1`
- **模型名称**：可以自定义，比如 `GPT5.5`
- **密钥 API**：随便填写一个即可，比如 `12345678`，但不能留空

![Cherry Studio 对接](/images/copilot-api/7.webp)

对接成功以后就可以正常使用了，比如制定自动化任务、定时任务、编写代码、制作文案等，都可以。

目前这个方式对接没有限额，但是**大家务必正常使用，切勿滥用**！

![正常使用](/images/copilot-api/8.webp)

## 三、测试 Copilot 是否正常工作

执行：

```bash
python -m copilot ask "你好"
```

如果返回类似：

```
你好，LING 🌟
看到你又来了，我挺开心的。
```

说明已经成功连接 Copilot。

经过实测，Cherry Studio 可以正常使用。但**不支持高并发**。

作者也明确说明：
> Copilot 聊天连接无法同时处理多个会话。所有请求都会排队执行。

因此：**它更适合作为个人使用方案，而不是生产级 API 服务**。

## 四、工作原理

需要说明的是：**这个项目并不是破解 GPT-5，也不是破解 OpenAI**。它本质上仍然是在调用微软官方 Copilot 服务。

工作流程如下：

```
用户程序
    ↓
Windows-Copilot-API
    ↓
Playwright（浏览器自动化）
    ↓
Copilot Web
    ↓
微软服务器
```

开发者利用 Playwright 自动控制浏览器，与 Copilot 网页端进行通信，然后将结果转换为 OpenAI API 格式返回。因此它本质上属于一种 **API 封装方案**。

## 五、微软的 AI 战略

就在这个项目爆火的同时，微软还确认：
- Edge Drop 文件分享功能即将下线
- Collections 收藏功能被移除
- Sidebar 侧边栏功能逐步淘汰

而 **Copilot 则被保留并持续强化**。

这释放出一个非常明显的信号：**微软正在把 Copilot 作为未来 Windows 的核心入口**。

从 Windows 11 开始：
- Copilot 进入系统
- 记事本加入 AI
- Paint 加入 AI
- Office 加入 AI
- Edge 全面整合 AI

未来用户或许不再需要寻找菜单和按钮，而是直接通过自然语言控制整个系统。微软真正想打造的，可能不是一个聊天机器人，而是一个 **AI 操作系统**。

## 总结

| 优点 | 缺点 |
|------|------|
| ✅ 免费使用 | ❌ 依赖 Copilot 网页端 |
| ✅ 无需 OpenAI API Key | ❌ 不支持高并发 |
| ✅ 支持 OpenAI 生态 | ❌ 未来存在失效风险 |
| ✅ 可接入 Cherry Studio | ❌ 不适合生产级 Agent |

**对于个人用户来说，这是一个非常值得尝试的项目。**

## 参考资料

- [Windows-Copilot-API GitHub](https://github.com/nicepkg/Windows-Copilot-API)
- [Cherry Studio](https://cherry-ai.com)
- [Playwright 文档](https://playwright.dev)
