---
title: "直接白嫖！GitHub 大神写的副业赚钱工具，自动发短视频+推商品+发推（保姆级）"
date: 2026-07-12T00:20:00+08:00
draft: false
tags: ["副业赚钱", "MoneyPrinter", "自动化", "YouTube Shorts", "联盟营销", "AI 工具", "白嫖", "保姆级教程"]
categories: ["副业"]
summary: "GitHub 31186 星，大神写的副业赚钱工具 MoneyPrinterV2，自动化发短视频、推商品、发推特，直接白嫖用。"
description: "MoneyPrinterV2 GitHub 大神真实副业赚钱工具，自动化 YouTube Shorts、Twitter 营销、Amazon 联盟营销，Python 开源免费。"
keywords: ["副业赚钱", "MoneyPrinterV2", "自动化", "YouTube Shorts", "联盟营销", "开源", "AI", "0149"]
---

# 直接白嫖！GitHub 大神写的副业赚钱工具，自动发短视频+推商品+发推（保姆级）

> 别再看"如何赚钱"的理论文章了，今天给你直接**抄代码**。
>
> GitHub 大神 **FujiwaraChoki** 写的 **MoneyPrinter V2**（★31,186 星，Python 开源），是一套**真正的自动化副业工具**：
>
> - 自动生成 YouTube Shorts 短视频，定时发布
> - 自动发推特推广内容
> - 自动推 Amazon 联盟商品赚佣金
> - 自动找本地商家，发冷邮件推广
>
> 作者甚至写了完整的 YouTube 教程视频，**GitHub 3 万多人已经 fork 在用了**。不是概念，是真能跑的代码。

## ⚠️ 先说清

1. **这是开源工具，不是赚钱保证**：工具帮你自动化，但能不能赚到钱看你运气和内容质量。
2. **需要 Python 3.12**：装好 Python 就能跑。
3. **需要 YouTube/Twitter 账号**：自动发布功能需要登录你的账号。
4. **AGPL-3.0 协议**：开源免费，可以商用，但改了的代码也要开源。

---

## 〇、准备清单

- 一台 Windows 电脑
- Python 3.12（[官网下载](https://www.python.org/downloads/)）
- 一个 YouTube 账号
- 一个 Twitter/X 账号
- Amazon 联盟账号（可选，赚佣金用）

---

## 一、第一步：检查 Python

```bash
python --version
```

必须是 **3.12 或更高**。如果显示 3.10/3.11，去官网重新下载 3.12。

---

## 二、第二步：克隆项目

```bash
git clone https://github.com/FujiwaraChoki/MoneyPrinterV2.git
cd MoneyPrinterV2
```

---

## 三、第三步：配置

```bash
# 复制配置模板
cp config.example.json config.json
```

打开 `config.json`，填上你的账号信息：

```json
{
  "youtube": {
    "client_id": "你的YouTube客户端ID",
    "client_secret": "你的YouTube客户端密钥"
  },
  "twitter": {
    "api_key": "你的Twitter API Key",
    "api_secret": "你的Twitter API Secret"
  }
}
```

---

## 四、第四步：安装依赖

```bash
# 创建虚拟环境
python -m venv venv

# 激活（Windows）
.\venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt
```

---

## 五、第五步：跑起来

```bash
python src/main.py
```

程序会自动：
1. 生成短视频内容
2. 发布到 YouTube Shorts
3. 定时发推特推广
4. 推 Amazon 联盟商品

---

## 六、工具能干嘛

| 功能 | 说明 | 能不能赚钱 |
|------|------|------------|
| **YouTube Shorts 自动发布** | 自动生成短视频并定时发布 | YouTube 分成 |
| **Twitter 自动推广** | 自动发推带商品链接 | 推流量赚佣金 |
| **Amazon 联盟营销** | 推商品赚佣金 | 直接佣金 |
| **本地商家推广** | 找商家发冷邮件 | 服务费/广告 |

---

## 七、配套资源

| 资源 | 地址 |
|------|------|
| **GitHub 仓库** | [github.com/FujiwaraChoki/MoneyPrinterV2](https://github.com/FujiwaraChoki/MoneyPrinterV2) |
| **官方视频教程** | [YouTube 教程视频](https://youtu.be/wAZ_ZSuIqfk) |
| **完整文档** | 仓库 `docs/` 目录 |
| **Discord 社区** | [dsc.gg/fuji-community](https://dsc.gg/fuji-community) |
| **中文版 MoneyPrinterTurbo** | [github.com/harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) ★96832 |

---

## ⚠️ 避坑总结

| 坑 | 现象 | 解法 |
|---|---|---|
| Python 版本不对 | 程序报错 `ModuleNotFoundError` | 重新装 Python 3.12 |
| YouTube API 申请失败 | 网站进不去 | 用 CC 代理翻墙申请 |
| Twitter API 申请失败 | 需要 Twitter 开发者账号 | 申请免费开发者计划 |
| 不知道填什么 | config.json 全是英文 | 打开仓库 `docs/` 目录看说明 |
| 不敢用怕封号 | 自动化发内容 | 先用小号测试，别上主号 |

---

## 一句话提醒

**工具是现成的，关键是你敢不敢跑起来。** 3 万多人已经在用，你还在等什么？

---

**我是先生靠谱，专讲小白能看懂的实操。**