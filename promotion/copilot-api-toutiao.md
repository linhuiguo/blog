# 标题（三选一）

1. 微软良心！不用花一分钱，把 GPT-5.5 接到你电脑上（保姆级教程）
2. 别再充 ChatGPT 会员了！教你白嫖微软 Copilot 的 GPT-5.5，永久免费
3. 0 成本搞定 GPT-5.5！普通人 10 分钟就能学会的神仙操作

---

# 正文

大家好，我是**先生靠谱**。

今天分享一个**真正能白嫖**的玩法：把微软 Copilot 里内置的 GPT-5.4 / GPT-5.5 模型，反代成标准的 OpenAI 接口，**免费、不需要 API Key、不扣任何费**，然后接到你常用的 AI 软件里随便用。

我自己实测跑通了，下面手把手教，照着做就行。

## 一、这玩意到底是个啥？

有个开源项目叫 **Windows-Copilot-API**（GitHub 上搜这个名字就能找到）。

它的原理很简单：

- 微软 Copilot 网页端登录后，背后其实跑的是 GPT 模型（目前大概是 GPT-5.4 或 5.5）
- 这个项目把 Copilot 的网页接口"逆向"了一下
- 封装成大家熟悉的 **OpenAI 标准接口**
- 你就能在 Cherry Studio、Codex、Hermes Agent 等任何支持 OpenAI 格式的软件里用它

**重点：免费账户就行，不需要任何 VIP，也不走 API 计费。**

支持 Windows、macOS、Linux，全平台通用。

## 二、准备工作（先确认这两样）

1. 一个 **Microsoft 账户**或 **Copilot 账户**（去 account.microsoft.com 免费注册，别开会员）
2. 装好 **Python 3.8 以上**
   - 打开 python.org 下载 Windows 安装包，双击运行
   - ⚠️ 安装时一定勾选 **"Add Python to PATH"**（加到环境变量），不然后面命令找不到

## 三、部署步骤（一步一步来）

### 第 1 步：下载项目

1. 浏览器打开 GitHub，搜 `Windows-Copilot-API`
2. 点绿色的 **Code** 按钮 → **Download ZIP**
3. 解压到任意文件夹，比如 `D:\Windows-Copilot-API-master`

### 第 2 步：打开命令行，进到项目目录

1. 打开解压后的文件夹
2. 在文件夹地址栏里输入 `cmd` 回车（或者按住 Shift 右键→"在此处打开 PowerShell 窗口"）
3. 输入下面命令确认进对了目录（能看到 requirements.txt 就对了）：
   ```bash
   dir
   ```

### 第 3 步：安装依赖

在命令行里粘贴回车：

```bash
pip install -r requirements.txt
```

等它跑完，看到一大堆 `Successfully installed` 就 OK。

### 第 4 步：下载内置浏览器（最容易卡的一步）

项目要下载一个内置浏览器（约 182MB）来模拟真实登录环境，这是必须的。

⚠️ **建议先挂上代理再下载**，不然可能慢到几十 KB/s。

命令行里执行：

```bash
python -m playwright install chromium
```

> 如果提示 playwright 没装，先执行 `pip install playwright` 再跑上面这条。

### 第 5 步：登录你的账号

```bash
python -m copilot login
```

会自动弹出一个浏览器窗口，用你的微软/Copilot 账号登录。登录成功后窗口会自动关闭，Token 已经存好了。

### 第 6 步：启动服务

```bash
python app.py
```

窗口会一直开着，别关。看到类似 `Running on http://127.0.0.1:8000` 就成功了。

记住这 3 个信息，后面配置要用：

- **地址**：`http://127.0.0.1:8000`
- **接口路径**：`/v1`
- **模型名**：`copilot`
- **密钥**：随便填（因为根本不验证）

> 想给局域网/别的电脑用，可以加 `--host 0.0.0.0` 参数启动，但本地用默认就行。

## 四、配置到 Cherry Studio（最直观的验证）

1. 打开 Cherry Studio，点右上角 **设置（齿轮图标）**
2. 找到 **模型服务** → **OpenAI** → 添加
3. 填写：
   - API 地址：`http://127.0.0.1:8000/v1`
   - API 密钥：随便填，比如 `sk-free`
4. 点 **"获取模型列表"**，会出来一个 `copilot` 模型
5. 点加号添加，回到首页就能聊天了

实测问"你好"、问"今天全球新闻"都能正常答，**而且它内置搜索**，不用额外接搜索插件，直接当搜索 AI 用。

## 五、配置到 Codex（OpenAI 官方 CLI）

Codex CLI 支持用环境变量指定自定义端点：

```bash
# 在启动 Codex 前，设置这两个环境变量
set OPENAI_BASE_URL=http://127.0.0.1:8000/v1
set OPENAI_API_KEY=sk-free

# 然后正常启动 Codex
codex
```

或者在 Codex 的配置文件里写死也行。这样 Codex 就会走你的白嫖接口，而不是官方收费 API。

## 六、配置到 Hermes Agent（咱自己用的这个）

Hermes Agent 支持自定义 OpenAI 兼容的 provider。改一下配置文件就行：

1. 打开 Hermes 的配置文件（一般在 `~/.hermes/config.yaml`）
2. 在 `custom_providers` 下加一段：

```yaml
custom_providers:
  copilot_free:
    base_url: "http://127.0.0.1:8000/v1"
    api_key: "sk-free"
    models:
      - id: "copilot"
        name: "Copilot (白嫖GPT-5.5)"
```

3. 保存后重启 Hermes
4. 在模型选择里就能看到 **Copilot (白嫖GPT-5.5)**，选它即可

> 注意：每次用之前，要保证第三步那个 `python app.py` 的服务窗口是开着的，关了接口就断了。

## 七、还能接哪里？

因为完全兼容 OpenAI 标准接口，理论上所有支持 OpenAI 格式的客户端都能接：

- ✅ Cherry Studio（图形界面，最省事）
- ✅ Codex / Claude Code（CLI 工具）
- ✅ Hermes Agent（咱们的自动化主力）
- ✅ 你自己写的 Python 脚本（直接 import openai，base_url 改一下就能调）

## 最后

这个项目的详细部署笔记和常见坑，我整理在了博客里，想看更细的可以戳：
👉 https://www.0149.cc.cd/posts/copilot-free-api/

觉得有用的话，**点赞 + 收藏 + 关注 @先生靠谱**，后续继续分享这种不花钱的硬核玩法。有问题评论区见 👇
