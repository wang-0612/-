---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/03-数据库/MySQL 数据库与表操作/","title":"MySQL 数据库与表操作","tags":["数据库/MySQL"],"dg-note-properties":{"title":"MySQL 数据库与表操作","created":"2026-10-13","tags":["数据库/MySQL"],"aliases":["MySQL 库操作","MySQL 表操作"]}}
---


# MySQL 数据库与表操作

> [!note]
> 以下语句在 `mysql>` 提示符中执行，**每条语句以分号 `;` 结尾**。

## 数据库操作

```bash
show databases;            # 查看所有数据库名
create database mytest;    # 创建名为 mytest 的数据库
use xx;                    # 进入（切换到）某个数据库
select database();         # 查询当前使用的数据库是哪一个
drop database mytest;      # 删除数据库
```

> [!warning] drop database
> `drop` 是**不可恢复**的删除操作，执行前务必确认数据库名拼写正确。

## 表操作

```bash
create table tablename(
    column_name1 column_type1,
    column_name2 column_type2
);                                        # 创建表

desc 表名;                                # 查看表结构
show create table 表名;                   # 查看创建表的语句（含引擎、字符集）

alter table 表名 add column 列名 列类型;   # 添加列
alter table 表名 drop column 列名;         # 删除指定列

drop table table_name;                    # 删除表
```

## 自增属性

```bash
unsigned auto_increment;
```

- `auto_increment`：定义列为**自增**属性，数值自动加 1
- `unsigned`：无符号（非负数）
- 一般用于**主键**（id）

> [!example] 典型主键定义
> ```sql
> create table users(
>     id int unsigned auto_increment primary key,
>     name varchar(50),
>     age int
> );
> ```

## 操作顺序示意

```mermaid
graph LR
    A[create database] --> B[use 库名]
    B --> C[create table]
    C --> D[insert 插入数据]
    D --> E[select 查询]
    E --> F[update / delete]
    F --> G[drop table / database]
```

## 相关笔记

- [[计算机相关技术知识库/03-数据库/MySQL 入门与连接\|MySQL 入门与连接]] — 登录 MySQL
- [[计算机相关技术知识库/03-数据库/MySQL 增删改查\|MySQL 增删改查]] — 数据的增删改查
