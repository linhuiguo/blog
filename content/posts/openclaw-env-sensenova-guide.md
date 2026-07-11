---
title: "从零配置 OpenClaw 环境 + 接入商汤「日日新」模型（Win10/Win11 保姆级）"
date: 2026-07-11T23:00:00+08:00
draft: false
tags: ["OpenClaw", "SenseNova", "日日新", "商汤", "AI", "保姆级教程", "Windows"]
categories: ["AI教程"]
summary: "手把手从零检查 Win10/Win11 底层依赖，安装 OpenClaw，注册商汤日日新并接入模型。每一步都给小白看，Win10 和 Win11 不一样的地方专门标出。"
description: "Windows 从零配置 OpenClaw 环境并接入商汤日日新（SenseNova）模型的保姆级教程，包含 winget/Git/Node/Python/C++ 依赖检查、OpenClaw 安装、交互式选择器配置 Custom Provider、API Key 获取等完整步骤。"
keywords: ["OpenClaw 配置", "商汤日日新", "SenseNova", "Windows 环境配置", "OpenClaw 教程", "保姆级教程", "0149"]
---

> 你是不是也烦了：想用 AI，要么开会员，要么怕数据上传？今天手把手教你**从零把 OpenClaw 跑起来**，并接上国产大模型**商汤「日日新」SenseNova**——国内节点、中文友好、注册就能拿 Key。
> 本文只讲两件事：**第一部 检查并装好所有运行环境** + **第二部 注册日日新并把模型接进 OpenClaw**。从"连 Node 是啥都不知道"讲起，**每一步都给小白，Win10 和 Win11 不一样的地方我专门标出来**。

## ⚠️ 先说清两件事

1. **软件免费，但"脑子"要 Key**：OpenClaw 是壳，真正回答问题的是大模型（LLM）。本文用**商汤日日新 SenseNova**（国产、国内节点、注册就能拿 API Key，对新手最友好）。
2. **本文不含微信接入**：OpenClaw 还能接微信/飞书等，那是另一块内容，本文先不展开，专注"环境 + 模型"这两步打牢。

---

## 〇、准备清单

- 一台 Windows 10（1809 及以上）或 Windows 11 电脑
- 能联网
- 一个手机号（注册商汤日日新用，收验证码）
- 一点点耐心，照着抄命令就行

---

# 第一部：检查所有环境 + 安装环境 + 安装 OpenClaw

## 一、第一步：检查并安装所有底层依赖（最关键，一步都不能少）

OpenClaw 主程序只硬性要求 **Node.js**，但为了后续少踩坑（ClawHub 拉技能、npm 编译原生模块、从源码装都要用到其他工具），我们把"包管理器、Git、Node、npm、Python、C++ 运行库"一次性查全、装全。下面每条都教你怎么**先看有没有，再装**。

### 1.0 先认清楚：你是 Win10 还是 Win11？（后面入口不一样）

- **Win11**：点屏幕底部"开始"图标（或按 `Win` 键）→ 在"开始"上点**右键** → 选 **"终端(管理员)"**。
- **Win10**：点屏幕左下角"开始" → 在"开始"上点**右键** → 选 **"Windows PowerShell(管理员)"**。
- 不管哪个，弹出的蓝框都叫"管理员终端/PowerShell"，**后面所有命令都在这里面敲**。

> 小白提示：如果右键没有"管理员"选项，就在开始里搜"PowerShell"（Win11 搜"终端"），搜到后**右键它 → 以管理员身份运行**。

### 1.1 检查 winget（Windows 官方包管理器）

winget 是用来"一行命令装软件"的工具，后面装 Node、Python、Git 都靠它。

**① 检查有没有**
在管理员 PowerShell 里敲：
```powershell
winget --version
```
- 显示 `v1.x.x` 之类版本号 → 已装，跳到 1.2
- 提示"winget 不是内部或外部命令" → 没装，继续

**② 安装 winget（分系统）**

- **Win11**：默认就带，基本不会走到这步。
- **Win10（重点差异）**：Win10 只有在 **1809 版本以上且微软商店正常**时才自带。很多老 Win10 / 精简版没有，需要手动装：
  - **最简办法**：打开 Microsoft Store（微软商店）→ 搜 **"应用安装程序"**（App Installer）→ 点"获取/更新"。装完重开 PowerShell，再 `winget --version` 验证。
  - **商店被禁用时（手动 sideload）**：去 GitHub 搜 `microsoft/winget-cli` 的 Releases，下载 `Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle` 和依赖包（VCLibs、UI.Xaml），用 `Add-AppxPackage` 依次安装。小白优先用上面商店法，别硬刚手动。

