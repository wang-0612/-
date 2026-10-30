---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/01-Linux/Linux 基础命令/","title":"Linux 基础命令","tags":["Linux/命令"],"dg-note-properties":{"title":"Linux 基础命令","created":"2026-10-13","tags":["Linux/命令"],"aliases":["Linux 常用命令"]}}
---


# Linux 基础命令

## 终端快捷键

| 快捷键 | 作用 |
|--------|------|
| `Ctrl + L` 或 `clear` | 清屏 |
| `Tab` | 快速补齐（命令、路径、文件名） |
| `Ctrl + C` | 终止正在执行的命令 |

> [!tip] Tab 补齐是效率核心
> 输入命令或路径的前几个字符后按 Tab，系统会自动补全；有歧义时再按一次 Tab 会列出所有候选。

## 系统与登录

```bash
reboot          # 重启
exit            # 退出登录
```

## 查看主机信息

```bash
uname           # 显示系统名称
uname -a        # 显示主机名、内核版本、硬件平台等详细信息
uname -r        # 显示内核版本
hostname        # 查看主机名
```

## 查看网络与用户

```bash
ifconfig        # 查看 IP 地址信息（传统工具）
ip addr         # 查看 IP 地址信息（现代工具）
whoami          # 查看当前用户名
who             # 查看当前登录的用户
```

> [!note]
> 新版发行版中 `ifconfig` 已逐渐被 `ip` 命令取代（net-tools → iproute2），但两者都很常见。

## 相关笔记

- [[计算机相关技术知识库/01-Linux/Linux 入门概览\|Linux 入门概览]] — 起点：发行版选择与 SSH 远程连接
- [[计算机相关技术知识库/01-Linux/Linux 用户与用户组管理\|Linux 用户与用户组管理]] — 用户与组的增删改
- [[计算机相关技术知识库/01-Linux/Linux 进程与服务管理\|Linux 进程与服务管理]] — systemctl 服务管理
- [[计算机相关技术知识库/04-速查表/Linux 命令速查\|Linux 命令速查]] — 全部命令一页速查
- [[计算机相关技术知识库/04-速查表/网络基础速查\|网络基础速查]] — 网络基础与端口速查
