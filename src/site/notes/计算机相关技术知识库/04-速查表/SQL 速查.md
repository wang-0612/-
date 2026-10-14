---
title: SQL 速查
created: 2026-10-13
tags:
  - 速查
  - 数据库/SQL
aliases:
  - SQL cheatsheet
  - MySQL 速查
dg-publish: true
---

# SQL 速查

一页汇总 MySQL 常用语句。

## 连接

```bash
mysql -u root -p                                # 登录本地
mysql -u root -p -h 127.0.0.1 -P 3306           # 登录远程（-h 主机 -P 端口）
```

## 库操作

| 语句 | 作用 |
|------|------|
| `show databases;` | 查看所有数据库 |
| `create database mytest;` | 创建数据库 |
| `use xx;` | 进入数据库 |
| `select database();` | 当前所在的数据库 |
| `drop database mytest;` | 删除数据库 |

## 表操作

| 语句 | 作用 |
|------|------|
| `create table t(id int auto_increment primary key, name varchar(50));` | 创建表 |
| `desc 表名;` | 查看表结构 |
| `show create table 表名;` | 查看建表语句 |
| `alter table 表名 add column 列名 类型;` | 加列 |
| `alter table 表名 drop column 列名;` | 删列 |
| `drop table 表名;` | 删表 |

## 增

```sql
insert into 表名 values(值1, 值2, ...);
insert into 表名 (列1, 列2, ...) values(值1, 值2, ...);
insert into 表名 (列1, 列2, ...) values(...), (...);    -- 多条
```

## 查

```sql
select * from 表名;                       -- 全部
select 列1, 列2 from 表名;                -- 指定列
select * from 表名 where 条件;             -- 条件（= > < != >= <= and or）
select * from 表名 limit 偏移量, 个数;      -- 分页
select 字段 from 表1 union select 字段 from 表2;  -- 联合查询（去重）
```

## 改

```sql
update 表名 set 字段 = 新值 where 条件;    -- ⚠️ 必须带 where
```

## 删

```sql
delete from 表名 where 条件;              -- 按条件删
delete from 表名;                        -- 清空表（保留结构）⚠️
```

## SQL 注入常用函数

```sql
select database();                      -- 当前库名
select user();                          -- 当前用户
select version();                       -- 版本号
select concat(name, age);               -- 拼接字符串 → 张三18
select group_concat(table_name) ...;    -- 多行合并一行（爆表必用）
select substring('abcdef', 2, 3);       -- 截取 → bcd
select length('admin');                 -- 长度 → 5
```

## 默认端口

| 数据库 | 端口 |
|--------|------|
| MySQL | 3306 |
| SQL Server | 1433 |
| Oracle | 1521 |
| MongoDB | 27017 |

## 相关笔记

- [[MySQL 入门与连接]] · [[MySQL 数据库与表操作]] · [[MySQL 增删改查]]
- [[SQL 注入常用命令]] · [[常见数据库类型]] · [[数据库基础概念]]
