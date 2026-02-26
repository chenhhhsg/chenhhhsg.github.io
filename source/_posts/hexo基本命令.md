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

这篇文章整理 Hexo 的高频操作，按“先配置、再写作、再发布”的顺序组织，尽量做到开箱即用。

---

## 一、先准备好（一次性配置）

### 1）安装依赖

```bash
npm install
```

### 2）确认部署配置（`_config.yml`）

```yml
deploy:
  type: git
  repo: git@github.com:chenhhhsg/chenhhhsg.github.io.git
  branch: master
```

说明：
- `repo`：你的 GitHub Pages 仓库地址。
- `branch`：静态页面分支（当前为 `master`）。

---

## 二、目录结构速览（常用部分）

```text
hexo-blog
├── _config.yml              # 全局配置
├── _config.landscape.yml    # 主题配置（菜单等）
├── source/_posts            # 文章目录
├── source/images            # 图片目录
└── public                   # 生成后的静态文件
```

---

## 三、日常写作与发布流程（最常用）

推荐流程：创建文章 → 写内容 → 本地预览 → 生成并部署。

```bash
# 创建新文章
hexo new "文章标题"

# 本地预览（浏览器访问 http://localhost:4000）
hexo s

# 生成并部署（一步完成）
hexo g -d

# 如遇到生成异常，先清理再生成
hexo clean
hexo g
```

---

## 四、内容管理（图片与草稿）

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

---

## 五、文章超链接索引页（点击标题进入文章）

Hexo 原生就能实现“按标题点击进入文章”的索引页，最简单做法是使用 `/posts`。

### 1）在 `_config.yml` 开启文章索引路径

```yml
index_generator:
  path: posts
  per_page: 10
  order_by: -date
```

这会把文章列表生成到 `/posts`，每篇标题都可点击进入详情页。

### 2）把索引入口放到顶部菜单

编辑 `_config.landscape.yml`：

```yml
menu:
  Home: /
  索引: /posts
  Archives: /archives
```

### 3）本地预览

```bash
hexo s
```

浏览器打开 `http://localhost:4000/posts/`，即可看到可点击的文章标题列表。

### 4）可选：做一个“手工精选索引页”

如果想做“只展示精选文章”的目录页，可以新建页面并手工维护链接：

```bash
hexo new page index-list
```

在 `source/index-list/index.md` 中写：

```md
- [Hexo 基础操作流程（从写作到发布）](/2026/01/04/hexo基本命令/)
- [GitHub 工作流（Git 基本指令）](/2026/02/25/github工作流/)
```

---

## 六、分支与提交建议

当前策略：
- `source` 分支：Hexo 源码（文章、配置、主题）。
- `master` 分支：部署后的静态页面（由 `hexo g -d` 生成并推送）。

源码提交示例：

```bash
git checkout source
git add -A
git commit -m "update posts"
git push
```

---

## 七、常见问题排查

- 页面没更新：先确认 `hexo g -d` 是否成功，再等待几分钟刷新缓存。
- 部署失败：检查 `_config.yml` 的 `deploy.repo` 与 SSH 权限是否正确。
- 菜单没变化：确认修改的是 `_config.landscape.yml`，并执行过 `hexo clean && hexo g`。

---

## 八、命令速查

- `hexo new "标题"`：创建文章
- `hexo s`：本地预览
- `hexo g`：生成静态文件
- `hexo d`：部署
- `hexo g -d`：生成并部署
- `hexo clean`：清理缓存后重建
