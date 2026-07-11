# 三款 AI 编程助手保姆级安装教程：OpenClaw / Codex / Hermes（零基础跟着做）

> 作者：先生靠谱  
> 适用：Windows 10 / 11  
> 读完时间：约 10 分钟  
> 难度：★★☆☆☆（只要不跳步骤，小白也能搞定）

---

## 先搞清楚：这三款工具分别干什么？

| 工具 | 一句话定义 | 适合谁 |
|------|-----------|--------|
| **OpenClaw** | 国产 AI 浏览器，内置多种大模型（Claude、GPT、Gemini、DeepSeek），能直接在网页上对话、划词提问 | 不想折腾代码、敲敲打打就行 |
| **Codex** | OpenAI 出的命令行编程助手，终端里帮你写代码、改 bug、跑脚本 | 会点基础命令行、想提升写代码效率 |
| **Hermes Agent** | 万能 AI 助手，能控制电脑、发头条/知乎、跑脚本、定时任务 | 想用 AI 真干活、不是光聊天 |

**三款装在一起不冲突**，各自发挥所长。下面从最基础的准备工作开始，一步步来。

---

## ⚠️ 第 0 步：必装前置条件（90% 的人卡在这里）

### 必须装的东西

| 软件 | 用途 | 装哪里 |
|------|------|--------|
| **Node.js 18+** | OpenClaw 和 Codex 都依赖它 | 官网下载 |
| **Python 3.10+** | Hermes 后端依赖 | 官网下载 |
| **Git** | 克隆项目代码 | 官网下载 |
| **OpenAI API Key** | Codex 和 OpenClaw 调用模型 | platform.openai.com |
| **(可选) Edge/Chrome 浏览器** | Hermes 控制浏览器要用 | 已有就不装 |

### 问题排查：Node.js 装完 `node -v` 提示"不是内部命令"

**原因**：安装时没勾上 **"Add to PATH"** 选项。

**解决方法**（二选一）：

**方法 A - 重装修复**：
1. 去 nodejs.org 重新下安装包
2. 安装时务必勾选 **"Add to PATH"**
3. 一路下一步，装完**重启终端**
4. 再跑 `node -v`，看到 `v18.x.x` 或更高就 OK

**方法 B - 手动加到环境变量**：
1. 找到 Node 安装位置，一般是：
   ```
   C:\Program Files\nodejs
   ```
2. Win + R 输入 `sysdm.cpl` → 高级 → 环境变量 → 系统变量 → 找到 `Path` → 编辑 → 新建 → 粘贴上面的路径
3. **重启终端**，再跑 `node -v`

### 问题排查：`npm install` 报错 `EACCES` 或权限不足

**原因**：npm 全局目录权限不对。

**解决方法**：
```bash
# 在管理员终端里执行，创建全局目录
mkdir C:\npm-global
npm config set prefix "C:\npm-global"
# 加到系统环境变量 Path 里（手动加上面的 C:\npm-global）
# 重启终端后再试
```

---

## 第 1 步：安装 OpenClaw（最简单，下载解压就能用）

### 1.1 下载

去 OpenClaw 官网（或项目 GitHub release 页面）下载 Windows 版，是一个 `.exe` 安装包。

### 1.2 安装

双击安装包，一路下一步就好。装完后桌面会出现 **OpenClaw** 图标。

### 1.3 首次登录

1. 双击打开 OpenClaw
2. 用手机号或邮箱注册登录
3. 登录后里面内置了多个 AI 模型，直接就能用

### 1.4 常见问题

| 问题 | 解决 |
|------|------|
| 下载慢/失败 | 用梯子或女朋友的热点 |
| 打开闪退 | 右键 → 以管理员身份运行 |
| 内置模型用不了 | 可能是会员额度用完，看界面右上角提示 |

---

## 第 2 步：安装 Codex（OpenAI 官方编程 CLI）

这一步最容易出错，注意每一步。

### 2.1 前置条件确认

先确认 Node.js 装好了：
```bash
node -v
npm -v
```
如果都输出版本号（比如 `v18.20.0`），继续下一步。

### 2.2 全局安装 Codex

```bash
npm install -g @openai/codex
```

**如果报错 `ENOENT` 或 `not found`**：
- 说明 npm 找不到这个包，可能是网络问题
- 解决方法：设个国内镜像源再装
  ```bash
  npm config set registry https://registry.npmmirror.com
  npm install -g @openai/codex
  ```

### 2.3 坑：Windows 系统更新导致安装失败（最常见）

**现象**：执行安装命令后提示：
```
npm ERR! Error: EBUSY: resource busy or locked, ...
npm ERR!     at ... (function that writes to Program Files)
```
或者提示 **"系统无法更新，文件被占用"**。

**原因**：Windows 前几天装过更新，有个后台进程正在锁文件；或者杀毒软件在扫文件。

**解决方法（按顺序试）**：

**第一步：重启电脑**
重启后再试一次。80% 的情况重启就能解决。

**第二步：关闭杀毒软件/安全卫士**
360、火绒、Windows Defender 等，临时关闭实时防护，再装。

**第三步：换非系统盘安装**
```bash
npm config set prefix "D:\npm-global"
```
把前缀改到 D 盘，再重新 `npm install -g @openai/codex`。

**第四步：管理员终端**
Win 键搜索 "终端" → 右键"以管理员身份运行" → 再装一次。

### 2.4 配置 API Key

安装完后，终端执行：
```bash
codex
```

**首次运行会提示你输入 API Key**。

**怎么获取 API Key？**
1. 打开 https://platform.openai.com/api-keys
2. 登录账号
3. 点 "Create new secret key"
4. 复制那串 `sk-...` 开头的密钥
5. 粘贴进终端提示的地方

### 2.5 验证安装

