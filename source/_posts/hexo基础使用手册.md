---
title: Hexo 基础使用手册
date: 2026-01-04 19:00:00
tags:
  - Hexo
  - 博客
  - GitHub Pages
categories:
  - 技术
published: false
---

## 前言

这篇文章整理 Hexo 的基础使用全流程：从初始化、写作、预览、部署到排障，按“能直接落地”的顺序组织。

---

## 一、从 0 创建一个 Hexo 博客

### 1）安装 Node.js（建议 LTS）

先确认版本：

```bash
node -v
npm -v
```

### 2）安装 Hexo CLI

```bash
npm install -g hexo-cli
```

### 3）初始化项目

```bash
hexo init hexo-blog
cd hexo-blog
npm install
```

### 4）安装部署插件（GitHub Pages 必需）

```bash
npm install hexo-deployer-git --save
```

---

## 二、核心目录结构（先认识最常用部分）

```text
hexo-blog
├── _config.yml              # 全局配置（站点信息、URL、部署等）
├── _config.landscape.yml    # 主题配置（菜单、封面等）
├── package.json
├── source
│   ├── _posts               # 博文目录
│   ├── _drafts              # 草稿目录（可选）
│   └── images               # 图片目录
├── themes                   # 主题目录
└── public                   # 生成后的静态文件（不手改）
```

---

## 三、基础配置（站点可用的关键项）

编辑 `_config.yml`，至少确认以下内容：

```yml
# Site
title: 你的博客名
language: zh-CN
timezone: Asia/Shanghai

# URL（GitHub Pages 必改）
url: https://你的用户名.github.io
root: /

# 固定链接样式（可选）
permalink: :year/:month/:day/:title/

# 首页文章列表
index_generator:
  path: posts
  per_page: 10
  order_by: -date

# 部署
deploy:
  type: git
  repo: git@github.com:chenhhhsg/chenhhhsg.github.io.git
  branch: master
```

---

## 四、写文章、写页面、写草稿

### 1）新建文章

```bash
hexo new "文章标题"
```

### 2）新建独立页面（如 about、links）

```bash
hexo new page about
```

页面文件会生成在 `source/about/index.md`。

### 3）草稿流程

```bash
# 新建草稿（source/_drafts）
hexo new draft "文章标题"

# 发布草稿到 source/_posts
hexo publish "文章标题"
```

### 4）常用 Front-matter

```yml
---
title: 文章标题
date: 2026-02-26 12:00:00
tags:
  - Hexo
categories:
  - 技术
published: true
---
```

---

## 五、日常发布流程（最常用）

推荐流程：写作 -> 本地预览 -> 生成部署。

```bash
# 本地预览（http://localhost:4000）
hexo s

# 生成 + 部署（一步完成）
hexo g -d

# 若异常，先清理再生成
hexo clean
hexo g
```

---

## 六、文章超链接索引页（标题可点击）

Hexo 原生支持 `/posts` 作为文章列表页。  
你当前配置里 `index_generator.path: posts` 即对应这个路径。

在 `_config.landscape.yml` 添加菜单入口：

```yml
menu:
  Home: /
  索引: /posts
  Archives: /archives
```

访问 `http://localhost:4000/posts/` 即可看到可点击标题列表。

---

## 七、分类、标签、归档怎么用

### 1）在文章里写分类和标签

```yml
tags:
  - 强化学习
  - 机器人
categories:
  - 调参日志
```

### 2）访问自动生成页面

- 标签页：`/tags/`
- 分类页：`/categories/`
- 归档页：`/archives/`

如果主题菜单没有入口，可在 `_config.landscape.yml` 增加：

```yml
menu:
  Home: /
  索引: /posts
  Archives: /archives
  Categories: /categories
  Tags: /tags
```

---

## 八、图片与资源管理

你可以用两种方式：

### 1）全局图片目录（简单直观）

图片放到 `source/images`，文中用绝对路径引用：

```md
![示例图](/images/example.png)
```

优点：路径简单，适合多篇文章复用同一张图。

### 2）文章私有图片目录（post_asset_folder，推荐）

你当前已经开启：

```yml
post_asset_folder: true
marked:
  prependRoot: true
  postAsset: true
```

此时新建文章：

```bash
hexo new "我的文章"
```

会生成：
- `source/_posts/我的文章.md`
- `source/_posts/我的文章/`（同名资源目录）

把图片放到同名目录后，正文可直接写：

```md
![实验截图](result.png)
```

优点：文章和图片绑定，迁移/备份不容易丢图。

---

## 九、分支与备份建议

当前策略：

- `source` 分支：Hexo 源码（文章、配置、主题等）。
- `master` 分支：部署静态页面（`hexo g -d` 推送）。

源码提交流程：

```bash
git checkout source
git add -A
git commit -m "update posts"
git push
```

恢复环境（换电脑常用）：

```bash
git clone <你的源码仓库>
cd hexo-blog
npm install
hexo s
```

---

## 十、隐藏博客/隐藏文章

1. 隐藏整站（外网不可访问）  
在 GitHub 仓库 `Settings -> Pages` 关闭 Pages（`Source` 设为 `None`）。

2. 只本地查看（不再公开更新）  
只执行：

```bash
hexo s
```

3. 只隐藏某篇文章  
文章头部设置：

```yml
published: false
```

---

## 十一、常见问题排查

- 页面没更新：确认 `hexo g -d` 成功后，等待几分钟再刷新。
- 部署失败：检查 `deploy.repo`、SSH 权限、仓库地址是否正确。
- 菜单没变化：确认改的是 `_config.landscape.yml`，并执行 `hexo clean && hexo g`。
- 链接错误：优先检查 `_config.yml` 的 `url` 和 `root`。
- 本地能看线上 404：检查 GitHub Pages 分支是否指向部署分支（你当前是 `master`）。

---

## 十二、命令速查

- `hexo init <dir>`：初始化站点
- `hexo new "标题"`：新建文章
- `hexo new page about`：新建页面
- `hexo new draft "标题"`：新建草稿
- `hexo publish "标题"`：发布草稿
- `hexo s`：本地预览
- `hexo g`：生成静态文件
- `hexo d`：部署
- `hexo g -d`：生成并部署
- `hexo clean`：清理缓存后重建
