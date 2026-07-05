---
title: "AI 一句话帮你部署博客：Hugo + GitHub + Cloudflare Pages 完整教程"
date: 2026-06-29T10:00:00+08:00
draft: false
tags: ["AI", "Hermes", "博客部署", "Cloudflare", "Hugo", "教程", "GitHub"]
categories: ["AI 工具"]
summary: "用 AI 代理（Hermes）一句话完成博客搭建：Hugo + GitHub + Cloudflare Pages，从零到上线全流程。"
description: "AI 一句话帮你部署博客：Hugo + GitHub + Cloudflare Pages 完整教程。用 AI 代理（Hermes）一句话完成博客搭建：Hugo + GitHub + Cloudflare Pages，从零到上线全流程。"
keywords: ["hugo 博客部署", "cloudflare pages 教程", "github pages 博客", "ai 部署博客", "hugo 教程"]
---

## 前言

本教程记录了用 AI 代理（Hermes）全自动完成博客搭建的全过程。整个流程只需要对 AI 说一句"帮我部署博客"，它会自动完成所有操作。

最终效果：博客地址 [www.0149.cc.cd](https://www.0149.cc.cd)，基于 Hugo + PaperMod 主题，部署在 Cloudflare Pages。

## 技术栈

| 组件 | 用途 |
|------|------|
| Hugo | 静态博客引擎 |
| PaperMod | 博客主题（暗黑模式） |
| GitHub | 代码托管 |
| Cloudflare Pages | 免费部署 |
| 0149.cc.cd | 自定义域名 |

## 第一步：安装 Hugo

Hugo 是一个静态网站生成器，速度快、配置简单。

```bash
# 安装 Hugo（需要 Node.js）
npm install -g hugo-extended

# 验证安装
hugo version
# 输出类似：hugo v0.163.3+extended
```

## 第二步：创建博客项目

```bash
# 创建博客目录
hugo new site blog-site
cd blog-site

# 安装 PaperMod 主题
git init
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

## 第三步：配置博客

创建 `hugo.toml` 配置文件：

```toml
baseURL = "https://www.0149.cc.cd/"
languageCode = "zh"
defaultContentLanguage = "zh"
title = "千华落影"
theme = "PaperMod"
paginate = 10

[params]
  author = "千华落影"
  description = "分享 AI、编程、部署等技术文章"
  defaultTheme = "dark"
  ShowReadingTime = true
  ShowShareButtons = false
  ShowPostNavLinks = true
  ShowBreadCrumbs = true
  ShowCodeCopyButtons = true
  ShowWordCount = true
  ShowToc = true

  [params.homeInfoParams]
    Title = "👋 你好，我是千华落影"
    Content = "一个技术博主，分享 AI 大模型、本地部署、编程开发等实用教程。"

[[params.socialIcons]]
  name = "github"
  url = "https://github.com/linhuiguo"
```

## 第四步：创建第一篇文章

```bash
# 创建文章
hugo new content posts/my-first-post.md
```

编辑 `content/posts/my-first-post.md`：

```markdown
---
title: "我的第一篇文章"
date: 2026-06-29T10:00:00+08:00
draft: false
tags: ["博客", "教程"]
categories: ["技术"]
---

## 开始写内容

这里是文章正文。
```

## 第五步：本地预览

```bash
# 启动本地服务器
hugo server -D

# 浏览器访问
# http://localhost:1313
```

`-D` 参数表示包含草稿文章。预览满意后，按 `Ctrl+C` 停止服务器。

## 第六步：推送到 GitHub

### 6.1 创建 GitHub 仓库

在 GitHub 网页端创建一个新仓库，例如 `blog`。

### 6.2 推送代码

```bash
cd blog-site

# 初始化 Git
git init
git add -A
git commit -m "feat: initial blog setup"

# 关联远程仓库
git remote add origin https://github.com/linhuiguo/blog.git

# 推送
git push -u origin main
```

## 第七步：Cloudflare Pages 部署

### 7.1 安装 Wrangler CLI

Wrangler 是 Cloudflare 的命令行工具。

```bash
npm install -g wrangler
```

### 7.2 登录 Cloudflare

```bash
wrangler login
```

会打开浏览器，完成 OAuth 授权。

### 7.3 创建 Pages 项目

```bash
cd blog-site

# 创建 Pages 项目
wrangler pages project create blog --production-branch main
```

### 7.4 部署

```bash
# 构建 Hugo 站点
hugo

# 部署到 Cloudflare Pages
wrangler pages deploy public --project-name blog --branch main
```

部署完成后，会得到一个 `xxx.pages.dev` 的临时地址。

### 7.5 设置 Hugo 版本（重要）

Cloudflare Pages 需要知道 Hugo 版本，否则可能构建失败。

```bash
# 在 Cloudflare Pages 控制台设置环境变量：
# HUGO_VERSION = 0.163.3
```

或者通过 CLI：

```bash
wrangler pages deployment create --project-name blog \
  --branch main \
  --env HUGO_VERSION=0.163.3
```

## 第八步：绑定自定义域名

### 8.1 在 Cloudflare Pages 添加域名

```bash
# 通过 API 添加自定义域名
curl -X POST \
  -H "Authorization: Bearer $CF_TOKEN" \
  -H "Content-Type: application/json" \
  "https://api.cloudflare.com/client/v4/accounts/$ACCOUNT_ID/pages/projects/blog/domains" \
  -d '{"name":"www.0149.cc.cd"}'
```

### 8.2 配置 DNS 记录

在 Cloudflare DNS 控制台添加 CNAME 记录：

| 类型 | 名称 | 目标 | 代理状态 |
|------|------|------|----------|
| CNAME | `www` | `blog.pages.dev` | 已代理 |
| CNAME | `@` | `blog.pages.dev` | 已代理 |

### 8.3 等待 SSL 证书

绑定域名后，Cloudflare 会自动签发 SSL 证书，通常几分钟内生效。

## 第九步：验证部署

```bash
# 检查博客是否可访问
curl -I https://www.0149.cc.cd

# 应该返回 HTTP/2 200
```

## 发布新文章

以后发布新文章的流程：

```bash
# 1. 创建新文章
hugo new content posts/new-article.md

# 2. 编辑文章内容
# 用你喜欢的编辑器打开 content/posts/new-article.md

# 3. 本地预览
hugo server -D

# 4. 提交并推送
git add -A
git commit -m "feat: add new article"
git push

# 5. Cloudflare Pages 自动部署
# 推送到 GitHub 后，Cloudflare Pages 会自动触发构建和部署
```

## 项目结构

```
blog-site/
├── hugo.toml                 # Hugo 配置文件
├── content/
│   ├── posts/                # 文章目录
│   │   ├── my-first-post.md
│   │   └── new-article.md
│   └── search.md             # 搜索页面
├── themes/
│   └── PaperMod/             # 主题（git submodule）
└── public/                   # 构建输出（自动生成）
```

## 总结

| 步骤 | 操作 |
|------|------|
| 安装 Hugo | `npm install -g hugo-extended` |
| 创建项目 | `hugo new site blog-site` |
| 安装主题 | `git submodule add` PaperMod |
| 写文章 | `hugo new content posts/xxx.md` |
| 本地预览 | `hugo server -D` |
| 推送 GitHub | `git push` |
| 部署 CF Pages | `wrangler pages deploy` |
| 绑定域名 | 添加 CNAME 记录 |

整个流程大约需要 30-60 分钟（首次），后续发布文章只需写 Markdown + git push。

## 参考资料

- [Hugo 官方文档](https://gohugo.io/documentation/)
- [Cloudflare Pages 文档](https://developers.cloudflare.com/pages/)
- [PaperMod 主题](https://github.com/adityatelange/hugo-PaperMod)
- [Wrangler CLI 文档](https://developers.cloudflare.com/workers/wrangler/)
