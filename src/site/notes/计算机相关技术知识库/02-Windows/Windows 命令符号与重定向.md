---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/02-Windows/Windows 命令符号与重定向/","title":"Windows 命令符号与重定向","tags":["Windows/命令"],"dg-note-properties":{"title":"Windows 命令符号与重定向","created":"2026-10-13","tags":["Windows/命令"],"aliases":["CMD 特殊符号","Windows 重定向"]}}
---


# Windows 命令符号与重定向

CMD 中的特殊符号可以让多条命令组合、串联，是命令行进阶的核心。

## 顺序执行类

| 符号 | 含义 | 行为 |
|------|------|------|
| `&` | 顺序执行 | 顺序执行多条命令，**不管**命令是否执行成功 |
| `&&` | 成功才继续 | 顺序执行，碰到**执行出错**的命令后**不再执行**后面的命令 |
| `\|\|` | 失败才继续 | 顺序执行，碰到**执行正确**的命令后**不再执行**后面的命令 |

```bash
mkdir dir & cd dir              # 无论 mkdir 成功与否，都执行 cd
mkdir dir1 && cd dir && echo 123  # mkdir1 成功才 cd，cd 成功才 echo
mkdir dir || cd dir             # mkdir 成功就不执行 cd；mkdir 失败才执行 cd
```

## 管道符（|）

```bash
netstat -ano | findstr 123      # 将上一条命令的输出当做下一条命令的输入
```

> [!tip] 管道是过滤器
> `|` 把左边命令的输出"流"给右边命令处理，是最常用的组合方式。

## 输出重定向

| 符号 | 含义 | 行为 |
|------|------|------|
| `>` | 覆盖重定向 | 将当前命令输出以**覆盖**方式写入（原有内容会被清空） |
| `>>` | 追加重定向 | 将当前命令输出以**追加**方式写入（原有内容保留，加一行新的） |
| `<` | 输入重定向 | 从文件中获得**输入**信息 |

```bash
tasklist > tl.txt               # 输出覆盖写入 tl.txt
tasklist >> tl.txt              # 输出追加到 tl.txt

# 创建文件的两种姿势
type nul > a.txt                # nul = Windows 的空内容占位符
echo 我是内容 > a.txt           # 写入一行内容

echo 追加一行内容 >> a.txt      # 追加一行

# 输入重定向
echo 22/1/1 > date.txt
date < date.txt                 # 从 date.txt 读取日期（需管理员权限）
```

> [!warning] `>` 会清空原文件
> `>` 是覆盖式写入，误用会丢失原文件内容。不确定时用 `>>` 追加。

## 组合示例

```bash
# 查找占用某端口的进程并写入报告
netstat -ano | findstr 3306 > report.txt

# 依次检查多个服务端口
netstat -ano | findstr 80 && netstat -ano | findstr 443
```

## 相关笔记

- [[计算机相关技术知识库/02-Windows/Windows findstr 文本搜索\|Windows findstr 文本搜索]] — findstr 参数详解
- [[计算机相关技术知识库/02-Windows/Windows 网络命令\|Windows 网络命令]] — netstat 详解
- [[计算机相关技术知识库/01-Linux/Linux 进程与服务管理\|Linux 进程与服务管理]] — 对比学习：Linux 的管道与重定向类似
- [[计算机相关技术知识库/04-速查表/Windows 命令速查\|Windows 命令速查]] — 特殊符号一页速查
