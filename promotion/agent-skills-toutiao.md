# 标题（三选一，发布时挑一个）
1. Google 大佬开源的 AI 技能库，一键白嫖！让 Codex / Claude 变身资深工程师
2. 免费！24 个生产级 AI Agent 技能，一条命令装进 Claude / Codex / Cursor
3. 写代码总返工？Google 工程师这套 Agent 技能包，小白也能一键用上

---

> 作者：先生靠谱
> 事实来源：GitHub 仓库 addyosmani/agent-skills（Addy Osmani，Google 工程师），README 公开内容。教程向，小白也能看懂。

---

# Google 大佬开源的 AI 技能库，一键白嫖！让 Codex / Claude 变身资深工程师

你是不是这样：让 AI 帮你写代码，它吭哧吭哧写一堆，结果跑不通、逻辑乱、还得你返工？

问题不在模型，在于**没人教 AI"怎么像个老工程师一样干活"**。

今天给你一个**真·黑马项目**：Google 工程师 Addy Osmani 开源的 `agent-skills`——一套"生产级工程技能包"，把资深工程师写软件的工作流、质量门禁、最佳实践，**打包成 24 个技能**，让你的 AI 编码助手（Claude / Codex / Cursor / Copilot 等）照着做。

最关键：**完全免费，一条命令装好**。

---

## 一、它到底是个啥

简单说：这是一套"AI 写代码的操作手册"。

普通 AI 写代码是"想到哪写到哪"。这套技能把软件开发拆成标准流程：

```
定需求 → 写规格 → 做计划 → 写代码 → 测 → 审查 → 发布
/spec   /plan    /build   /test  /review  /ship
```

每一步都有明确规矩和质量检查，AI 不能偷懒跳过。比如：
- **先写规格再写码**（spec before code）
- **小步快跑**（一次只干一块）
- **测试即证据**（tests are proof）
- **发布前先审查**（improve code health）

就像给 AI 配了个 strict 的 Tech Lead。

---

## 二、24 个技能，覆盖全流程

仓库一共 **24 个技能**（23 个工作流 + 1 个元技能）。几个你用得上的：

| 技能 | 干啥用 |
|------|--------|
| `/spec` | 先把要做啥写清楚（别上来就写码） |
| `/plan` | 把任务拆成小原子步骤 |
| `/build` | 增量构建，一次一片 |
| `/test` | 测试驱动，证明它真能跑 |
| `/review` | 合并前代码审查（五轴审查） |
| `/webperf` | 网页性能审计（先量再优化） |
| `/code-simplify` | 代码瘦身（清晰胜过炫技） |
| `/ship` | 发布上线（快就是稳） |
| `interview-me` | 反向"拷问"你需求，问到 95% 确定才动手 |
| `test-driven-development` | 红-绿-重构，强制践行 |

而且技能会**按场景自动触发**：你设计 API，它自动调 api-and-interface-design；你写界面，它自动调 frontend-ui-engineering。不用你记命令。

---

## 三、怎么装（核心：一条命令）

最省事的方式——**任意 AI Agent，一条命令装全部 24 个**：

```bash
npx skills add addyosmani/agent-skills
```

它支持 **70+ 种 Agent**：Claude Code、Cursor、Codex、Copilot、Cline、Windsurf、OpenCode、Gemini CLI 等等。

只想装某几个也行：

```bash
# 先看有哪些
npx skills add addyosmani/agent-skills --list

# 装单个
npx skills add addyosmani/agent-skills --skill code-review-and-quality
npx skills add addyosmani/agent-skills --skill test-driven-development
```

> ⚠️ 需要本机装了 Node.js（有 `npx` 命令）。没装去 nodejs.org 下 LTS 版。

---

## 四、装完怎么用

装好后，在你常用的 AI 编码工具里，直接打斜杠命令就行：

- 让 AI 先理清需求：输入 `/spec 我要做个待办清单网页`
- 让它按计划写：输入 `/build`
- 写完测一下：输入 `/test`
- 发布前审查：输入 `/review`

如果你用的是 Claude Code / Codex 这类，**连命令都不用记**——它看你干啥自动调对应技能。

---

## 五、为啥说它"黑马"

这个项目今天一天涨了 **2500+ star**（GitHub Trending 第一梯队），火不是没道理：

1. **出自 Google 资深工程师**，质量是"生产级"，不是玩具
2. **免费开源**，一键装，零门槛
3. **通用**——不绑定某个模型，Claude / Codex / Cursor 都能用
4. 解决的是**真痛点**：AI 写代码易翻车，缺的是"工程纪律"

---

## 六、小结

AI 编码助手强不强，一半看模型，一半看"你给它立的规矩"。这套技能包就是把规矩立好了，让免费/平价的模型也能产出接近资深工程师的代码。

一条命令白嫖 24 个生产级技能，不试白不试。

更多 AI 白嫖教程见博客 **0149.cc.cd**。装的过程中卡住，评论区甩报错，我看到回你。

---

*本文由「先生靠谱」整理，项目事实来自 GitHub 仓库 addyosmani/agent-skills 公开 README。*
