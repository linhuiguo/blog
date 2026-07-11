---
title: "Claude 的 MCP 服务器爆了！DesktopCommanderMCP 让 Claude 直接操作你的电脑（保姆级）"
date: 2026-07-12T00:00:00+08:00
draft: false
tags: ["MCP", "Claude", "DesktopCommanderMCP", "AI 编程", "保姆级教程"]
categories: ["AI工具"]
summary: "DesktopCommanderMCP 让 Claude 直接操作你的电脑，终端命令、文件搜索、代码编辑，今日 GitHub 热榜 +900 星。"
description: "DesktopCommanderMCP 保姆级教程，让 Claude 通过 MCP 协议直接操作你的电脑，终端命令、文件搜索、代码 diff 编辑，小白也能装。"
keywords: ["DesktopCommanderMCP", "MCP", "Claude", "AI 操作电脑", "终端控制", "0149"]
---

# Claude 的 MCP 服务器爆了！DesktopCommanderMCP 让 Claude 直接操作你的电脑（保姆级）

> GitHub 热榜今日 +900 星，一个 MCP 服务器让 Claude 直接操作你的电脑：终端命令、文件搜索、代码 diff 编辑。今天手把手教你装。

## ⚠️ 先说清

1. **你需要 Claude Code**：这个 MCP 服务器是给 Claude Code 用的，如果你没用 Claude Code，装不上。
2. **需要安装 Node.js**：MCP 服务器是 Node.js 写的，先装 Node。
3. **权限警告**：让 AI 操作你的电脑，有安全风险。建议先装到测试环境用。

---

## 〇、准备清单

- 一台 Windows 电脑
- Node.js 18+（[官网下载](https://nodejs.org/)）
- Claude Code（已安装）

---

## 一、第一步：检查 Node.js

打开命令提示符（开始菜单搜 `cmd` 回车）：

```bash
node -v
```

如果显示 `v18.x.x` 或更高，继续。如果显示 `不是内部或外部命令`，去 [nodejs.org](https://nodejs.org/) 下载 LTS 版双击安装。

---

## 二、第二步：安装 DesktopCommanderMCP

```bash
npm install -g @wonderwhy-er/desktop-commander-mcp
```

> ⚠️ 如果报错 `npm ERR`，试试 `npm install --legacy-peer-deps -g @wonderwhy-er/desktop-commander-mcp`

---

## 三、第三步：配置 Claude Code

打开 Claude Code 配置文件 `~/.claude/config.json`（或 `C:\Users\你的用户名\.claude\config.json`），添加：

```json
{
  "mcpServers": {
    "desktop-commander": {
      "command": "npx",
      "args": ["-y", "@wonderwhy-er/desktop-commander-mcp"]
    }
  }
}
```

保存后重启 Claude Code。

---

## 四、第四步：验证

在 Claude Code 里问它：

> "你现在能操作我的电脑吗？"

如果它说能，说明装好了。

---

## 五、它能干嘛

| 能力 | 说明 |
|------|------|
| **终端命令** | Claude 直接在命令行执行命令 |
| **文件搜索** | 搜索文件内容、文件名 |
| **代码 diff 编辑** | 修改代码时自动生成 diff |
| **远程协作** | 支持远程连接其他机器 |

---

## ⚠️ 避坑总结

| 坑 | 现象 | 解法 |
|---|---|---|
| npm 安装失败 | `npm ERR` | 加 `--legacy-peer-deps` |
| Claude Code 没反应 | 配置后没变化 | 重启 Claude Code |
| 权限不足 | 命令执行失败 | 以管理员身份运行终端 |
| 不知道装没装好 | 不确定 | 问 Claude "你现在能操作我的电脑吗？" |

---

**我是先生靠谱，专讲小白能看懂的实操。**