**③ 顺手设置：允许运行脚本（Win10/Win11 都要）**
首次跑 PowerShell 脚本可能被拦，先解锁：
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```
提示问你[Y/N]就按 `Y` 回车。这条 Win10、Win11 命令完全一样。

### 1.2 检查 Git（**ClawHub 拉技能 / 从源码装 / npm 编译原生模块都要它**）

说明：OpenClaw 的**一键安装脚本本身不强制 Git**，但只要你以后想用 ClawHub 技能市场、从 GitHub 源码装、或遇到 npm 提示"编译 C++ 原生模块"（node-gyp 需要 Git + Python + C++ 工具链），就离不开 Git。所以基础清单里必须有它。

**① 检查**
```powershell
git --version
```
- 显示 `git version 2.x.x` → 已装，跳 1.3
- 提示找不到命令 → 没装，继续

**② 安装（Win10 / Win11 命令一样，但都默认不预装，都得装）**
```powershell
winget install Git.Git
```
装完**关掉 PowerShell 重开**，敲 `git --version` 验证。
> Win10 和 Win11 都**不自带 Git**（Win11 自带的是 winget，不是 git），别以为 Win11 就不用装——这点两个系统一样，都得手动装。

### 1.3 检查 Node.js 和 npm（**OpenClaw 必需**）

OpenClaw 是用 JavaScript（Node）写的，没 Node 跑不起来。npm 是 Node 自带的"软件商店"，一起查。

**① 检查**
```powershell
node -v
npm -v
```
- `node` 显示 `v24.x.x`（或 `v22.19.x` 以上）→ 已装，看 npm 也有版本号就跳 1.4
- 提示找不到命令 → 没装，继续

**② 安装（Win10 / Win11 命令一样）**
```powershell
winget install OpenJS.NodeJS.LTS
```
装完**关掉 PowerShell 重开**，再敲 `node -v` 和 `npm -v` 验证。两个都出版本号 = 成功。

> 备选：去 nodejs.org 下载 LTS 安装包双击下一步到底也行（Win10/Win11 通用）。

### 1.4 检查 Python（**OpenClaw 不强制，但强烈建议装**）

说明：OpenClaw 主程序**不需要 Python**。但以后你想自己写小脚本调 AI、或装某些 AI 工具链，Python 几乎是标配；而且**npm 编译原生模块（node-gyp）也需要 Python**。所以趁现在顺手装了。

**① 检查**
```powershell
python --version
```
或（Win10/Win11 装了官方 Python 后都有 `py` 启动器）：
```powershell
py --version
```
- 显示 `Python 3.x.x` → 已装，跳 1.5
- 提示找不到 → 没装，继续

**② 安装（Win10 / Win11 命令一样）**
```powershell
winget install Python.Python.3.12
```
装完**重开 PowerShell**，敲 `python --version` 验证。
> ⚠️ **关键坑（Win10/Win11 都一样）**：用官网安装包装时，**第一屏务必勾选 "Add python.exe to PATH"**，不勾后面命令会找不到 python。winget 装的会自动加好 PATH，省心。

### 1.5 检查 C++ 运行库 / 构建工具（**编译原生模块用，保险步骤**）

- **运行库（Redistributable）**：跑别人编译好的 `.node` 二进制需要它。
- **构建工具（MSVC）**：只有你要**自己从源码编译 npm 原生模块**才需要，普通用户用不到，列在最后当备胎。

**① 检查运行库有没有**
```powershell
winget list | findstr /i "Visual C++"
```
- 列表里能看到一串 `Microsoft Visual C++ 20xx Redistributable` → 已装
- 啥都没有 → 继续装

**② 安装运行库（Win10 / Win11 命令一样）**
```powershell
winget install Microsoft.VCRedist.2015+.x64
```
若提示找不到这个包名，就去微软官网搜"Visual C++ Redistributable"下载最新的 x64 版双击安装。

**③ 需要"自己编译"怎么办（进阶，小白先跳过）**
如果某天 npm 报错说"需要 Windows SDK / MSVC"，那是 node-gyp 要编译。这时才装"Visual Studio 生成工具"里的 **"使用 C++ 的桌面开发"** 工作负载（含 MSVC + Windows SDK）。普通安装用不到，别提前装。

> 小白一句话：1.5 是备胎，装运行库不亏，构建工具等你真报错再装。

### ✅ 第一步验收

在 PowerShell 里依次敲下面五条，全部出版本号，第一部第一步就算过关：
```powershell
winget --version
git --version
node -v
npm -v
python --version
```

---

## 二、第二步：安装 OpenClaw（一条命令）

在刚才的管理员 PowerShell 里敲：
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```
脚本会自动检测系统、补装缺的东西、并启动初始化向导。等它跑完（Win10/Win11 过程一样）。

