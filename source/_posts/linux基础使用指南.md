---
title: linux基础使用指南
date: 2026-02-26 10:13:42
tags:
  - Linux
  - 命令行
categories:
  - 学习
---

## 前言
这篇文章用于整理 Linux 的基础使用命令，覆盖日常开发和服务器操作中最常见的场景。

更详细内容可以查看：`https://www.runoob.com/linux/linux-tutorial.html`

---

## 一、目录与文件操作

```bash
# 查看当前路径
pwd

# 查看当前目录文件（-a 包含隐藏文件，-l 显示详细信息）
ls
ls -al

# 切换目录
cd /path/to/dir
cd ..          # 返回上一级
cd ~           # 回到用户主目录

# 创建目录
mkdir demo
mkdir -p a/b/c # 递归创建多级目录

# 创建空文件
touch hello.txt

# 复制 / 移动 / 重命名
cp a.txt b.txt
cp -r dir1 dir2
mv old.txt new.txt

# 删除文件 / 目录（谨慎）
rm a.txt
rm -r dir
rm -rf dir     # 强制递归删除，危险
```

---

## 二、查看文件内容

```bash
# 直接输出全文
cat file.txt

# 分页查看（推荐查看大文件）
less file.txt

# 查看前/后 N 行
head -n 20 file.txt
tail -n 20 file.txt

# 实时追踪日志
tail -f app.log
```

---

## 三、搜索与查找

```bash
# 在文件中搜索关键词（-n 显示行号）
grep -n "keyword" file.txt

# 递归搜索目录中的文本
grep -R "keyword" ./project

# 按文件名查找
find . -name "*.md"
find /var/log -name "*.log"

# 查找命令路径
which python
```

---

## 四、权限与所有者

```bash
# 查看文件权限
ls -l

# 修改权限
chmod 755 script.sh
chmod +x run.sh

# 修改文件所属用户/组
chown user:user file.txt
chown -R user:user /data/project
```

常见权限位：
- `r`：读
- `w`：写
- `x`：执行

---

## 五、进程与系统状态

```bash
# 查看进程
ps -ef
ps -ef | grep nginx

# 实时查看系统资源
top

# 结束进程
kill PID
kill -9 PID      # 强制结束，谨慎

# 查看内存/磁盘
free -h
df -h
du -sh *
```

---

## 六、网络相关

```bash
# 测试连通性
ping 8.8.8.8

# 查看本机 IP（新系统常用）
ip addr

# 查看端口占用
ss -lntp

# 下载文件
wget https://example.com/file.tar.gz
curl -O https://example.com/file.tar.gz
```

---

## 七、压缩与解压

```bash
# 打包并压缩
tar -czvf archive.tar.gz mydir

# 解压
tar -xzvf archive.tar.gz

# zip / unzip
zip -r files.zip mydir
unzip files.zip
```

---

## 八、软件包管理（常见发行版）

Debian / Ubuntu：

```bash
sudo apt update
sudo apt install git
sudo apt remove git
```

CentOS / RHEL / Fedora：

```bash
sudo dnf install git
# 老版本也可能是 yum install git
```

---

## 九、常见高频组合

```bash
# 查看最近命令
history

# 查看命令帮助
command --help
man command

# 管道：把前一个命令输出交给后一个命令
ps -ef | grep python
cat app.log | grep ERROR
```

---

## 十、新手建议

- 先学会 `pwd`、`ls -al`、`cd`、`cp`、`mv`、`rm`。
- 先用 `ls`、`cat`、`less` 确认目标文件，再执行删除和覆盖。
- 生产环境谨慎使用 `rm -rf`、`kill -9`、`chmod -R`。
- 多用 `--help` 和 `man`，这是最快的自助学习方式。
