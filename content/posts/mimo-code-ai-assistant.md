---
title: "小米开源了 AI 编程助手！一行命令白嫖，Windows 也能用（保姆级）"
date: 2026-07-11T23:45:00+08:00
draft: false
tags: ["MiMo Code", "小米", "AI 编程", "开源", "白嫖", "保姆级教程", "Windows"]
categories: ["AI工具"]
summary: "小米开源了 MiMo Code，一行命令就能装、本地跑 AI 编程助手，Windows 原生支持。小白照抄就能用。"
description: "小米开源 MiMo Code 保姆级安装教程，从检查 PowerShell 到一行命令装好，Win10/Win11 差异已标出。小白也能用的 AI 编程助手。"
keywords: ["MiMo Code", "小米开源", "AI 编程助手", "免费", "Windows", "0149"]
---

# 小米开源了 AI 编程助手！一行命令白嫖，Windows 也能用（保姆级）

> 你可能还不知道，**小米**最近开源了一个 AI 编程助手 **MiMo Code**（GitHub 11,000+ 星），本地运行、数据不出门、不用开会员。最关键是——**Windows 一行命令就能装好**。
> 本文从"连 PowerShell 是啥都不知道"讲起，**每一步都给小白，Win10 和 Win11 不一样的地方我标出来**。

## ⚠️ 先说清

1. **MiMo Code 不是给普通用户写文章的**：它是给程序员写代码用的 AI 助手，需要在终端里以对话方式用。如果你完全不会任何编程，这一篇可能暂时用不上——但可以先收藏，以后想学编程时就是你的免费老师。
2. **完全免费开源**：MIT 协议，不收费、没广告。
3. **本文不含小米云服务**：MiMo Code 是本地工具，不会把你的代码上传。

---

## 〇、准备清单

- 一台 Windows 10 或 Windows 11 电脑
- 能联网
- 不用会任何编程（照做就行）

---

## 一、第一步：打开管理员 PowerShell

MiMo Code 只需要**一行命令**就能装好，但这行命令要在"管理员 PowerShell"里敲。

**Win11**：点底部"开始"图标 → 在上面点**右键** → 选 **"终端(管理员)"**。
**Win10**：点左下角"开始" → 点**右键** → 选 **"Windows PowerShell(管理员)"**。

> 如果右键没有"管理员"选项，就在开始里搜"PowerShell"（Win11 搜"终端"），搜到后**右键它 → 以管理员身份运行**。

### 先检查 PowerShell 能不能跑脚本（Win10/Win11 一样）

首次运行 PowerShell 脚本可能被拦，先解锁：
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```
提示问[Y/N]就按 `Y` 回车。

---

## 二、第二步：一行命令装 MiMo Code

在管理员 PowerShell 里敲：
```powershell
powershell -ep Bypass -c "irm https://mimo.xiaomi.com/install.ps1 | iex"
```
等它跑完（Win10/Win11 过程一样），会自动装好 MiMo Code。

> 装完后，在 PowerShell 里敲 `mimo` 就能启动 AI 编程助手。
> 如果你有 Node.js 环境，也可以用 `npm install -g @mimo-ai/cli` 装（效果一样）。

---

## 三、第三步：启动并验证

```powershell
mimo
```
如果显示欢迎信息和版本号，说明装上了。之后你就可以在终端里直接跟它对话、让它帮你写代码。

---

## 四、⚠️ 避坑总结

| 坑 | 现象 | 解法 |
|---|---|---|
| PowerShell 跑脚本被拦 | 报"禁止运行脚本" | `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` |
| 不是管理员运行 | 安装报权限错 | 右键 PowerShell → 以管理员身份运行 |
| 装完 `mimo` 找不到命令 | 未自动加 PATH | 重开 PowerShell 再试；或 `refreshenv` |

---

**更多 AI 白嫖教程，来我博客 👉 0149.cc.cd**
我是先生靠谱，专讲小白能看懂的实操。