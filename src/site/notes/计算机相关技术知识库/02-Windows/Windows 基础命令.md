---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/02-Windows/Windows 基础命令/","title":"Windows 基础命令","tags":["Windows/命令"],"dg-note-properties":{"title":"Windows 基础命令","created":"2026-10-13","tags":["Windows/命令"],"aliases":["CMD 基础命令","Windows 常用命令"]}}
---


# Windows 基础命令

以下命令在 **CMD（命令提示符）** 中执行。

## 常用基础命令

| 命令 | 作用 |
|------|------|
| `cls` | 清屏 |
| `whoami` | 查看当前用户 |
| `net user` | 查看计算机上的用户 |
| `systeminfo` | 查看计算机信息 |
| `ver` | 显示系统版本号 |
| `hostname` | 显示当前主机名 |
| `date` | 显示日期 |
| `echo Hello world` | 输出内容 |

```bash
cls                 # 清屏
whoami              # 查看当前用户
net user            # 查看计算机上的用户
systeminfo          # 查看计算机信息（较慢，信息全）
ver                 # 显示系统版本号
hostname            # 显示当前主机名
date                # 显示日期
echo Hello world    # 输出内容
```

## Windows 内置账户一览

| 账户名 | 账户类型 | 默认状态 |
|--------|---------|---------|
| （你的账户） | 本地管理员账户 | 启用（当前登录用户） |
| Administrator | 内置超级管理员账户 | 默认禁用（Win10/11） |
| DefaultAccount | 系统内置默认账户 | 默认禁用 |
| Guest | 来宾账户 | 默认禁用 |
| WDAGUtilityAccount | Windows Defender 应用防护账户 | 系统专用，默认启用 |
| WsiAccount | 系统服务/第三方工具账户 | 按需启用 |

> [!note] Administrator
> 内置超级管理员 `Administrator` 在 Windows 10/11 中**默认禁用**，与日常使用的本地管理员账户不同——前者权限不受 UAC 限制。

## 字符编码设置（chcp）

```bash
chcp            # 查看命令行环境字符编码（全局设置）
chcp 65001      # 切换为 UTF-8
```

| 编码页 | 含义 |
|--------|------|
| 936 | GBK（一般情况下为默认编码） |
| 437 | 美国英语 |
| 65001 | UTF-8 |
| 1200 | UTF-16 |
| 12000 | UTF-32 |

> [!tip] 中文乱码怎么办
> CMD 输出中文乱码时，先执行 `chcp 65001` 切换到 UTF-8。

## 相关笔记

- [[计算机相关技术知识库/02-Windows/Windows 目录与文件操作\|Windows 目录与文件操作]] — dir、cd、copy 等
- [[计算机相关技术知识库/02-Windows/Windows 进程管理\|Windows 进程管理]] — tasklist、taskkill
- [[计算机相关技术知识库/02-Windows/Windows 网络命令\|Windows 网络命令]] — ipconfig、ping、netstat
