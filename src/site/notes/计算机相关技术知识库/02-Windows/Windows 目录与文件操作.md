---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/02-Windows/Windows 目录与文件操作/","title":"Windows 目录与文件操作","tags":["Windows/命令","Windows/文件"],"dg-note-properties":{"title":"Windows 目录与文件操作","created":"2026-10-13","tags":["Windows/命令","Windows/文件"],"aliases":["Windows 目录操作","Windows 文件操作"]}}
---


# Windows 目录与文件操作

## 目录操作

### 查看目录

```bash
dir             # 查看目录内容
tree            # 以树形显示目录结构
```

### 创建目录

```bash
mkdir dirname   # 创建目录
md dirname      # 创建目录（mkdir 的缩写）
```

### 切换目录

```bash
cd dirname          # 进入 dirname 目录
cd ..               # 回到上级目录
cd ..\..            # 一次性跳回上两级目录（多级回退）
cd /d D:\some\path  # 跨盘符切换（需加 /d）
```

### 删除目录

```bash
rd dirname          # 只能删除空目录，无需确认
rd /s dirname       # 删除目录（含内容），需要确认（/s = subdirectory，递归删除所有子目录和文件）
rd /s /q dirname    # 删除目录，无需确认（/q = quiet 安静模式）
```

> [!warning] rd /s /q
> 相当于 Linux 的 `rm -rf`，**不可恢复**，执行前务必确认路径。

## 文件操作

### 查看文件

```bash
type a.txt      # 显示文件所有内容
```

### 复制

```bash
copy a.txt aa.txt           # 复制文件
xcopy /E olddir newdir      # 复制文件夹（/E 会连同子目录一起复制，含空目录）
```

### 移动 / 重命名

```bash
move oldname newname        # 移动或重命名文件/文件夹
```

### 删除

```bash
del a.txt           # 删除文件，无需确认
erase a.txt         # 删除文件，无需确认（del 的同义词）
```

> [!note] del 与 rd
> `del` 删除**文件**，`rd` 删除**目录**——Windows 不允许用 `del` 直接删目录。

## 相关笔记

- [[计算机相关技术知识库/02-Windows/Windows findstr 文本搜索\|Windows findstr 文本搜索]] — 在文件中搜索字符串
- [[计算机相关技术知识库/01-Linux/Linux 文件与目录操作\|Linux 文件与目录操作]] — 对比学习：Linux 版本的文件操作
