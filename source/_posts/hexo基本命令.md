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

本文记录我在使用 **Hexo + GitHub Pages** 搭建个人博客后的基础操作流程，覆盖写作、上传源代码与发布静态网页。

---

## 一、目录结构速览

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

---

## 二、日常写作与发布流程

推荐流程：创建文章 → 写内容 → 本地预览 → 生成并部署。

```bash
# 创建新文章
hexo new "文章标题"

# 本地预览（浏览器访问 http://localhost:4000）
hexo s

# 生成并部署（推荐一步完成）
hexo g -d

# 如遇到生成异常，先清理再生成
hexo clean
hexo g
```

常用命令对照：
- `hexo new`：创建文章
- `hexo s` / `hexo server`：本地预览
- `hexo g` / `hexo generate`：生成静态文件
- `hexo d` / `hexo deploy`：部署

---

## 三、源码分支与静态分支

当前策略：**源码分支 `source`**，**静态页面分支 `master`**。

说明：
- `source`：提交 Hexo 项目文件（配置、主题、`source/` 文章等）。
- `master`：由 Hexo 生成并部署的静态页面，通常不手动编辑。
- `public/` 是生成后的静态文件目录，一般不手动修改。

源码提交示例：

```bash
git checkout source
git add -A
git commit -m "update posts"
git push
```

部署静态页面：

```bash
hexo g -d
```

---

## 四、部署配置（`_config.yml`）

当前项目配置示例：

```yml
deploy:
  type: git
  repo: git@github.com:chenhhhsg/chenhhhsg.github.io.git
  branch: master
```

如需迁移仓库，只需要修改 `repo` 地址即可，分支继续使用 `master`。

---

## 五、补充：图片、草稿与常见问题

图片管理建议：
```md
![示例图](/images/example.png)
```

草稿流程：

```bash
# 创建草稿（存到 source/_drafts）
hexo new draft "文章标题"

# 发布草稿（移动到 source/_posts）
hexo publish "文章标题"
```

部署后页面不更新的常见原因：
- GitHub Pages 可能有缓存，稍等几分钟再刷新。
- 仓库的 Pages 分支/目录设置要指向 `master` 并选择根目录。
- 确认 `hexo g -d` 成功执行，且 `_config.yml` 的 `deploy` 配置正确。

依赖安装与更新：

```bash
npm install
npm update
```
