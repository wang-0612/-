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

```sql
-- 列属性必须写在 CREATE TABLE 的列定义里，不能单独执行
id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY
```

- `auto_increment`：定义列为**自增**属性，数值自动加 1
- `unsigned`：无符号（非负数）
- 一般用于**主键**（id）

> [!note] 不能单独执行
> `UNSIGNED` 和 `AUTO_INCREMENT` 是**列属性**，必须跟在列名和数据类型之后、写在 `CREATE TABLE` / `ALTER TABLE` 的列定义中；单独写 `unsigned auto_increment;` 会报语法错误。

> [!example] 典型主键定义
> ```sql
> create table users(
>     id int unsigned auto_increment primary key,
>     name varchar(50),
>     age int
> );
> ```

## 实战：jingsai 库与 yuangong 表

从建库到查询走一遍完整流程：

```sql
-- 1. 创建数据库并进入
create database jingsai;
use jingsai;

-- 2. 创建员工表
create table yuangong (
    id int primary key auto_increment,
    name varchar(20),
    age int,
    job varchar(20),
    salary int
);

-- 3. 插入三条记录（id 自增，插入时省略）
insert into yuangong (name, age, job, salary) values
    ('张三', 22, '程序员', 8500),
    ('李四', 25, '产品经理', 5000),
    ('王五', 20, '实习生', 4200);

-- 4. 查询全部记录
select * from yuangong;

-- 5. 查看表结构
desc yuangong;
```

执行 `select * from yuangong;` 后应看到：

| id | name | age | job | salary |
|----|------|-----|-----|--------|
| 1 | 张三 | 22 | 程序员 | 8500 |
| 2 | 李四 | 25 | 产品经理 | 5000 |
| 3 | 王五 | 20 | 实习生 | 4200 |

### 语句说明

- **`id int primary key auto_increment`**：`id` 是主键且自增，插入时**省略不写**，数据库自动从 1 开始依次赋值。因此 `insert` 的列清单里只有 `name, age, job, salary` 四列。
- **`varchar(20)`**：可变长字符串，括号里的 20 表示**最多 20 个字符**（MySQL 5.0.3 起按字符计，不是字节），姓名、职位这类短文本足够用。
- **`desc yuangong;`**：`desc` 是 describe 的缩写，用于**查看表结构**——字段名、类型、是否允许 NULL、键、默认值、Extra（自增会显示在这里）。
- **`select * from yuangong;`**：`*` 代表所有列，即**查询整张表**的全部数据。

> [!tip] 字符串必须加单引号
> `'张三'`、`'程序员'` 是字符串，必须用**单引号**包裹；数字（22、8500）直接写。漏掉引号时 MySQL 会把中文当成列名而报错。

> [!note] 多行插入
> 一条 `insert` 后面可以跟多组 `(...)`，用逗号分隔，一次插入多行，比逐条执行效率更高。

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
- [[计算机相关技术知识库/04-速查表/SQL 速查\|SQL 速查]] — 建库建表语句一页速查
