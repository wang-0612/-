---
{"dg-publish":true,"permalink":"/计算机相关技术知识/01-Linux/Linux 进程与服务管理/","title":"Linux 进程与服务管理","tags":["Linux/进程"],"dg-note-properties":{"title":"Linux 进程与服务管理","created":"2026-10-13","tags":["Linux/进程"],"aliases":["Linux 进程管理","Linux 服务管理","systemctl"]}}
---


# Linux 进程与服务管理

## 查看进程（ps）

```bash
ps        # 查看系统当前正在运行的进程
```

| 选项 | 作用 |
|------|------|
| `a` | 显示所有终端进程 |
| `u` | 显示用户、CPU、内存使用率 |
| `x` | 显示后台无终端进程 |

```bash
ps -a        # 列出所有运行中进程
ps -aux      # 查看所有进程并显示 CPU、内存占用（最常用）
```

## 实时查看进程（top）

```bash
top         # 实时查看进程（动态刷新，类似任务管理器）
```

按 `q` 退出 top。

## 查找指定进程

用管道把 `ps` 的输出交给 `grep` 过滤：

```bash
ps -aux | grep 进程名     # 查找包含某个名字的进程
ps -aux | grep ping      # 查找 ping 进程
```

> [!note] grep 的小技巧
> `ps -aux | grep xxx` 有时会把 grep 自己也匹配出来。可用 `ps -aux | grep [x]xx` 技巧性地排除。

## 结束进程（kill）

```bash
kill -9 PID       # 强制结束进程（必须用 PID）
```

完整流程：

```bash
ps -aux | grep ping    # 1. 先查找 ping 进程，拿到 PID
kill -9 1234           # 2. 根据 PID 强制结束
```

> [!warning] kill -9 的含义
> `-9` 即 SIGKILL 信号，**强制杀死**，进程没有机会清理现场（保存数据、释放资源）。能用默认 `kill PID` 优雅结束就不要用 `-9`。

## 服务管理（systemctl）

systemd 是现代 Linux 的服务管理器，`systemctl` 是其核心命令：

```bash
systemctl start 服务名       # 启动服务
systemctl stop 服务名        # 关闭服务
systemctl status 服务名      # 查看当前服务状态
```

> [!example] 示例
> ```bash
> systemctl status sshd      # 查看 SSH 服务状态
> systemctl start sshd       # 启动 SSH 服务
> ```

## 相关笔记

- [[计算机相关技术知识/01-Linux/Linux 基础命令\|Linux 基础命令]] — reboot、uname 等系统命令
- [[计算机相关技术知识/04-速查表/Linux 命令速查\|Linux 命令速查]] — 命令汇总
