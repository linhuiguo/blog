---
title: "4G 显存跑 Claude Mythos 5？Qwythos-9B 突然开源，百万上下文免费用"
date: 2026-06-29T19:00:00+08:00
draft: false
tags: ["Qwen", "Claude", "开源模型", "本地部署", "推理"]
categories: ["AI 工具"]
summary: "Qwythos-9B 突然开源，吸收 Claude Mythos 5 推理能力，百万 Token 上下文，4GB 显存即可本地部署。"
---

## 前言

不得不说，现在开源 AI 模型的发展速度，正在变得越来越快！

就在前两天，一款名为 **Qwythos-9B-Claude-Mythos-5-1M** 的全新开源推理模型悄然发布，并迅速引起了本地 AI 社区的广泛关注。

与传统开源模型不同，Qwythos-9B 最大的亮点在于它**吸收了大量来自 Claude Mythos 和 Claude Fable 的高质量推理轨迹**，并通过专门的后训练技术，将这些推理能力迁移到一个仅有 9B 参数的开源模型之中。

![Qwythos-9B 介绍](/images/qwythos/1.webp)

更令人惊讶的是，它不仅支持**超过 104 万 Token 的超长上下文窗口**，同时还提供 GGUF 量化版本，**最低仅需 4GB 显存即可本地部署运行**。

那么这款被誉为"Claude Mythos 开源平替"的模型究竟表现如何？本文将带大家深入了解这款近期爆火的开源推理模型。

![Qwythos-9B 详情](/images/qwythos/2.webp)

## 一、什么是 Qwythos-9B？

Qwythos-9B 是一个基于 **Qwen3.5-9B 架构**训练的全参数推理模型。

开发团队 Empero AI 在深度未审查版本的 Qwen3.5-9B 基础上，使用超过 **5 亿条高质量 Claude Mythos 与 Claude Fable 推理轨迹**进行了后训练。

这些推理轨迹并非简单问答数据，而是包含完整思维链（Chain of Thought）的高质量推理过程，其逻辑链由 Empero AI 内部的 rethink 系统自动生成。

简单来说，你可以把它理解为：

> **将 Claude Mythos 的推理思维方式迁移到了一个仅有 9B 参数的开源模型之中。**

最终得到了一款兼顾推理能力、部署成本以及长上下文能力的新型开源模型。

## 二、本地部署要求

Qwythos 已提供 GGUF 格式，可通过以下工具直接运行：

- llama.cpp
- llama-server
- OpenWebUI
- Cherry Studio
- OpenClaw

不同量化版本对应显存需求如下：

| 显存 | 推荐量化 |
|------|----------|
| 4GB | Q4_K_M |
| 6GB | Q5_K_M |
| 8GB | Q6_K |
| 12GB | Q8_K_M |
| 16GB | BF16 |
| 24GB | MTP-BF16 |

如果使用 BF16 全精度版本，则建议至少配备 24GB 显存。

## 三、部署教程

### 1、下载 Qwythos 模型

下载地址：[HuggingFace - Qwythos-9B GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)

![模型下载](/images/qwythos/3.webp)

### 2、安装 llama.cpp

下载地址：[GitHub - llama.cpp](https://github.com/ggml-org/llama.cpp/releases)

![llama.cpp 安装](/images/qwythos/4.webp)

### 3、启动命令

根据你的显存选择对应的量化版本和上下文长度：

**4GB 显存（Q4_K_M）：**

```bash
llama-server.exe \
  -m "models\Qwythos-9B-Q4_K_M.gguf" \
  -ngl 999 \
  -c 16384 \
  -n 2048 \
  -fa on \
  --jinja \
  --host 127.0.0.1 \
  --port 8080
```

**8GB 显存（Q6_K）：**

```bash
llama-server.exe \
  -m "models\Qwythos-9B-Q6_K.gguf" \
  -ngl 999 \
  -c 65536 \
  -n 2048 \
  -fa on \
  --jinja \
  --host 127.0.0.1 \
  --port 8080
```

**12GB+ 显存（Q8_K_M）：**

```bash
llama-server.exe \
  -m "models\Qwythos-9B-Q8_K_M.gguf" \
  -ngl 999 \
  -c 131072 \
  -n 2048 \
  -fa on \
  --jinja \
  --host 127.0.0.1 \
  --port 8080
```

启动后 API 地址为 `http://127.0.0.1:8080/v1`，可对接：

- OpenWebUI
- Cherry Studio
- OpenClaw
- Hermes
- 任何 OpenAI 兼容客户端

## 四、总结

| 要点 | 说明 |
|------|------|
| **模型架构** | 基于 Qwen3.5-9B |
| **训练数据** | 5 亿+ Claude Mythos/Fable 推理轨迹 |
| **上下文长度** | 超过 104 万 Token |
| **最低显存** | 4GB（Q4_K_M 量化） |
| **推理能力** | 接近 Claude Mythos 水平 |
| **开源格式** | GGUF 量化版 |
| **兼容工具** | llama.cpp / OpenWebUI / Cherry Studio / OpenClaw |

Qwythos-9B 的出现，让普通用户也能在 4GB 显存的消费级显卡上体验接近 Claude Mythos 的推理能力。如果你对 AI 推理感兴趣，强烈建议试试这个模型。

## 参考资料

- [HuggingFace - Empero AI](https://huggingface.co/EmperoAI)
- [llama.cpp GitHub](https://github.com/ggml-org/llama.cpp)
- [OpenWebUI](https://openwebui.com)
