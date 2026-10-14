---
title: Linux 命令速查
created: 2026-10-13
tags:
  - 速查
  - Linux/命令
aliases:
  - Linux cheatsheet
dg-publish: true
---

# Linux 命令速查

一页汇总 Linux 常用命令。详细讲解见对应笔记链接。

## 系统与快捷键

| 命令 | 作用 |
|------|------|
| `Ctrl + L` / `clear` | 清屏 |
| `Tab` | 快速补齐 |
| `Ctrl + C` | 终止命令 |
| `reboot` | 重启 |
| `exit` | 退出登录 |
| `uname -a` | 系统详细信息 |
| `hostname` | 主机名 |
| `ifconfig` / `ip addr` | IP 地址信息 |
| `whoami` | 当前用户名 |
| `who` | 当前登录用户 |

## 用户与组

| 命令 | 作用 |
|------|------|
| `useradd tom` | 添加用户 |
| `usermod -l tom2 tom` | 改用户名 |
| `userdel -r tom2` | 删除用户及家目录 |
| `groupadd group1` | 添加用户组 |
| `groupmod -n 新组名 旧组名` | 改组名 |
| `groupdel 组名` | 删除用户组 |
| `passwd tom` | 修改密码 |
| `passwd -d / -l / -u tom` | 清空 / 锁定 / 解锁密码 |
| `sudo passwd root` | 给 root 设置密码（Kali） |
| `su 用户名` | 切换用户 |
| `sudo 命令` | 以 root 执行 |

## 文件与目录

| 命令 | 作用 |
|------|------|
| `ls -al` | 所有文件详细信息（含隐藏） |
| `mkdir -p a/b/c` | 递归创建目录 |
| `rmdir test1` | 删除空目录 |
| `cp -r dir1 dir2` | 复制目录 |
| `cp -a 1.txt 3.txt` | 复制并保留属性 |
| `rm -r dir1` | 删除目录及内容 |
| `rm -rf dir2` | 强制删除（危险） |
| `mv 1.txt abc.txt` | 移动 / 重命名 |
| `touch filename` | 创建空文件 |
| `cat / less abc.txt` | 查看文件内容 |

## 权限

| 命令 | 作用 |
|------|------|
| `chmod 644 test.txt` | 所有者读写，其他只读 |
| `chmod 755 test_dir/` | 所有者全权限，其他读+执行 |
| `chmod 755 test.sh` | 让脚本可执行 |

> 数字含义：`r=4` `w=2` `x=1`；顺序：所有者 → 所属组 → 其他

## 进程与服务

| 命令 | 作用 |
|------|------|
| `ps -aux` | 查看所有进程（含 CPU/内存） |
| `top` | 实时进程监视 |
| `ps -aux \| grep 进程名` | 查找进程 |
| `kill -9 PID` | 强制结束进程 |
| `systemctl start/stop/status 服务名` | 服务 启动/关闭/状态 |

## 关键配置文件

| 文件 | 内容 |
|------|------|
| `/etc/passwd` | 用户信息 |
| `/etc/shadow` | 密码 |
| `/etc/group` | 用户组 |
| `/etc/gshadow` | 组密码 |

## 相关笔记

- [[Linux 入门概览]] · [[Linux 基础命令]] · [[Linux 用户与用户组管理]]
- [[Linux 权限与 sudo]] · [[Linux 文件与目录操作]] · [[Linux 文件系统目录结构]]
- [[Linux 文件权限]] · [[Linux vim 文本编辑]] · [[Linux 进程与服务管理]]