> 完全不想碰命令行？Windows 用户去 GitHub 的 releases 页下载 **OpenClaw Companion** 桌面版（带图形界面，点点点就能装），适合纯小白。

装完验证：
```powershell
openclaw --version
```
出版本号 = 安装成功。第一部完成。

---

# 第二部：注册商汤「日日新」+ 配置模型

> 第一部只是把"空壳"装好，现在给它接上"脑子"（商汤日日新 SenseNova 的模型）。
> 为什么选日日新？它是**商汤科技**国产大模型，国内节点、中文友好、注册就能拿 API Key，对新手最省心。本文以它为主线。

## 三、第三步：注册商汤日日新并接入模型

商汤日日新的官方平台是 **https://platform.sensenova.cn/** 。我们分两步：先去网页注册、拿 API Key；再把 Key 接进 OpenClaw。

### 3.1 注册日日新、拿到 API Key（网页操作，Win10/Win11 一样）

**步骤 1：打开官网，手机号一步注册**

1. 浏览器打开 **https://platform.sensenova.cn/**（Edge/Chrome 都行），会自动跳到登录/注册页（地址栏形如 `platform.sensenova.cn/login?...`）。
2. 登录框默认选中 **「手机号登录 / 注册」**（浅紫底色那一项；另一项「账号密码登录」是给已有账号的，不用管）。
   > 页面左栏大标题是 **SenseNova · 原生多模态智能体模型**，下方有个白色胶囊标签 **「Token Plan 同步开启 免费公测」**（"免费公测"四字是紫色）——说明现在**能免费拿 API Key 试用**，小白零成本起步。
3. 在右侧表单填：
   - **手机号**：国家码默认 `+86`，框里输入你的手机号；
   - **短信验证码**：点 **「发送验证码」**，手机收到短信后把 6 位码填进去。
4. 点紫色 **「下一步」** 按钮 → 首次会自动注册并登录，进入控制台。

**步骤 2：进控制台，创建 API Key**

5. 登录后自动进**控制台**（顶部导航「模型 / Token Plan / 文档 / 控制台」，当前在「控制台」）。
6. 看左侧栏 **「订阅模式」** 分组下的 **「API Keys」**（浅紫高亮那一项，钥匙图标），点它。
7. 主区标题 **API Keys**，右上角紫色按钮 **「创建 API Key」** → 点它 → 起个名字（如 `openclaw`）→ 生成一串 `sk-...` 开头的密钥，表格里显示为掩码（`sk-h******XXXX`）。
   > ⚠️ **密钥只显示/可复制这一次！** 点表格里那行的**复制图标**（或创建成功弹窗里的复制），立刻存到记事本。关掉就再也看不到了，丢了只能回这里点「注销」再新建一个。
   > 顶部还有个紫色 **「公测 FREE」** 胶囊——目前是免费公测期，小白零成本拿 Key 试用，不用付费。

> 顺带记一句：商汤官方 API 的模型名是 `sensenova-6.7-flash-lite`（控制台"快速使用示例"里的 cURL 能看到），而在 OpenClaw 里它写成 `custom-token-sensenvova-cn/sensenvova-6.7-flash-lite`——同一模型，两种写法，别混。

### 3.2 把日日新接进 OpenClaw（两种起点，按你机器现状选）

> 重要：OpenClaw 的接入命令分两种情况——**机器从没配过**（走 `onboard` 引导）和**已经配过模型**（走 `configure`/`models set`）。两种情况界面完全不同，别搞混。都是在管理员 PowerShell 里操作（Win10/Win11 入口见 1.0）。

**情况一：你是全新安装，从没配过模型（首次 onboard）**

OpenClaw 官方对商汤有专门的 `tencent` provider 插件，用 TokenHub / TokenPlan 两条通道。首次接入跑：
```powershell
openclaw onboard --auth-choice tokenhub-api-key
```
按提示粘贴你的商汤 Key（屏幕不显示字符，正常），回车即存好。
> 备选通道：`--auth-choice tokenplan-api-key`（对应 `TOKENPLAN_API_KEY`）。两条都能用，看你在商汤后台拿到的是哪种 Key。