装完跑一下：
```bash
codex --version
```
输出版本号就 OK。直接跑 `codex` 就能进入对话式编程助手界面，输入框里直接打字提问。

---

## 第 3 步：安装 Hermes Agent（我们的主角）

### 3.1 前置条件确认

```bash
# 确认 Python 版本
python --version
# 要求 3.10 或更高，看到 Python 3.10.x 或 3.11.x / 3.12.x 都可以
```

如果提示 `python 不是内部命令`，去 python.org 下载 3.11/3.12，安装时**勾选 "Add Python to PATH"**。

### 3.2 安装 Hermes

```bash
pip install hermes-agent
```

装完后，终端执行：
```bash
hermes
```

看到欢迎界面就成功了。首次启动需要配置一个模型提供商（比如 OpenAI、Anthropic、或本地模型）。

---

## 第 4 步：把三者串起来用（关键，很多教程漏了这步）

光装完各用各的低效，下面是实战用法。

### 4.1 OpenClaw + Codex 联动

**场景**：你在 OpenClaw 里看到一段网上代码，想让它帮你优化然后本地跑。

**方法**：
1. OpenClaw 里选中代码 → 复制
2. 终端打开 Codex：`codex`
3. 对 Codex 说："这是我刚从 OpenClaw 复制的一段代码（粘贴代码），帮我看有没有性能问题，并优化。已装 Python 3.12"

Codex 就会在终端里帮你分析、修改、甚至帮你跑。

### 4.2 Hermes 直接调用 Codex

**场景**：用 Hermes 定时跑脚本，Codex 帮忙写脚本。

** Hermes → Codex 方式**：
Hermes 的配置里加一个自定义 provider，指向 Codex 背后用的模型端点（也就是 OpenAI API），这样 Hermes 可以调度 Codex 的能力。

这是进阶玩法，后面文章再展开。

### 4.3 OpenClaw + Hermes 联动

**场景**：Hermes 帮你自动发头条，OpenClaw 帮你写文章。

**方法**：
1. Hermes 后台跑定时任务，抓取热点话题
2. 把热点发给 OpenClaw，让它生成文章草稿
3. Hermes 自动把草稿填进头条编辑器
4. 你检查插图，点发布

三个步骤全自动，你只做**插图 + 确认发布**。

---

## 第 5 步：如何接入 Hermes Agent / Codex 使用

这里教你把这两款工具真正用起来，不只是"装完就完了"。

### 5.1 Codex 接入 Hermes

在 Hermes 配置文件 `~/.hermes/config.yaml` 里加：

```yaml
custom_providers:
  codex:
    provider: openai
    api_base: https://api.openai.com/v1
    api_key: sk-你的key
    model: o4-mini
```

重启 Hermes 后，在对话里直接说：
```
用 codex 模型帮我写一个脚本来批量重命名文件
```

Hermes 就会调用 Codex 的能力来完成。

### 5.2 Hermes 接入 OpenClaw

OpenClaw 本身是桌面应用，Hermes 通过 **CDP（Chrome DevTools Protocol）** 来控制它：

1. 启动 OpenClaw 时加调试参数：
   ```bash
   OpenClaw.exe --remote-debugging-port=9222
   ```
2. Hermes 配置文件里加上 CDP 地址：
   ```yaml
   mcp_servers:
     openclaw:
       command: npx
       args: ["-y", "@openai/codex"]
       env:
         CUA_DRIVER_CDP_PORT: "9222"
   ```
3. 重启 Hermes，就能用 `computer_use` 工具控制 OpenClaw 里的网页操作

---

## 第 6 步：你必须知道的 5 个坑

### 坑 1：Codex 在中国大陆网络不通

**现象**：`codex` 命令一直转圈连不上。

**解决**：
- 检查你的网络能不能访问 api.openai.com
- 需要科学上网，Clash / V2Ray 节点配好全局模式

### 坑 2：Hermes 和 OpenClaw 抢同一个端口

**现象**：启动报错 "Port 9222 already in use"。

**解决**：把其中一个的端口改了。比如 Hermes 用 9222，OpenClaw 用 9223。

### 坑 3：OpenClaw 内置模型额度用完

**现象**：对话返回 "额度不足" 或 "请升级会员"。

**解决**：
- 换用免费模型（DeepSeek、GLM 等）
- 或者自己的 OpenAI Key 充钱

### 坑 4：Codex 写的中文代码有乱码

**现象**：生成的代码注释是乱码。

**解决**：在终端设置里改编码：
```bash
chcp 65001
```
然后在 Codex 里说："用 UTF-8 编码，注释写中文"。

### 坑 5：Windows 杀毒软件误删 Hermes

**现象**：安装后打开 Hermes 被杀毒软件拦截。

**解决**：
- 把 Hermes 安装目录加到杀毒软件白名单
- 或者在 Windows Defender 里添加排除项：
  Win + R → `windowsdefender://` → 病毒防护 → 排除项 → 添加文件夹

---

## 总结：三款工具的分工

| 你想做什么 | 用哪个 |
|-----------|--------|
| 快速在网页上和 AI 对话、划词提问 | OpenClaw |
| 终端里写代码、改 bug、跑脚本 | Codex |
| 自动发文章、定时任务、控制电脑 | Hermes Agent |
| 三款配合 | Hermes 调度 + OpenClaw 写稿 + Codex 写脚本 |

---

## 关于作者

我叫**先生靠谱**，专注写 AI 工具实操教程，目标是让小白也能跟着做。有问题留言，我看到就回。

**博客**：https://www.0149.cc.cd  
**番茄小说**：《苏鹤年》（全版权 20 年）

---

*本文只介绍工具安装与使用方式，不涉及任何违法用途。请遵守当地法律法规和平台规则。*
