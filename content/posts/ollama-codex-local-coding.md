---
title: "Ollama + Codex App 本地 AI 编程：断网也能自动写代码"
date: 2026-06-29T16:00:00+08:00
draft: false
tags: ["Ollama", "Codex", "AI编程", "本地部署", "Agent"]
categories: ["AI 工具"]
summary: "最新版 Ollama 正式支持接入 Codex App，本地大模型直接变身 AI 自动编程 Agent，断网也能用，零 API 费用。"
---

## 前言

过去很长一段时间里，很多人都认为，像 OpenAI Codex、Claude Code、Cursor Agent 这种 AI 编程工具，必须依赖云端运行。因为它们需要强大的模型推理能力，所以几乎都离不开 OpenAI API、Claude API 或者 Gemini API。也正因为如此，AI 编程虽然很强，但成本一直都不低。

![AI 编程工具](/images/ollama-codex/1.webp)

尤其是大型项目。一次完整的代码分析、项目扫描、Agent 推理，往往就会消耗大量 Token。很多开发者可能只是测试几个小时，API 费用就已经开始快速上涨。

但现在，这件事情开始发生变化了。因为**最新版的 Ollama，已经正式支持接入 Codex App**。也就是说，你本地运行的大模型，现在已经可以直接变成 AI 自动编程 Agent。

![Ollama + Codex](/images/ollama-codex/2.webp)

而且最离谱的是：**整个过程，甚至不需要联网**。

## 一、本地 AI Agent 的能力

以前很多人对本地大模型的印象，其实还停留在"聊天机器人"阶段。比如本地运行一个 Qwen、DeepSeek、Gemma，然后进行简单对话、文本生成、代码补全等等。

但现在已经完全不同了。因为 **AI Agent 和普通聊天机器人，本质上是两回事**。聊天机器人只能回答问题，但 Agent 已经开始"执行任务"了。

比如：
- 自动分析项目结构
- 自动扫描代码
- 自动寻找 Bug
- 自动修改文件
- 自动创建项目
- 甚至自动操作浏览器

这意味着，**本地 AI 已经开始真正具备"干活"的能力**。

![AI Agent 能力](/images/ollama-codex/3.webp)

## 二、实测：修复崩掉的游戏项目

我这次测试的时候，最让我震惊的，并不是 AI 能聊天，而是它真的开始接管电脑了。

比如我故意准备了一个已经崩掉的空战游戏项目。这个游戏原本已经报错，甚至无法正常运行。

正常情况下，如果是人工修复，我们可能需要：
1. 先查看控制台报错
2. 再检查代码逻辑
3. 然后逐步定位问题
4. 最后再尝试修复

但这次，我直接把整个项目丢给了 AI Agent。

结果它会自动开始：
1. 扫描项目文件
2. 分析代码结构
3. 定位错误逻辑
4. 自动修改代码
5. 修复 Bug
6. 最后重新运行整个游戏

最离谱的是，**修复完成之后，游戏居然真的恢复正常运行了**。整个过程，几乎不需要人工干预。

![修复游戏](/images/ollama-codex/4.webp)

而且这还不是最夸张的。真正让我觉得离谱的是：**哪怕断网，它依然可以继续工作**。因为它调用的是我本地 GPU 上的大模型。整个 AI 推理过程，全部都在本地完成。没有任何 OpenAI API，也没有任何 Token 消耗。

## 三、本地部署步骤

### 1、安装 OpenAI Codex

