---
title: "百度开源了无限 OCR！图片文字随便识别，免费（需要 GPU，小白慎入）"
date: 2026-07-11T23:55:00+08:00
draft: false
tags: ["百度", "Unlimited-OCR", "OCR", "免费", "开源", "保姆级教程"]
categories: ["AI工具"]
summary: "百度最新开源的 Unlimited-OCR，一张图随便识别文字，免费开源。但需要 N 卡 GPU，小白先看显卡再装。"
description: "百度 Unlimited-OCR 保姆级安装教程，从检查显卡到一行命令跑通，需要 Python 3.12 + 英伟达 GPU，没有 GPU 可以用在线 Demo。"
keywords: ["百度 OCR", "Unlimited-OCR", "免费 OCR", "图片文字识别", "开源", "0149"]
---

# 百度开源了无限 OCR！图片文字随便识别，免费（需要 GPU，小白慎入）

> 百度最近在 GitHub 开源了一个叫 **Unlimited-OCR** 的东西（13,900+ 星），一句话说就是：**一张图扔进去，不管多少文字，它都能给你识别出来**，而且是免费的、无限次用。
> 不过**这个需要一台有 N 卡（英伟达显卡）的电脑**，没显卡也能用——后面教你用在线 Demo 体验。本文实话实说，不忽悠。

## ⚠️ 先说清

1. **需要一台有英伟达显卡的电脑**：GTX 1060 以上都行，显卡越好跑越快。**没有显卡也能用在线 Demo**（见文末）。
2. **Python 环境**：需要 Python 3.12，如果你已经装了 OpenClaw 那篇的 Python 3.12，直接复用。
3. **百度官方出品**：MIT 开源协议，免费、可商用，不限次数。

---

## 〇、准备清单

- 一台有英伟达显卡的 Windows 10/11 电脑（GTX 1060 及以上）
- 安装了 Python 3.12（参考前面 OpenClaw 教程的 1.4 节）
- 显卡驱动已更新到最新版

---

## 一、第一步：先检查有没有显卡

```powershell
nvidia-smi
```
如果显示了显卡型号和 CUDA 版本（如 `CUDA Version: 12.x`），说明有 N 卡且驱动正常。如果提示"不是内部或外部命令"，说明没有显卡驱动或没有 N 卡——**跳转到文末"在线 Demo"那节走在线体验**。

---

## 二、第二步：安装依赖

有显卡的朋友继续。在管理员 PowerShell 里逐条敲：

```powershell
# 安装 PyTorch（CUDA 版）
pip install torch==2.10.0 torchvision==0.25.0

# 安装其他依赖
pip install transformers==4.57.1 Pillow==12.1.1 matplotlib==3.10.8 einops==0.8.2 addict==2.4.0 easydict==1.13 pymupdf==1.27.2.2 psutil==7.2.2
```

> 装完第一步后，用 `python -c "import torch; print(torch.cuda.is_available())"` 验证 GPU 是否可用——输出 `True` 说明显卡就绪。

---

## 三、第三步：运行 OCR

```python
from transformers import pipeline

ocr = pipeline("image-text-recognition", model="baidu/Unlimited-OCR")
result = ocr("你的图片路径.jpg")
print(result)
```

把 `你的图片路径.jpg` 换成你要识别的图片文件路径，即可得到识别结果。

---

## 四、在线 Demo（没有显卡也能体验）

百度把 Unlimited-OCR 也放到了 HuggingFace 上，**没有显卡也能在线试用**：

1. 浏览器打开 https://huggingface.co/baidu/Unlimited-OCR
2. 找到 **Hosted inference API** 或 **Spaces** 入口
3. 上传一张图片，点运行，等几秒出结果

> 在线版速度比本地慢，但胜在不用装任何东西，适合小白先体验。

---

## 五、⚠️ 避坑总结

| 坑 | 现象 | 解法 |
|---|---|---|
| 没有 N 卡 | nvidia-smi 报错 | 用在线 Demo 体验 |
| CUDA 版本不对 | torch 报错 | 检查显卡驱动，更新到最新 |
| pip 找不到包 | 版本号写错了 | 检查 Python 版本（需 3.12） |
| 模型下载慢 | 卡在 "Downloading..." | 换个网络或挂梯子 |

---

**更多 AI 白嫖教程，来我博客 👉 0149.cc.cd**
我是先生靠谱，专讲小白能看懂的实操。