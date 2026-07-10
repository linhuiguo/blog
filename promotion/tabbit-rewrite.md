你是不是嫌 Claude、GPT 的 API 太贵？

Tabbit 是一款国产 AI 浏览器，内置了 **21 个模型**——Claude-Opus-4.8、GPT-5.5、Gemini-3.5 这些顶级模型全在里面，而且**本来就不要钱**。

正常你得打开 Tabbit 浏览器才能用。这篇教你在**本地起一个服务**，把这些模型"翻译"成标准 OpenAI 接口，然后接到 Cherry Studio、Codex、NextChat 里——**以后调顶级模型不用花一分钱 API 钱**。

全程跟着做，小白也能通。项目叫 `tabbit-toy`（GitHub 搜 `goehou/tabbit-toy`），下面是实测可走的步骤。

> ⚠️ 声明：仅供个人学习研究，别商用、别高并发滥用。Cookie 等同账号控制权，千万别提交到公开仓库。

---

## 一、先搞懂它在干什么

```
你的客户端 ──OpenAI格式──▶ 本地服务(Tabbit2API) ──翻译+签名──▶ web.tabbit.ai
   ▲                                            │
   └──────── OpenAI 格式回复 ◀── SSE 流 ◀────────┘
```

简单说：本地起个服务，把 Tabbit 浏览器里的模型变成标准接口，任意支持 OpenAI 格式的客户端都能直接调用。

---

## 二、前置条件（缺一不可）

• **装了 Tabbit 浏览器并登录**：去 tabbit.ai 下载安装、登录账号。没登录导不出 Cookie，后面全卡住。
• **Pro 会员（免费解锁）**：Tabbit **设置里"设为默认浏览器"**即可解锁，不花钱。免费模型能用；Claude/GPT/Gemini 等 premium 模型必须这个，否则报 `premium users only`。
• **Node.js 18+**：命令行 `node -v` 确认 ≥ 18。低于 18 启动报错；项目零依赖，不用 npm install。

> ✅ 检查：`node -v` 输出版本 ≥ 18 就 OK。没装去 nodejs.org 下 LTS 版。

---

## 三、第 1 步：下载项目

从 GitHub（搜 `goehou/tabbit-toy`）下载 ZIP，解压到任意目录，比如 `D:\toy\tabbit2api`。解压后应有 `src/`、`scripts/`、`cookie-helper-extension/`、`.env.example`、`README.md` 等。

---

## 四、第 2 步：导出 Cookie（关键，最容易错）

Tabbit 登录态存在浏览器 Cookie 里（含 HttpOnly 的 JWT），需导出一次。

**方法 A：用项目自带扩展（推荐）**
1. 打开 Tabbit 浏览器（或普通 Chrome），地址栏输 `chrome://extensions/` 回车
2. 右上角打开**「开发者模式」**
3. 点**「加载已解压的扩展程序」**，选项目里的 `cookie-helper-extension` 文件夹
4. 地址栏输 `https://web.tabbit.ai/` 并**登录**账号
5. 点工具栏扩展图标 → **「复制 Cookie」**
6. 整串 Cookie 先贴记事本存好，第 4 步用

**方法 B：手动复制（兜底）**
1. Tabbit 浏览器打开 `https://web.tabbit.ai/` 登录
2. `F12` → `Application` → `Cookies` → `https://web.tabbit.ai`
3. 每条 `name=value` 用分号 `;` 拼成一行

> ⚠️ **Cookie 有效期约 7 天**，过期服务连不上，用扩展重导一次、更新 `.env` 重启即可。

---

## 五、第 3 步：获取真实版本号（必须真实，不能瞎填）

Tabbit 浏览器打开 `https://web.tabbit.ai/`，`F12` 切 `Console`，执行：

```js
chrome.tabInstance.getDeviceInfo().then(d => console.log(d.tabbitVersion))
```

> 注意函数名是 `chrome.tabInstance`（不是 `tabbitInstance`）。

输出类似 `1.1.39(10101039)`，**记下来**第 4 步用。

> ⚠️ 版本号必须真实格式 `1.1.39(10101039)`（带括号数字）。瞎填报 `493 "update_version"`。

---

## 六、第 4 步：配置 .env

1. 复制模板：命令行 `copy .env.example .env`（或直接重命名）
2. 记事本打开 `.env`，填两行必填：

```env
TABBIT_COOKIE=粘贴第2步复制的整串 Cookie
TABBIT_VERSION=1.1.39(10101039)   # 填第3步拿到的值
```

> ⚠️ `TABBIT_COOKIE` 是一整行，中间别换行、别加空格。`.env` 已在 `.gitignore`，**千万别提交 git 或发别人**。

---

## 七、第 5 步：启动服务

在 `tabbit2api` 目录打开命令行，执行：

```bash
node src/server.mjs
```

看到下面说明成功：

```
Tabbit2API · OpenAI 兼容代理
  端口: 8787
  版本: 1.1.39(10101039)
```

> ⚠️ 端口 8787 被占用 → 改 `.env` 里 `PORT=8799` 重启，调用地址同步改。保持窗口开着，关了服务停。

---

## 八、第 6 步：验证通不通

新开命令行，先测健康检查：

```bash
curl http://localhost:8787/healthz
```

再测聊天（PowerShell 执行，避开 cmd 引号地狱）：

```bash
curl http://localhost:8787/v1/chat/completions -H "Content-Type: application/json" -d "{\"model\":\"Default\",\"messages\":[{\"role\":\"user\",\"content\":\"你好\"}]}"
```

> ⚠️ 报 `premium users only` → 用了 Pro 模型但没设默认浏览器，回 Tabbit 设置设一下重启。
> ⚠️ 报 `AI service temporarily unavailable` → 会话失效，先在 Tabbit 浏览器发条消息建对话再试。

---

## 九、接到客户端（Cherry Studio / NextChat / Codex）

这样填：
• **API 地址 (baseURL)**：`http://localhost:8787/v1`
• **API Key**：随便填（如 `sk-anything`），不校验
• **模型名**：`Default` / `Claude-Opus-4.8` / `GPT-5.5` 等

---

## 十、可用模型一览

**免费无限：**
• `Default`

**免费计量：**
• `GLM-5.2`、`DeepSeek-V4-Pro`、`Kimi-K2.6`、`MiniMax-M3`、`Claude-Haiku-4.5`、`GPT-5.2-Chat`、`Qwen3.5-Plus`、`Doubao-Seed-1.8`

**⭐ Pro 会员（设置里"设为默认浏览器"解锁）：**
• `Claude-Opus-4.8`、`GPT-5.5`、`Gemini-3.5-Flash` 等

---

## 十一、排错速查

• **`493 "update_version"`**：版本号格式不对 → 用第 3 步 `getDeviceInfo()` 拿真实值
• **`premium users only`**：用 Pro 模型没设默认浏览器 → Tabbit 设置"设为默认浏览器"重启
• **`AI service temporarily unavailable`**：会话失效 → Tabbit 浏览器先发条消息建对话
• **Cookie 过期**：JWT 约 7 天过期 → 扩展重导，更新 `.env` 重启
• **端口被占用**：8787 被占 → `.env` 改 `PORT=8799`

---

卡住的直接评论区甩报错截图，我看到回你。更多 AI 白嫖教程见博客 **0149.cc.cd**。
