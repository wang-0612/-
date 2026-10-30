---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/02-Windows/Windows 网络命令/","title":"Windows 网络命令","tags":["Windows/网络"],"dg-note-properties":{"title":"Windows 网络命令","created":"2026-10-13","tags":["Windows/网络"],"aliases":["ipconfig","netstat","ping 命令"]}}
---


# Windows 网络命令

## ipconfig：查看 IP 信息

```bash
ipconfig            # 查看 IP 信息
ipconfig /all       # 查看详细 IP 信息（含 MAC、DNS、DHCP 等）
```

> [!example] 查看本机 IP
> 在 `ipconfig` 输出中找到**正在使用的适配器**（有线为「以太网」，无线为「WLAN」），看「IPv4 地址」一行，例如 `192.168.31.141`。
>
> - 有多个适配器时，显示「媒体状态 . . . : 媒体已断开连接」的不是当前在用的，跳过即可。
> - `ipconfig /all` 还能查看 **MAC 地址**（物理地址）、**DNS 服务器**、**DHCP** 是否启用等详细信息。
>
> ```bash
> ipconfig | findstr IPv4      # 只筛选 IPv4 地址行
> ```

## ping：测试连通性

```bash
ping ip             # 测试与目标 IP/域名的连通性
```

> [!tip] 常见用法
> ```bash
> ping 127.0.0.1     # ping 本机，检测 TCP/IP 协议栈是否正常
> ping www.baidu.com  # 测试外网连通性
> ping -t 10.0.0.1   # 持续 ping，按 Ctrl+C 停止
> ```

## netstat：查看网络连接

```bash
netstat             # 查看网络连接情况
netstat -a          # 查看开启了哪些端口
netstat -ao         # 查看开启了哪些端口，并显示进程 PID
netstat -n          # 查看端口的网络连接情况（以数字形式显示）
netstat -p tcp      # 查看 TCP 协议的使用情况
```

> [!example] 只看监听中的端口
> ```bash
> netstat -ano | findstr LISTENING
> ```
> `LISTENING` 表示该端口正在监听、等待连接；`-ano` 同时显示地址、端口和对应的进程 PID。

### netstat 参数详解

| 参数 | 全称 | 作用 |
|------|------|------|
| `-a` | all | 显示所有连接和**监听端口** |
| `-n` | numeric | 以数字形式显示 IP/端口，**不解析域名**（加速输出） |
| `-o` | owners | 显示每个连接对应的**进程 PID**（排查占用端口的核心参数） |
| `-p` | protocol | 按协议过滤（`-p tcp` / `-p udp`） |
| `-r` | route | 显示路由表 |
| `-s` | statistics | 显示各协议的统计信息（TCP/UDP/ICMP 等） |

> [!example] 实战：排查端口被谁占用
> ```bash
> netstat -ano | findstr 8080     # 找出占用 8080 端口的进程 PID
> tasklist | findstr <PID>        # 查看该 PID 是什么程序
> taskkill /f /pid <PID>          # 结束它
> ```
> 这是开发中最常用的端口排查套路。

## 相关笔记

- [[计算机相关技术知识库/02-Windows/Windows 基础命令\|Windows 基础命令]] — 起点：CMD 常用基础命令
- [[计算机相关技术知识库/02-Windows/Windows 进程管理\|Windows 进程管理]] — tasklist / taskkill
- [[计算机相关技术知识库/02-Windows/Windows 命令符号与重定向\|Windows 命令符号与重定向]] — 管道符 `|` 的用法
- [[计算机相关技术知识库/03-数据库/数据库基础概念\|数据库基础概念]] — 各类数据库默认端口（3306/1433/1521/27017）
- [[计算机相关技术知识库/04-速查表/Windows 命令速查\|Windows 命令速查]] — 网络命令一页速查
- [[计算机相关技术知识库/04-速查表/网络基础速查\|网络基础速查]] — 网络基础与端口速查
