---
title: GitHub 工作流（Git 基本指令）
date: 2026-02-25 10:54:39
tags:
  - Git
  - GitHub
categories:
  - 技术
---

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

## 六、常见问题提示

- 首次推送失败多为远程地址或权限问题，先检查 `git remote -v`。
- 分支名如果不是 `main`，把命令里的 `main` 改成你的实际主分支。
