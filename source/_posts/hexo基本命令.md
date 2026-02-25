---
title: Hexo 基础操作流程（从写作到发布）
date: 2026-01-04 19:00:00
tags:
  - Hexo
  - 博客
  - GitHub Pages
categories:
  - 技术
---

## 前言

本文记录我在使用 **Hexo + GitHub Pages** 搭建个人博客后的**基础操作流程**，适合刚开始使用 Hexo 的新手，用来快速上手日常写作与发布。

---

## 一、Hexo 博客的整体结构

一个典型的 Hexo 博客目录结构如下：

```text
hexo-blog
├── _config.yml        # 全局配置文件
├── package.json
├── node_modules
├── source
│   ├── _posts         # 博客文章目录（最常用）
│   └── images         # 图片资源（可选）
├── themes             # 主题目录
└── public             # 生成后的静态文件
```

## 二、为什么写博客

写博客可以帮助我：
- 记录学习过程
- 梳理知识结构
- 沉淀个人经验

使用 Hexo 写博客的流程很简单：先用 `hexo new "文章标题"` 创建文章，Hexo 会在 `source/_posts` 目录生成对应的 Markdown 文件；每篇文章顶部都有 Front-matter，用来设置标题、时间、分类和标签。写作时推荐用 VS Code 编辑正文，并在发布前用本地预览确认效果；确认无误后再生成静态文件并部署到 GitHub Pages。日常维护只要重复“创建文章 → 写内容 → 预览 → 发布”这条链路，就能稳定、高效地更新个人博客。

```bash
# 创建新文章
hexo new "文章标题"

# 本地预览（浏览器访问 http://localhost:4000）
hexo s

# 生成并部署到 GitHub Pages
hexo clean
hexo g
hexo d

# 或者一步完成（生成 + 部署）
hexo g -d
```
