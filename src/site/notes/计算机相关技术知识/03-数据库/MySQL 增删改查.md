---
{"dg-publish":true,"permalink":"/计算机相关技术知识/03-数据库/MySQL 增删改查/","title":"MySQL 增删改查","tags":["数据库/MySQL","SQL/CRUD"],"dg-note-properties":{"title":"MySQL 增删改查","created":"2026-10-13","tags":["数据库/MySQL","SQL/CRUD"],"aliases":["MySQL CRUD","SQL 增删改查"]}}
---


# MySQL 增删改查

CRUD = **C**reate（增）· **R**ead（查）· **U**pdate（改）· **D**elete（删），是数据库操作的四大基本功。

## 增：插入数据（INSERT）

```bash
insert into 表名 values(值1, 值2, ......);                       # 插入整行数据（按列顺序）
insert into table_name (列1, 列2,...) values(值1, 值2, ...);     # 插入一条语句（指定列）
insert into table_name (列1, 列2,...) values(值1, ...),(值1, ...); # 同时插入多条语句
```

> [!tip] 推荐写法
> 指定列名的写法更安全：即使表结构变化（加列），旧的插入语句依然有效。

## 查：查询数据（SELECT）

```bash
select * from 表名;                     # 查询表中所有内容
select name from users;                 # 查询 users 表中 name 字段的值
select * from users where age < 30;     # where 条件查询
select * from users limit 1,2;          # limit 分页
```

### where 子句：指定条件

可用运算符：`=` `>` `<` `!=` `>=` `<=`，并可用 `and`、`or` 指定多个条件。

```bash
select * from users where age >= 18 and age < 30;
select * from users where name = '张三' or age = 20;
```

### limit 语句：分页

```bash
select * from users limit 1,2;
```

- 第一个参数：**开始位置**（偏移量，从 0 开始）
- 第二个参数：**个数**

> [!example] limit 理解
> `limit 1,2` = 跳过第 1 条，从第 2 条开始取 2 条 → 返回第 2、3 条记录。

## 改：更新数据（UPDATE）

```bash
update 表名 set 字段 = 新值 where 条件;
```

> [!danger] 不要忘记 where
> `update users set age = 18;` 会把**所有记录**的 age 都改成 18！
> 执行 update 前先用同样的 where 条件跑一遍 select 确认范围。

## 删：删除数据（DELETE）

```bash
delete from 表名 where 条件;    # 删除指定条件的语句
delete from 表名;               # 删除所有数据（保留表结构）
```

> [!danger] 同样必须带 where
> 不带 where 的 `delete from 表名` 会清空整张表。

## union 联合查询

使用 `union` 联合查询语句，把多个 `select` 的结果合并为一个结果集：

```bash
select 字段 from 表1 union select 字段 from 表2;
```

> [!note] union 使用要点
> - 前后两个 select 的**列数必须相同**
> - 结果集的列名由**第一个 select** 决定
> - `union` 默认去重；想保留重复行用 `union all`

## 相关笔记

- [[计算机相关技术知识/03-数据库/MySQL 数据库与表操作\|MySQL 数据库与表操作]] — 建库建表
- [[计算机相关技术知识/03-数据库/SQL 注入常用命令\|SQL 注入常用命令]] — 非授权环境下针对 SQL 的攻击面（仅限授权测试）
