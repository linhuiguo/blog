---
title: "一条龙！GitHub 选题 → AI 写文 → 自动配图 → 直接发布（保姆级）"
date: 2026-07-12T00:00:00+08:00
draft: false
tags: ["自动化", "内容生产", "Hermes", "AI 配图", "保姆级教程"]
categories: ["效率工具"]
summary: "从 GitHub 热榜选题到文章发布，全自动一条龙，每天 3 篇不是问题。"
description: "一条龙内容生产流水线：GitHub 热榜选题→AI 写小白教程→归藏材质配图→填头条编辑框→发博客，全自动完成。"
keywords: ["内容自动化", "AI 写作", "配图", "头条", "GitHub 热榜", "0149"]
---

# 一条龙！GitHub 选题 → AI 写文 → 自动配图 → 直接发布（保姆级）

> 内容创作者最怕什么？每天想选题、写文章、找配图、排版发布，一个人干四个人的活。
> 今天把我的**一条龙流水线**公开：从 GitHub 热榜扫选题 → AI 写小白教程 → 归藏材质自动配图 → 填头条编辑框 → 发博客，全程自动化。

## 这条流水线能干嘛

我每天靠它稳定输出 2-3 篇文章，每篇都有配图，从选题到填进头条编辑框不超过 30 分钟。核心思想就四个字：**信息差**。

GitHub 上每天都有新项目爆火，但大部分人不知道、知道了也不会装——这就是信息差。把热榜项目翻译成小白能看懂的保姆级教程，就是流量密码。

## 流水线分 5 步

### 第一步：扫 GitHub 热榜

```
curl -s 'https://api.github.com/search/repositories?q=created:>7天前+stars:>300&sort=stars&order=desc' | python3 -c "import sys,json; [print(f'★{i[\"stargazers_count\"]} | {i[\"full_name\"]}') for i in json.load(sys.stdin).get('items',[])]"
```

筛选标准：
- 星星 > 300（有热度）
- 小白能装（双击 exe / 一行命令 / 浏览器打开）
- 有钩子（大厂出品 / 免费 / 信息差）

### 第二步：写小白教程

按固定模板：标题带钩子 → 先说清 → 准备清单 → 第一步检查依赖 → Win10/Win11 差异 → 命令先检查再安装 → 避坑表。

### 第三步：AI 配图

用归藏材质插画风格（见上篇文章），给每篇文章生成带中文标签的 3D 材质解释图。

### 第四步：填头条编辑框

Edge CDP 启动后，一行命令填进头条：

```bash
cd /c/Users/guo12/auto-video-edit
PYTHONPATH= python312 fill_only.py
```

脚本自动填标题+正文，不点发布，留你修改配图后手动发。

### 第五步：发博客

```bash
cd /c/Users/guo12/blog-site
hugo && git add -A && git commit -m "feat: 文章名" && git push
```

## 效果

下面这张图就是这条流水线的概念图（归藏材质风格生成）：

![一条龙流水线](/images/guizang/auto_pipeline.png)

## 一件事提醒

**头条不加引流链接**：结尾只写"我是先生靠谱，专讲小白能看懂的实操。"，不写博客地址，避免限流。

---

**我是先生靠谱，专讲小白能看懂的实操。**