**情况二：你已经装过 OpenClaw、之前配过别的模型（本文实测场景）**

这时再跑 `openclaw onboard` **不会**重新走授权弹窗，而是进入一个 **Security disclaimer（安全须知）** 页面。真正改配置要跑 `openclaw configure`。

**已配过模型、确认/切换回日日新的正确做法**

1. 跑配置命令，确认现状：
```powershell
openclaw configure
```
   会显示 `Existing config detected` 和当前 Model/Gateway。其中 `Where will the Gateway run?` 是交互选择，**直接按回车用默认项 `Local (this machine)`**（本机运行最稳），小白无脑回车即可。
2. 把默认模型设成日日新：
```powershell
openclaw models set custom-token-sensenvova-cn/sensenvova-6.7-flash-lite
```
   > 实测：直接敲上面命令会进入**交互式选择器**，而不是一步到位。选择界面顶部有几个必填项（红色 `*` 标记）：
   > ```
   > * Where will the Gateway run?        → Local (this machine)
   > * What do you want to configure?     → Model
   > * Model/auth provider
   >   > OpenAI (ChatGPT/Codex sign-in or API key)   ← 当前高亮
   >     Anthropic
   >     xAI (Grok)
   >     Google
   >     OpenRouter
   >     More**
   >     Skip for now
   > ```
   > 用 **↓ 方向键** 移到 **`More…`**（绿色 `>` 箭头），按 **回车** 展开完整厂商列表（带搜索框）。在展开列表里找 **`> Custom Provider (Any OpenAI or Anthropic compatible endpoint)`**（SenseNova 是 OpenAI 兼容接口，所以走 Custom 最稳），回车选中。
   >
   > 选定 Custom Provider 后，逐项确认：
   > - **Model/auth provider** → `Custom Provider`
   > - **API Base URL** → 填 **`https://token.sensenova.cn/v1`**（商汤官方的 OpenAI 兼容 `/v1` 端点，照抄别改）
   > - **How do you want to provide this API key?** → 选 `Paste API key now`
   > - **API Key** → 粘贴 `sk-...`（屏幕不显示字符，正常）
   > - **Endpoint compatibility** → 显示 `Unknown (detect automatically)`（自动检测，直接回车）
   > - **Model ID** → 填 **`SenseNova 6.7 Flash-Lite`**（自由文本框，照抄），回车即完成配置。
   >
   > ⚠️ **模型名坑**：OpenClaw 这里填的是显示名 `SenseNova 6.7 Flash-Lite`；商汤官方 API 的真实模型 ID 是 **`sensenova-6.7-flash-lite`**（全小写、连字符，控制台 cURL 里那个）。如果 `openclaw chat` 报"model not found"，就把 Model ID 改成 `sensenova-6.7-flash-lite`。
3. 重启 Gateway 让配置生效：
```powershell
openclaw gateway restart
```

### 3.3 选模型（默认就能用，进阶再换）

- 上面接好后，OpenClaw 默认用你设的日日新模型，不用再改就能聊天。
- 想换日日新其它版本/模型，格式是 `custom-token-sensenvova-cn/<模型名>`。
- 想临时用别的厂商，在 `openclaw configure` 的交互选择器里改 Model/auth provider 即可。

### 3.4 验收：真的能对话了

先测"脑子"通没通：
```powershell
openclaw gateway restart
openclaw chat "你好，用一句话介绍你自己"
```
如果它回你一段话，说明日日新模型接上了。

---

## 四、⚠️ 避坑总结

| 坑 | 现象 | 解法 |
|---|---|---|
| Node 版本太低 | 安装报语法错 | 升到 24，重开终端 |
| 漏装 Git | ClawHub/源码/npm编译报错 | `winget install Git.Git` |
| python 没加 PATH | 脚本找不到 python | 重装勾选 Add to PATH |
| 商汤 Key 没存 | 对话失败/401 | 回 platform.sensenova.cn 控制台重新建 |
| PowerShell 跑脚本被拦 | 报"禁止运行脚本" | `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` |

---

## 五、下一步你可以做什么

- **换日日新其它模型**：在 `openclaw configure` 选择器里改具体型号，或 `openclaw models set custom-token-sensenvova-cn/<型号>`。
- **常开方案**：旧笔记本/树莓派/几十块月的轻量云服务器都行，让 AI 7×24 在线。
- **ClawHub 技能市场**：用 Git 拉社区技能扩展助手能力。
- **接微信/飞书等通道**：这是另一块内容，需要的话我单独出一期。