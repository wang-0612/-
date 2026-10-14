---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/04-速查表/Windows 命令速查/","title":"Windows 命令速查","tags":["速查","Windows/命令"],"dg-note-properties":{"title":"Windows 命令速查","created":"2026-10-13","tags":["速查","Windows/命令"],"aliases":["CMD cheatsheet","Windows cheatsheet"]}}
---


# Windows 命令速查

一页汇总 Windows CMD 常用命令。详细讲解见对应笔记链接。

## 基础命令

| 命令 | 作用 |
|------|------|
| `cls` | 清屏 |
| `whoami` | 当前用户 |
| `net user` | 查看计算机上的用户 |
| `systeminfo` | 计算机信息 |
| `ver` | 系统版本号 |
| `hostname` | 主机名 |
| `date` | 日期 |
| `echo 内容` | 输出内容 |
| `chcp 65001` | 切换到 UTF-8 编码 |

## 目录操作

| 命令 | 作用 |
|------|------|
| `dir` | 查看目录 |
| `tree` | 目录结构树 |
| `mkdir / md dirname` | 创建目录 |
| `cd dirname` | 进入目录 |
| `cd ..` | 回上级（`cd ..\..` 回两级） |
| `rd dirname` | 删除空目录 |
| `rd /s dirname` | 删除目录（需确认） |
| `rd /s /q dirname` | 删除目录（不确认，危险） |

## 文件操作

| 命令 | 作用 |
|------|------|
| `type a.txt` | 显示文件内容 |
| `copy a.txt aa.txt` | 复制文件 |
| `xcopy /E olddir newdir` | 复制文件夹 |
| `move oldname newname` | 移动/重命名 |
| `del / erase a.txt` | 删除文件 |

## 文本搜索（findstr）

| 命令 | 作用 |
|------|------|
| `findstr "关键词" 1.txt` | 搜索（多词 = 或） |
| `findstr /c:"完整短语" 1.txt` | 按完整字符串搜索 |
| `findstr /i "hello" 1.txt` | 不分大小写 |
| `findstr /s /i "test" .` | 递归搜索子目录 |
| `findstr /n "hello" 1.txt` | 显示行号 |
| `findstr /v "error" 1.txt` | 反向：不含关键词的行 |
| `findstr /w "and" 1.txt` | 只匹配完整单词 |

## 进程操作

| 命令 | 作用 |
|------|------|
| `tasklist` | 显示进程 |
| `tasklist \| findstr 进程名` | 过滤进程 |
| `taskkill /im name.exe` | 按进程名结束 |
| `taskkill /f /pid PID` | 按 PID 强制结束 |
| `taskkill /f /t /pid PID` | 连子进程一起杀 |

## 网络命令

| 命令 | 作用 |
|------|------|
| `ipconfig` / `ipconfig /all` | IP 信息 / 详细信息 |
| `ping ip` | 测试连通性 |
| `netstat -a` | 所有连接和监听端口 |
| `netstat -n` | 数字形式显示（不解析域名） |
| `netstat -o` | 显示对应进程 PID |
| `netstat -ano \| findstr 8080` | 排查端口占用 |

## 特殊符号

| 符号 | 含义 |
|------|------|
| `&` | 顺序执行（不管成败） |
| `&&` | 前一条成功才执行 |
| `\|\|` | 前一条失败才执行 |
| `\|` | 管道：输出作为下一条的输入 |
| `>` | 覆盖重定向（清空原内容） |
| `>>` | 追加重定向 |
| `<` | 输入重定向 |

## 相关笔记

- [[计算机相关技术知识库/02-Windows/Windows 概述\|Windows 概述]] · [[计算机相关技术知识库/02-Windows/Windows 基础命令\|Windows 基础命令]] · [[计算机相关技术知识库/02-Windows/Windows 目录与文件操作\|Windows 目录与文件操作]]
- [[计算机相关技术知识库/02-Windows/Windows findstr 文本搜索\|Windows findstr 文本搜索]] · [[计算机相关技术知识库/02-Windows/Windows 进程管理\|Windows 进程管理]] · [[计算机相关技术知识库/02-Windows/Windows 网络命令\|Windows 网络命令]]
- [[计算机相关技术知识库/02-Windows/Windows 命令符号与重定向\|Windows 命令符号与重定向]]
