---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/01-Linux/Linux 权限与 sudo/","title":"Linux 权限与 sudo","tags":["Linux/权限"],"dg-note-properties":{"title":"Linux 权限与 sudo","created":"2026-10-13","tags":["Linux/权限"],"aliases":["Linux sudo","Linux root"]}}
---


# Linux 权限与 sudo

## 切换用户

```bash
su 用户名        # 切换到指定用户（su = switch user）
```

## sudo：以管理者身份执行指令

```bash
sudo 命令        # 以系统管理者（root）的身份执行指令
```

| 选项 | 作用 |
|------|------|
| `-V` | 显示版本编号 |
| `-h` | 显示版本编号及指令的使用方式说明 |
| `-l` | 显示出自己（执行 sudo 的使用者）的权限 |
| `-u` | 代表要以 root 的身份执行指令 |

> [!tip] sudo vs su
> - `su`：直接切换成另一个用户（需要知道对方密码）
> - `sudo`：以 root 身份执行**某一条命令**（需要当前用户在 sudo 白名单中），更安全、更可控

## 四大用户信息配置文件

| 文件 | 内容 |
|------|------|
| `/etc/passwd` | 用户信息 |
| `/etc/shadow` | 密码文件 |
| `/etc/group` | 用户组文件 |
| `/etc/gshadow` | 组密码 |

## /etc/passwd 七个字段详解

`/etc/passwd` 每行一个用户，字段之间用 `:` 分隔：

| 字段 | 含义 | 说明 |
|------|------|------|
| 第 1 字段 | 用户名（Username） | 登录名 |
| 第 2 字段 | 密码占位符 | 不是真实密码，只是占位符（真实密码在 /etc/shadow） |
| 第 3 字段 | UID（用户 ID） | `0` = root 超级管理员；`1-999` = 系统服务/伪用户（不能登录）；`1000+` = 普通可登录用户 |
| 第 4 字段 | GID（用户组 ID） | 创建用户时默认自动创建同名主组，GID 和 UID 一致 |
| 第 5 字段 | 用户注释信息（GECOS） | 描述性文字 |
| 第 6 字段 | 家目录（Home Directory） | 登录后默认进入的目录；普通用户在 `/home/用户名`，系统服务用户在 `/var/lib/` 下 |
| 第 7 字段 | 登录 Shell（Login Shell） | 用户登录后启动的命令行解释器，决定用户能否登录 |

> [!example] 示例
> ```bash
> cat /etc/passwd
> # tom:x:1000:1000::/home/tom:/bin/bash
> #  用户:占位:UID:GID:注释:家目录:登录Shell
> ```

## 相关笔记

- [[计算机相关技术知识库/01-Linux/Linux 用户与用户组管理\|Linux 用户与用户组管理]] — 用户与组的增删改
- [[计算机相关技术知识库/01-Linux/Linux 文件权限\|Linux 文件权限]] — 文件级的 rwx 权限控制