下载地址：[OpenAI Codex 官网](https://codex.openai.com)

如果你下载的是 macOS 版，注意选择 Intel 或 M 芯片版本。

### 2、安装新版 Ollama

**目前只有最新版 Ollama 0.24 版本才完全适配 Codex**，所以如果你安装的是旧版 Ollama，一定要将其升级到最新版。

下载地址：[https://ollama.com](https://ollama.com)

### 3、下载模型

在 4B~40B 消费级显卡能跑的开源模型，首推 **Qwen3.6** 以及谷歌的 **Gemma 4** 开源模型，因为无论是模型智力、代码编写、逻辑推理、中文理解等方面，这两款模型的综合评分都是数一数二的！

![推荐模型](/images/ollama-codex/5.webp)

**Qwen3.6 开源模型**

安装命令：

```bash
ollama run qwen3.6
ollama run qwen3.6:27b
```

Mac 电脑上请选择 mlx 结尾的适配版：

```bash
ollama run qwen3.6:27b-mlx
ollama run qwen3.6:35b-mlx
```

**Gemma 4 开源模型**

安装命令：

```bash
ollama run gemma4
ollama run gemma4:26b
ollama run gemma4:31b
```

Mac 电脑可选模型：

```bash
ollama run gemma4:e2b-mlx
ollama run gemma4:e4b-mlx
ollama run gemma4:26b-mlx
```

### 4、对接命令

```bash
ollama launch codex-app
```

注意：如果需要使用之前的模型，可以通过下方的命令进行恢复：

```bash
ollama launch codex-app --restore
```

## 四、更强玩法：通过 llama.cpp 对接越狱版模型

### 1、修改 Codex 的配置文件

```toml
model = "Qwen3.6-27B-UD-Q5_K_XL.gguf"
model_reasoning_effort = "low"
profile = "llamacpp-codex"
model_provider = "llamacpp"

[profiles.llamacpp-codex]
model = "Qwen3.6-27B-UD-Q5_K_XL.gguf"
model_provider = "llamacpp"
model_reasoning_effort = "low"

[profiles.llamacpp-codex.windows]
sandbox = "elevated"

[model_providers.llamacpp]
name = "llama.cpp"
base_url = "http://127.0.0.1:8080/v1/"
wire_api = "responses"

[windows]
sandbox = "elevated"
```

### 2、llama.cpp 的启动命令

```bash
llama-server.exe ^
  -m "models\Qwen3.6-27B-UD-Q5_K_XL.gguf" ^
  -ngl 999 ^
  -c 16384 ^
  -n 2048 ^
  -fa on ^
  --jinja ^
  --host 127.0.0.1 ^
  --port 8080
```

里面的模型改成你自己的。

## 五、硬件门槛

现在本地 AI 的硬件门槛，其实已经没有大家想象中那么高了。

很多人以前一提到 AI Agent，第一反应就是：
- 必须 RTX 4090
- 必须 80G 显存
- 必须企业级 GPU

但实际上，现在很多小模型已经完全可以胜任基础 AI 编程任务。最低 **6G、8G 显存**，现在都已经可以跑起来了。

| 模型 | 显存需求 | 速度 |
|------|----------|------|
| Qwen 3.6 7B | 6G | 快 |
| Qwen 3.6 14B | 8G | 中等 |
| Qwen 3.6 27B | 16G | 较慢 |
| Gemma 4 26B | 16G | 较慢 |

虽然速度肯定没办法和 4090 相比，但对于很多普通用户来说，已经足够体验"本地 AI 自动编程"这件事情了。

## 六、更多玩法

除了修 Bug 之外，我还测试了其他场景：

### 自动开发小游戏

让 AI 帮我做一个打地鼠小游戏。结果 AI 会自动创建 HTML、CSS、JavaScript 文件，甚至连 UI 界面和游戏逻辑都会一起完成。几分钟时间，一个小游戏居然真的能运行起来。

### 自动创建网页

让它创建一个苹果官网风格的 AI 产品首页。结果 AI 自动完成了页面布局、动画、响应式设计、UI 风格，甚至还会自动调整细节。最终效果，已经开始接近商业级网页设计了。

### 自动操作浏览器

现在很多 Agent 已经不仅仅局限于代码开发，它甚至还能自动打开浏览器、自行搜索、自行浏览网页、自行下载文件，然后自动完成整个操作流程。

## 总结

| 要点 | 说明 |
|------|------|
| **核心变化** | Ollama 正式支持 Codex App |
| **本地运行** | 100% 本地，断网可用 |
| **零费用** | 无需 OpenAI API，无 Token 消耗 |
| **推荐模型** | Qwen3.6、Gemma 4 |
| **硬件门槛** | 最低 6G 显存即可 |
| **AI 能力** | 自动编程、修 Bug、创建项目、操作浏览器 |

AI 的真正方向，可能根本不是聊天，而是 **Agent** —— 真正帮你执行任务的 AI。而 Ollama，现在正在成为整个本地 AI 生态里非常核心的一环。

## 参考资料

- [Ollama 官网](https://ollama.com)
- [OpenAI Codex](https://codex.openai.com)
- [Qwen3.6 模型](https://ollama.com/library/qwen3.6)
- [Gemma 4 模型](https://ollama.com/library/gemma4)
