---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/01-Linux/Linux 用户与用户组管理/","title":"Linux 用户与用户组管理","tags":["Linux/用户管理"],"dg-note-properties":{"title":"Linux 用户与用户组管理","created":"2026-10-13","tags":["Linux/用户管理"],"aliases":["Linux 用户管理","Linux 用户组管理"]}}
---


# Linux 用户与用户组管理

Linux 是多用户系统，每个使用者对应一个**用户**，用户默认自动加入一个同名的**用户组**（主组）。

## 用户管理

### 添加用户

```bash
useradd 选项 用户名
```

| 选项 | 作用 |
|------|------|
| `-c` comment | 指定一段注释性描述 |
| `-d` 目录 | 指定用户主目录；若目录不存在，配合 `-m` 可创建 |

```bash
useradd tom            # 添加新用户 tom
cat /etc/passwd        # 查看用户是否创建成功
```

### 修改用户

```bash
usermod 选项 用户名
```

| 选项 | 作用 |
|------|------|
| `-d` | 修改用户家目录 |
| `-m` | 配合 `-d` 使用，移动家目录内容 |
| `-l` | 修改用户名 |

```bash
usermod -l tom2 tom    # 修改用户 tom 的用户名为 tom2
```

### 删除用户

```bash
userdel 选项 用户名
```

| 选项 | 作用 |
|------|------|
| `-r` | 把用户的主目录一起删除 |

```bash
userdel -r tom2        # 删除用户 tom2 及它的家目录
```

## 用户组管理

```bash
groupadd group1                 # 增加用户组
cat /etc/group                  # 查看用户组是否创建成功
groupmod -n admin_group group1  # 修改组名（-n 最常用）
groupdel admin_group            # 删除用户组
```

## 用户密码管理

```bash
passwd 选项 用户名
```

| 选项 | 作用 |
|------|------|
| `-d` | 清空用户密码（无密码登录） |
| `-f` | 强制用户下次登录必须修改密码 |
| `-l` | 锁定用户（禁止登录，但不删用户、不删文件） |
| `-u` | 解锁用户（恢复登录权限） |

```bash
passwd tom        # 修改指定用户 tom 的密码
passwd -d tom     # 清空 tom 的密码（空口令）
passwd -l tom     # 锁定 tom 的账号
passwd -u tom     # 解锁 tom 的账号
```

> [!warning] Kali 的 root 账号
> Kali 系统刚开始一般**没有启用 root 用户**，需要自行设置密码：
> 1. `sudo passwd root`
> 2. 先输入当前普通用户的密码（默认是 `kali`）
> 3. 输入给 root 设置的新密码（输入时不显示，属正常现象）
> 4. 再次确认密码
> 5. 提示 `passwd: password updated successfully` → 成功

## 相关笔记

- [[计算机相关技术知识库/01-Linux/Linux 入门概览\|Linux 入门概览]] — 起点：发行版选择与 SSH 远程连接
- [[计算机相关技术知识库/01-Linux/Linux 基础命令\|Linux 基础命令]] — whoami / who 查看当前登录用户
- [[计算机相关技术知识库/01-Linux/Linux 权限与 sudo\|Linux 权限与 sudo]] — su 切换用户、sudo 提权、配置文件字段详解
- [[计算机相关技术知识库/04-速查表/Linux 命令速查\|Linux 命令速查]] — 用户与组命令一页速查
