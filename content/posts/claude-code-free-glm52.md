# 免费白嫖！用 Cloudflare AI Gateway 跑 GLM-5.2，Claude Code 零成本接入

**标题：免费白嫖！用 Cloudflare AI Gateway 跑 GLM-5.2，Claude Code 零成本接入**

想用 Claude Code 但又不想花钱？Cloudflare AI Gateway 上有免费的 GLM-5.2 和大模型，通过 LiteLLM 代理桥接到 Claude Code，完全免费。

---

## 准备工作

先确认你已安装：

- **Node.js / npm**（搜"Node.js 安装"按默认路径装即可）
- **uv**（Python 包管理器，比 pip 快）

---

## 步骤 0：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

---

## 步骤 1：单测 GLM-5.2（确认 Cloudflare 模型能调用）

```bash
curl https://api.cloudflare.com/client/v4/accounts/你的ACCOUNT_ID/ai/gateway/ai-chat \
 -H "Authorization: Bearer 你的API_TOKEN" \
 -H "Content-Type: application/json" \
 -d '{"messages":[{"role":"user","content":"用一句话介绍你自己"}]}'
```

> 把 `你的ACCOUNT_ID` 和 `你的API_TOKEN` 替换成你的 Cloudflare 信息。能返回模型回复就说明通了。

---

## 步骤 2：安装 uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

uv 是 Python 包管理工具，避免 Python 3.14 编译失败的问题。

---

## 步骤 3：验证 LiteLLM 能跑

```bash
uvx --python 3.12 --from 'litellm[proxy]' litellm --version
```

能输出版本号就说明 LiteLLM 装好了。

---

## 步骤 4：配置 cf-config.yaml

新建文件 `cf-config.yaml`，内容如下（**缩进一定要严格，ACCOUNT_ID 有两处都要替换**）：

```yaml
model_list:
  - model_name: cf-glm-5.2
    litellm_params:
      model: openai/@cf/zai-org/glm-5.2
      api_base: https://api.cloudflare.com/client/v4/accounts/你的ACCOUNT_ID/ai/gateway/ai-chat
      api_key: os.environ/CLOUDFLARE_API_TOKEN

  - model_name: cf-small
    litellm_params:
      model: openai/@cf/meta/llama-3.1-8b-instruct-fp8
      api_base: https://api.cloudflare.com/client/v4/accounts/你的ACCOUNT_ID/ai/gateway/ai-chat
      api_key: os.environ/CLOUDFLARE_API_TOKEN

litellm_settings:
  use_chat_completions_url_for_anthropic_messages: true
```

---

## 步骤 5：启动翻译桥

在同一文件夹下执行（**这个窗口要一直开着，不能关**）：

```bash
export CLOUDFLARE_API_TOKEN=你的token
uvx --python 3.12 --from 'litellm[proxy]' litellm --config cf-config.yaml --port 4000
```

窗口启动后会在 `localhost:4000` 上运行代理。

---

## 步骤 6：测试桥是否通

新开一个终端窗口，执行：

```bash
curl http://localhost:4000/v1/models -H 'Authorization: Bearer sk-1234'
```

能返回模型列表就说明桥通了。

---

## 步骤 7：Claude Code 接免费引擎

设置四个环境变量：

```bash
export ANTHROPIC_BASE_URL=http://localhost:4000
export ANTHROPIC_AUTH_TOKEN=sk-1234
export ANTHROPIC_MODEL=cf-glm-5.2
export ANTHROPIC_DEFAULT_HAIKU_MODEL=cf-small
```

然后启动 Claude Code：

```bash
claude
```

首次启动会弹出自定义 API 设置提示，选「是」。

用 `/status` 命令确认 Base URL 指向 `localhost:4000`，说明接入成功。

---

## 步骤 8：一劳永逸

把四个环境变量写进配置文件，以后开机自动生效：

**Linux / macOS（bash/zsh）**：

把上面四行 `export` 追加到 `~/.zshrc`（或 `~/.bashrc`）末尾：

```bash
echo 'export ANTHROPIC_BASE_URL=http://localhost:4000' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN=sk-1234' >> ~/.zshrc
echo 'export ANTHROPIC_MODEL=cf-glm-5.2' >> ~/.zshrc
echo 'export ANTHROPIC_DEFAULT_HAIKU_MODEL=cf-small' >> ~/.zshrc
source ~/.zshrc
```

**Windows（PowerShell）**：

把所有 `export` 改成 `$env:变量名="值"` 格式，写入 PowerShell 配置文件 `$PROFILE`：

```powershell
$env:ANTHROPIC_BASE_URL="http://localhost:4000"
$env:ANTHROPIC_AUTH_TOKEN="sk-1234"
$env:ANTHROPIC_MODEL="cf-glm-5.2"
$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="cf-small"
```

写入配置：

```powershell
Add-Content -Path $PROFILE -Value '$env:ANTHROPIC_BASE_URL="http://localhost:4000"'
Add-Content -Path $PROFILE -Value '$env:ANTHROPIC_AUTH_TOKEN="sk-1234"'
Add-Content -Path $PROFILE -Value '$env:ANTHROPIC_MODEL="cf-glm-5.2"'
Add-Content -Path $PROFILE -Value '$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="cf-small"'
```

---

## 总结

通过 Cloudflare AI Gateway + LiteLLM 代理桥，Claude Code 可以免费接入 GLM-5.2 和 Llama 等免费模型，完全不用花钱。

**核心流程**：
1. 装 Claude Code
2. 装 uv
3. 配置 cf-config.yaml
4. 启动 LiteLLM 代理桥
5. 设环境变量接入 Claude Code

---

**我是千华落影，一个技术博主，分享 AI 大模型、本地部署、编程开发等实用教程。**
