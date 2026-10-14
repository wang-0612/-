---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/02-Windows/Windows findstr 文本搜索/","title":"Windows findstr 文本搜索","tags":["Windows/命令"],"dg-note-properties":{"title":"Windows findstr 文本搜索","created":"2026-10-13","tags":["Windows/命令"],"aliases":["findstr 命令"]}}
---


# Windows findstr 文本搜索

`findstr` 是 Windows 命令行下最常用的**文本搜索**工具，用于在文件中查找字符串。

## 基本用法

```bash
findstr "hello world" 1.txt            # 在 1.txt 中搜索 hello 或 world（空格分隔 = 或的关系）
findstr /c:"hello world" 1.txt         # 按完整字符串搜索 "hello world"，空格不拆分
findstr /s /i "Hello" *.*              # 不区分大小写，在当前目录和所有子目录的所有文件中搜索
```

## 常用参数

| 参数 | 英文含义 | 作用说明 | 示例 |
|------|---------|---------|------|
| `/c:string` | — | 按完整字符串搜索，空格不拆分 | `findstr /c:"hello world" 1.txt` |
| `/i` | ignore | 不区分大小写搜索 | `findstr /i "Hello" 1.txt` |
| `/s` | subdir | 递归搜索当前目录 + 所有子目录 | `findstr /s /i "test" .` |
| `/n` | number | 显示匹配内容所在行号 | `findstr /n "hello" 1.txt` |
| `/v` | reverse | **反向查找**，显示不包含关键词的行 | `findstr /v "error" 1.txt` |
| `/w` | word | 只匹配完整单词，不匹配半截字符 | `findstr /w "and" 1.txt` |

> [!tip] 重点区分
> - 不加 `/c:` 时，`"hello world"` 表示搜索 `hello` **或** `world`
> - 加 `/c:"hello world"` 表示搜索完整短语 `hello world`
> - `/w` 保证精确单词匹配，避免 `android` 命中 `and` 的问题

## 常见组合

```bash
findstr /n /i "error" *.log        # 在所有 log 文件中找 error（带行号、不分大小写）
findstr /v /n "debug" app.log      # 反向查找：显示所有不含 debug 的行（过滤日志）
findstr /s /n "password" C:\conf\* # 递归搜索目录下所有文件
```

## 相关笔记

- [[计算机相关技术知识库/02-Windows/Windows 命令符号与重定向\|Windows 命令符号与重定向]] — 配合管道 `|` 使用效果更佳
- [[计算机相关技术知识库/02-Windows/Windows 基础命令\|Windows 基础命令]] — 其他基础命令
