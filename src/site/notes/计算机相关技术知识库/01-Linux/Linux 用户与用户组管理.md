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

## 实战：创建、切换、删除 test01

```bash
sudo useradd -m test01        # 创建用户 test01，并创建家目录
sudo passwd test01            # 为 test01 设置密码
su - test01                   # 切换到 test01（加载其环境与家目录）
whoami                        # 确认当前用户，应输出 test01
exit                          # 退回原来的用户
sudo userdel -r test01        # 删除用户，同时删除家目录和邮件池
grep test01 /etc/passwd       # 检查是否删除干净
```

| 命令 / 选项 | 说明 |
|-------------|------|
| `useradd -m` | 创建家目录；部分发行版（如 Debian/Ubuntu）的 `useradd` 默认**不**创建家目录，必须加 `-m` |
| `su - test01` | 带 `-` 相当于完整登录：切换到 test01 的家目录，并加载其环境变量与配置文件 |
| `su test01` | 不带 `-`：只切换身份，保留当前工作目录和环境变量 |
| `userdel -r` | 删除用户的同时删除其家目录和邮件池（mail spool） |
| `grep test01 /etc/passwd` | 无输出说明用户记录已删除 |

> [!note] /etc/passwd 里没有真实密码
> `/etc/passwd` 对所有用户可读，密码字段只是占位符 `x`；真正的密码哈希保存在只有 root 可读的 `/etc/shadow` 中。

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
| `-e` / `--expire` | 使密码立即过期，强制用户下次登录修改密码 |
| `-l` | 锁定用户（禁止登录，但不删用户、不删文件） |
| `-u` | 解锁用户（恢复登录权限） |

```bash
passwd tom        # 修改指定用户 tom 的密码
passwd -d tom     # 清空 tom 的密码（空口令）
passwd -e tom     # 让密码立即过期，下次登录必须改密
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
