---
title: GitHub 工作流（Git 基本指令）
date: 2026-02-25 10:54:39
tags:
  - Git
  - GitHub
categories:
  - 技术
---
更详细内容可见这篇：`https://www.cnblogs.com/jamiechoo/articles/18408791`

## 目标

记录在 GitHub 上使用 Git 的基础命令与常见流程，适合日常提交与协作。

---

## 一、一次性配置

```bash
# 设置用户名与邮箱（会显示在提交记录里）
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# 查看配置
git config --global --list
```

---

## 二、从零开始（初始化并首次推送）

```bash
# 在本地初始化仓库
git init

# 绑定远程仓库（替换为你的 GitHub 仓库地址）
git remote add origin git@github.com:username/repo.git

# 创建 README 或进行第一次改动
git add -A
git commit -m "init"

# 首次推送
git push -u origin main
```

如果是已有仓库，直接克隆：

```bash
git clone git@github.com:username/repo.git
```

---

## 三、日常更新流程

```bash
# 查看状态
git status

# 暂存改动
git add -A

# 提交
git commit -m "update"

# 拉取远端最新（可选）
git pull

# 推送到 GitHub
git push
```

---

## 四、分支常用操作

```bash
# 查看分支
git branch

# 新建并切换分支
git switch -c feature/xxx

# 切回主分支
git switch main

# 合并分支（在 main 上执行）
git merge feature/xxx
```

---

## 五、常用排查命令

```bash
# 查看提交历史
git log --oneline --graph --decorate

# 查看改动内容
git diff

# 临时保存改动
git stash
git stash pop
```

---

## 六、git reset：回滚代码

`git reset` 用于把当前分支回退到某个提交，并根据参数决定是否保留工作区/暂存区改动。

```bash
# 仅回退提交记录，保留工作区和暂存区改动
git reset --soft HEAD~1

# 回退提交记录，保留工作区改动（清空暂存区）
git reset --mixed HEAD~1

# 回退提交记录，同时丢弃工作区改动（谨慎）
git reset --hard HEAD~1
```

常用场景：
- 提交写错了，想重新整理提交内容：用 `--soft`。
- 只想撤销提交、保留本地改动：用 `--mixed`（默认）。
- 想彻底丢弃最近改动：用 `--hard`（不可逆，谨慎）。

提示：
- 回退到指定提交：`git reset --hard <commit_id>`。
- 如果提交已经推到远端，回滚需谨慎，建议用 `git revert` 生成反向提交。

### 与 `git revert` 的对比

`git revert` **不会改写历史**，而是创建一个“反向提交”来撤销某次提交的效果，适合已推送到远端的场景。

```bash
# 撤销某次提交（生成一个新的反向提交）
git revert <commit_id>

# 连续撤销多个提交（按顺序生成多个反向提交）
git revert <commit_id1> <commit_id2>
```

简要对比：
- `git reset`：移动分支指针，可能改写历史；更适合本地或未推送的提交。
- `git revert`：新增反向提交，保留历史；更适合已推送的提交。

---

## 七、.gitignore 的用法与格式

`.gitignore` 用来告诉 Git **哪些文件/目录不需要纳入版本控制**。常见场景是忽略构建产物、依赖目录、临时文件、日志等。

基本规则：
- 每行一个匹配规则
- 以 `#` 开头的是注释
- 以 `/` 结尾表示目录
- `*` 匹配任意字符，`?` 匹配单个字符
- `**` 可跨目录匹配
- 以 `!` 开头表示“取消忽略”

示例：

```gitignore
# 依赖目录
node_modules/

# 构建输出
public/
dist/

# 系统文件
.DS_Store
Thumbs.db

# 日志
*.log

# 忽略所有 .env 文件，但保留示例
.env*
!.env.example

# 忽略某个目录下的所有临时文件
temp/**
```

使用方式：

```bash
# 新建或编辑 .gitignore
touch .gitignore

# 将忽略规则写入后提交
git add .gitignore
git commit -m "add .gitignore"
```

注意：
- `.gitignore` **只对未追踪文件生效**，已被追踪的文件不会自动移除。
- 若要停止追踪已提交的文件，可以先执行 `git rm --cached 文件名` 再提交。

---

## 八、常见问题提示

- 首次推送失败多为远程地址或权限问题，先检查 `git remote -v`。
- 分支名如果不是 `main`，把命令里的 `main` 改成你的实际主分支。
