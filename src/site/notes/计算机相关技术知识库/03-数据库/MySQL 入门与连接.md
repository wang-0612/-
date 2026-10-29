---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/03-数据库/MySQL 入门与连接/","title":"MySQL 入门与连接","tags":["数据库/MySQL"],"dg-note-properties":{"title":"MySQL 入门与连接","created":"2026-10-13","tags":["数据库/MySQL"],"aliases":["MySQL 简介","MySQL 登录"]}}
---


# MySQL 入门与连接

## SQL 是什么

**SQL**（Structured Query Language，结构化查询语言）是用于管理**关系数据库管理系统**（RDBMS）的**标准编程语言**。

SQL 用于执行各种操作，包括：

- 查询、插入、更新和删除数据
- 创建和修改数据库模式

SQL 语句通常由数据库管理系统（DBMS）执行，常见的 DBMS 包括 **Oracle、MySQL、SQL Server、PostgreSQL** 等。

## MySQL 简介

**MySQL** 由瑞典的 **MySQL AB** 公司开发，后来被 **Sun Microsystems** 收购，最终被 **Oracle** 公司收购，成为 Oracle 旗下产品。

MySQL 是最流行的**关系型数据库管理系统**之一，尤其是在 **WEB 应用**方面。

> [!info] 历史沿革
> MySQL AB（开发）→ Sun Microsystems（收购）→ Oracle（收购 Sun，2010）→ 现为 Oracle 旗下产品

## 连接 MySQL（命令行）

> [!tip] 练习环境：phpstudy
> 使用 **PHPStudy** 工具进行 MySQL 的练习：
> 1. 找到 phpstudy 目录，进入 `C:\phpstudy\Extensions\MySQL5.7.26\bin`
> 2. 在该目录下 **Shift + 鼠标右键** → 「在此处打开命令窗口」
> 3. 输入 `mysql -u 用户名 -p密码` 登录

### 登录命令

```bash
mysql -u root -p                                    # 登录本地数据库（推荐的输入方式）
mysql -u root -p -h 127.0.0.1 -P 3306               # 登录指定 IP、端口的数据库（可用于远程登录）
```

| 参数 | 含义 |
|------|------|
| `-u` | 指定用户名（如 root） |
| `-p` | 后接密码；**建议只写 `-p` 回车后再输入密码**，避免明文留在命令记录里 |
| `-h` | 指定主机地址（默认 localhost） |
| `-P` | 指定端口（默认 3306，P 为大写） |

> [!warning] 密码输入安全
> `mysql -u root -p123456` 这种写法会把密码明文留在 shell 历史中。规范做法是：
> ```bash
> mysql -u root -p
> Enter password:            # 输入密码时不显示，正常
> ```

登录成功后出现 `mysql>` 提示符，即可执行 SQL 语句（以 `;` 或 `\g` 结尾）。

## 相关笔记

- [[计算机相关技术知识库/03-数据库/数据库基础概念\|数据库基础概念]] — 起点：库 / 表 / 字段 / 记录与 SQL
- [[计算机相关技术知识库/03-数据库/MySQL 数据库与表操作\|MySQL 数据库与表操作]] — 库与表的管理
- [[计算机相关技术知识库/03-数据库/MySQL 增删改查\|MySQL 增删改查]] — 数据操作
- [[计算机相关技术知识库/03-数据库/常见数据库类型\|常见数据库类型]] — 其他数据库对比
- [[计算机相关技术知识库/04-速查表/SQL 速查\|SQL 速查]] — 连接与常用语句一页速查
