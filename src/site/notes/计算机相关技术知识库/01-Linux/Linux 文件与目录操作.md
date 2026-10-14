---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/01-Linux/Linux 文件与目录操作/","title":"Linux 文件与目录操作","tags":["Linux/命令","Linux/文件"],"dg-note-properties":{"title":"Linux 文件与目录操作","created":"2026-10-13","tags":["Linux/命令","Linux/文件"],"aliases":["Linux 文件管理","Linux 目录操作"]}}
---


# Linux 文件与目录操作

## 列出目录（ls）

```bash
ls              # 列出目录内容
```

| 选项 | 作用 |
|------|------|
| `-a` | 全部的文件，连同隐藏文件（开头为 `.` 的文件）一起列出来 |
| `-l` | 长数据串列出，包含文件的属性与权限等数据 |

```bash
ls -a       # 查看所有文件（含隐藏）
ls -l       # 查看详细信息（权限、大小）
ls -al      # 查看所有文件的详细信息（最常用）
```

## 创建目录（mkdir）

```bash
mkdir 目录名称
```

| 选项 | 作用 |
|------|------|
| `-m` | 创建时直接设置权限 |
| `-p` | 递归创建多级目录（上级不存在也能建） |

```bash
mkdir test1         # 创建单个目录 test1
mkdir -p a/b/c      # 递归创建 a/b/c 三层目录
```

## 删除空目录（rmdir）

```bash
rmdir 目录名称
```

| 选项 | 作用 |
|------|------|
| `-p` | 从该目录起，一次删除多级空目录 |

```bash
rmdir test1         # 删除空目录 test1
rmdir -p a/b/c      # 递归删除空目录 a/b/c
```

## 复制文件或目录（cp）

```bash
cp 来源档(source) 目标档(destination)
```

| 选项 | 作用 |
|------|------|
| `-r` | 递归持续复制，用于目录的复制 |
| `-a` | 复制并保留属性 |

```bash
cp 1.txt 2.txt      # 复制文件 1.txt 到 2.txt
cp -r dir1 dir2     # 复制目录 dir1 到 dir2（必须加 -r）
cp -a 1.txt 3.txt   # 复制并保留属性
```

## 删除文件或目录（rm）

```bash
rm 文件或目录
```

| 选项 | 作用 |
|------|------|
| `-f` | force，忽略不存在的文件，不会出现警告 |
| `-i` | 互动模式，删除前询问是否动作 |
| `-r` | 递归删除，常用于删除目录，**非常危险** |

```bash
rm 1.txt            # 删除文件 1.txt
rm -f 2.txt         # 强制删除文件，不提示
rm -r dir1          # 删除整个目录及里面所有内容（最常用）
rm -rf dir2         # 强制删除目录，不提示
```

> [!danger] 危险操作
> `rm -rf` 是 Linux 最危险的命令之一，执行后**无法撤销**。尤其不要在根目录附近使用，如 `rm -rf /*`。

## 移动与重命名（mv）

```bash
mv source destination
```

| 选项 | 作用 |
|------|------|
| `-f` | force 强制，目标文件已存在时直接覆盖 |
| `-i` | 目标文件已存在时，询问是否覆盖 |

```bash
mv 1.txt dir1/      # 移动 1.txt 到 dir1 目录里
mv 1.txt abc.txt    # 将 1.txt 重命名为 abc.txt（移动到同目录 = 重命名）
```

> [!note] mv 的双重身份
> `mv` 既是「移动」也是「重命名」：目标路径是别的目录 → 移动；目标路径是同目录的新名字 → 重命名。

## 创建文件

```bash
touch filename      # 创建空文件（或更新时间戳）
vi filename         # 用 vi 创建/编辑文件
vim filename        # 用 vim 创建/编辑文件（vi 的升级增强版）
```

## 查看文件内容

| 命令 | 作用 |
|------|------|
| `cat` | 由第一行开始显示文件内容 |
| `more` | 一页一页地显示文件内容 |
| `less` | 与 more 类似，但**可以往前翻页** |

```bash
cat abc.txt
less abc.txt
```

## 相关笔记

- [[计算机相关技术知识库/01-Linux/Linux 文件系统目录结构\|Linux 文件系统目录结构]] — 各目录都是干什么的
- [[计算机相关技术知识库/01-Linux/Linux 文件权限\|Linux 文件权限]] — chmod 修改权限
- [[计算机相关技术知识库/01-Linux/Linux vim 文本编辑\|Linux vim 文本编辑]] — vim 三种工作模式
