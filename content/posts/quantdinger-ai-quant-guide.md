---
title: "AI 量化交易平台白嫖！开源免费，回测+实盘，小白也能跑（保姆级）"
date: 2026-07-12T00:05:00+08:00
draft: false
tags: ["量化交易", "AI 量化", "QuantDinger", "Python", "开源", "白嫖", "保姆级教程"]
categories: ["AI工具"]
summary: "QuantDinger 开源 AI 量化交易平台，支持加密货币、股票、外汇，回测+实盘，GitHub 9472 星，Apache-2.0 免费。"
description: "QuantDinger 开源 AI 量化交易平台保姆级教程，Python 实现，支持 Binance/Alpaca 实盘交易，内置回测、市场数据、AI 策略模板，免费开源。"
keywords: ["量化交易", "AI 量化", "QuantDinger", "开源量化", "免费", "Binance", "Alpaca", "0149"]
---

# AI 量化交易平台白嫖！开源免费，回测+实盘，小白也能跑（保姆级）

> GitHub 9472 星，一个完全开源的 AI 量化交易平台——**QuantDinger**。支持加密货币、股票、外汇，内置回测、AI 策略、实盘交易，还能接 Binance 和 Alpaca。Apache-2.0 协议，**商业也能免费用**。

## ⚠️ 先说清

1. **量化≠稳赚**：回测好不代表实盘赚，本文只讲怎么用，不保证收益。
2. **需要 Python 3.10+**：装好 Python 就能跑。
3. **可以跑加密货币**：接 Binance 实盘；也可以跑股票，接 Alpaca（美股）。
4. **开源免费**：Apache-2.0 协议，代码随便用，改都行。

---

## 〇、准备清单

- 一台 Windows 电脑
- Python 3.10+
- 一个交易所账号（Binance 或 Alpaca，用于实盘；回测不需要）

---

## 一、第一步：检查 Python

打开命令提示符（开始菜单搜 `cmd` 回车）：

```bash
python --version
```

显示 `Python 3.10` 或更高就行。如果没有，去 [python.org](https://python.org) 下载安装。

---

## 二、第二步：克隆项目

```bash
git clone https://github.com/brokermr810/QuantDinger.git
cd QuantDinger
```

> ⚠️ 没装 Git？去 [git-scm.com](https://git-scm.com/) 下载，一路下一步。

---

## 三、第三步：安装依赖

```bash
pip install -r requirements.txt
```

如果速度慢，换国内镜像：

```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

---

## 四、第四步：配置交易所

编辑 `config.json`（如果不存在，复制 `config.example.json` 改个名字）：

```json
{
  "exchange": "binance",
  "api_key": "你的Binance API Key",
  "api_secret": "你的Binance API Secret",
  "testnet": true
}
```

> ⚠️ 先在 Binance 申请测试网 Key（免费，不用真钱），`testnet: true` 就是模拟盘，亏了也不心疼。

---

## 五、第五步：跑回测

```bash
python backtest.py --symbol BTCUSDT --strategy sma_cross --start 2024-01-01 --end 2024-12-31
```

几秒后输出回测结果：收益率、最大回撤、胜率。

---

## 六、它能干嘛

| 能力 | 说明 |
|------|------|
| **回测引擎** | 历史数据回测策略效果 |
| **AI 策略** | 内置机器学习模型训练 |
| **实盘交易** | 接 Binance（加密）/ Alpaca（美股） |
| **市场数据** | 内置 K 线、OHLCV 数据获取 |
| **风控模块** | 止损、仓位管理、风险预警 |

---

## ⚠️ 避坑总结

| 坑 | 现象 | 解法 |
|---|---|---|
| API Key 申请失败 | 网站进不去 | 用 CC 代理或海外 VPS 申请 |
| 依赖安装慢 | pip 一直卡着 | 换清华镜像 `-i https://pypi.tuna.tsinghua.edu.cn/simple` |
| 回测数据为空 | 没数据下载 | 检查网络，或用 `--download` 参数拉历史数据 |
| 测试网 Key 找不到 | 在 Binance 没找到 testnet | 直接搜 "Binance testnet API"，单独申请 |

---

## 一句话提醒

**回测是回测，实盘是实盘。** 先在 testnet 跑一周模拟盘，看看策略稳不稳，再考虑真金白银。

---

**我是先生靠谱，专讲小白能看懂的实操。**