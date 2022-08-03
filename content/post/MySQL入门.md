---
title: "MySQL入门"
date: 2022-08-06T11:49:50+08:00
draft: false
tags:
- MySQL
---

## 基本概念

- 数据库(Database, DB): 将大量数据保存起来, 通过计算机加工而成的可以进行高效访问的数据集合.
- 数据库管理系统(Database Management System, DBMS): 管理数据库的计算机系统.
- 表(Table): 包含数据库中所有数据的数据库对象, 定义为列的集合. 与电子表格相似, 数据在表中式按行和列的格式组织排列的.
- 列(Column): 又称字段(Field), 存储某种类型的信息. 每列只能指定一种数据类型.
- 行: 又称记录或元组, 一组相关的数据.
- 主键: 记录的唯一标识, 通常指定一个字段担任, 但也可以通过复合不同字段构成唯一标识, 此时称为复合键.
- 外键: 

## 创建

- 数据库: CREATE DATABASE dbname;
- 表: CREATE TABLE table_name;
- 字段: 创建/新增字段时, 也要对数据类型和值的限制进行设置.
    - 数据类型
        - 数值
            - TINYINT
            - SMALLINT
            - MEDIUMINT
            - INT / INTEGER
            - BIGINT
            - FLOAT
            - DOUBLE
            - DECIMAL
        - 日期
            - DATE
            - TIME
            - YEAR
            - DATETIME
            - TIMESTAMP
        - 字符
            - CHAR
            - VARCHAR
            - TINYBLOB
            - TINYTEXT
            - BLOB
            - TEXT
            - MEDIUMBLOB
            - MEDIUMTEXT
            - LONGBLOB
            - LONGTEXT
        - 特殊值
            - NULL  
            注意: 无法比较 NULL和0, 它们是不等价的. 判断NULL需要用到IS (NOT) NULL
    - 值的限制
        - NOT NULL
        - UNIQUE 
        - PRIMARY KEY:  NOT NULL 和 UNIQUE 的结合.
        - FOREIGN KEY
        - CHECK
        - DEFAULT 
        - AUTO_INCREMENT(=100)
    - 在创建表时即指定字段  
    CREATE TABLE table_name ( column_name data_type(size) constraint_name, ...);
    - 对已存在表插入新字段  
    ALTER TABLE testalter_tbl ADD i INT; 
- 记录: INSERT INTO table_name (field, ...) VALUES (value, ...), (value, ...), ...;

## 查询与选择

- 通用语法
SELECT column_name, ...
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
- 字段选取
    - 指定字段: 打出字段名, 逗号分隔.
    - 全选: 星号(*).
- 筛选条件WHERE  
在初始表中筛选查询。它是一个约束声明，用于约束数据，在返回结果集之前起作用。
    - 比较操作符: =, <> 或 !=, >, <, >=, <=  
    特别地, MySQL 的 WHERE 子句的字符串比较是不区分大小写的. 你可以使用 BINARY 关键字来设定 WHERE 子句的字符串比较是区分大小写的.
    - 字符串匹配LIKE
        - %: 任意数量字符. 中文请使用两个百分号(%%)表示.
        - _: 任意单个字符.
        - []: 括号内所列字符中的一个. 或指定一个字符、字符串或范围，要求所匹配对象为它们中的任一个.
        - \[^\]: 表示不在括号所列之内的单个字符.
        - 特别地, 如要查询以上转义字符, 需要[]括起.
        - 正则模式REGEXP 
- SELECT结果集组合: UNION [ALL | DISTINCT]  
    备注: 默认为DISTINCT
    请注意, UNION 内部的每个 SELECT 语句必须拥有相同数量的列. 列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同.
- SELECT结果集分组: GROUP BY column_name (WITH ROLLUP)
- 多表查询JOIN  
    JOIN 子句用于把来自两个或多个表的行结合起来, 基于这些表之间的共同字段.

## 修改

- 记录: UPDATE table_name SET field=new-value, ... [WHERE Clause]
- 字段 
    - 类型及名称
        - ALTER TABLE testalter_tbl MODIFY c CHAR(10);
        - ALTER TABLE testalter_tbl CHANGE old_name new_name BIGINT;
    - 默认值
        - ALTER TABLE testalter_tbl ALTER i SET DEFAULT 1000;
- 表名: ALTER TABLE testalter_tbl RENAME TO alter_tbl;
- 存储引擎: alter table tableName engine=myisam;

## 删除

- 数据库: DROP DATABASE database_name;
- 表: DROP TABLE table_name;
- 索引: DROP INDEX table_name.index_name;
- 表内数据: TRUNCATE TABLE table_name;
- 记录: DELETE FROM table_name [WHERE Clause]
- 字段: ALTER TABLE testalter_tbl DROP i;
- 如果数据表中只剩余一个字段则无法使用DROP来删除字段.
- 外键约束: alter table tableName drop foreign key keyName;


delete，drop，truncate 都有删除表的作用，区别在于：  
1、delete 和 truncate 仅仅删除表数据，drop 连表数据和表结构一起删除，打个比方，delete 是单杀，truncate 是团灭，drop 是把电脑摔了。  
2、delete 是 DML 语句，操作完以后如果没有不想提交事务还可以回滚，truncate 和 drop 是 DDL 语句，操作完马上生效，不能回滚，打个比方，delete 是发微信说分手，后悔还可以撤回，truncate 和 drop 是直接扇耳光说滚，不能反悔。  
3、执行的速度上，drop>truncate>delete，打个比方，drop 是神舟火箭，truncate 是和谐号动车，delete 是自行车。
