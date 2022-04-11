---
title: MySQL常用语句
urlname: mzaqet
date: '2022-04-11 02:46:44 +0000'
tags:
  - php
  - json
  - mysql
categories:
  - 学无止境
  - - Msql
copyright_author_href: 'https://www.xiaohuihui.net'
cover: >-
  https://www.sxkawzp.cn/upload/2020/2/mysql1-12a849f2afe54669a6cc3259e9d380ac.jpg
---

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649645235333-50c0c21c-f619-40d2-a2cc-04e39d484716.png#clientId=u78daabba-49db-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=503&id=u9de5a5e2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=555&originWidth=900&originalType=binary∶=1&rotation=0&showTitle=false&size=116531&status=done&style=none&taskId=uff38ef90-5ac3-4ff7-ad55-dfe5c18811b&title=&width=815.0943689560213)

## 常用的语句

### 查询

> select \* from bbs where id=1;

### 增加

> insert into bbs (name,data_year) values ("jack","1993-10-01");

### 修改

> update bbs set name="tom",sex=1,age=18 where name="jack";

### 删除

> delete form bbs where id=2;

## 更多语句

### 进入数据库：

```sql
mysql -u root -p
mysql -h localhost -u root -p database_name
```

### 列出数据库：

```sql
show databases;
```

### 选择数据库：

```sql
use databases_name;
```

### 列出数据表：

```sql
show tables;
```

### 显示表格列的属性：

```sql
show columns from table_name;
describe table_name;
```

### 导出整个数据库：

```sql
mysqldump -u user_name -p database_name > /tmp/file_name
```

> 例如：mysqldump -u root -p test_db > d:/test_db.sql

### 导出一个表：

```sql
mysqldump -u user_name -p database_name table_name > /tmp/file_name
```

> 例如：mysqldump -u root -p test_db table1 > d:/table1.sql

### 导出一个数据库结构：

```sql
mysqldump -u user_name -p -d --add--table database_name > file_name
```

> 例如：mysqldump -u root -p -d --add-drop-table test_db > test_db.sql

### 导入数据库：

```sql
source file_name;
# 或者
mysql -u user_name -p database_name < file_name
```

> 例如：
> source /tmp/bbs.sql；
> source d:/bbs.sql；
> mysql -u root -p bbs < "d:/bbs.sql"
> mysql -u root -p bbs < "/tmp/bbs.sql"

### 将文本文件导入数据表中（excel 与之相同）

```sql
load data infile "tables.txt" into table table_name;
```

> 例如：
> load data infile "/tmp/bbs.txt" into table bbs；
> load data infile "/tmp/bbs.xls" into table bbs；
> load data infile "d:/bbs.txt" into table bbs；
> load data infile "d:/bbs.xls" into table bbs；

### 将数据表导出为文本文件（excel 与之相同）

```sql
select * into outfile "path_file_name" from table_name;
```

> 例如：
> select _ into outfile "/tmp/bbs.txt" from bbs；
> select _ into outfile "/tmp/bbs.xls" from bbs where id=1;
> select _ into outfile "d:/bbs.txt" from bbs;
> select _ into outfile "d:/bbs.xls" from bbs where id=1;

### 创建数据库时先判断数据库是否存在：

```sql
create database if not exists database_name;
```

> 例如：create database if not exists bbs

### 创建数据库：

```sql
create database database_name;
```

> 例如：create database bbs;

### 删除数据库：

```sql
drop database database_name;
```

> 例如：drop database bbs;

### 创建数据表：

```sql
mysql> create table <table_name> ( <column 1 name> <col. 1 type> <col. 1 details>,<column 2 name> <col. 2 type> <col. 2 details>, ...);
```

> 例如：create table (id int not null auto_increment primary key,name char(16) not null default "jack",date_year date not null);

### 删除数据表中数据：

```sql
delete from table_name;
```

> 例如：
> delete from bbs;
> delete from bbs where id=2;

### 删除数据库中的数据表：

```sql
drop table table_name;
```

> 例如：
> drop table test_db;
> rm -f database_name/table_name._ (linux 下）
> 例如：
> rm -rf bbs/accp._

### 向数据库中添加数据：

```sql
insert into table_name set column_name1=value1,column_name2=value2;
```

> 例如：insert into bbs set name="jack",date_year="1993-10-01";

```sql
insert into table_name values (column1,column2,...);
```

> 例如：insert into bbs ("2","jack","1993-10-02")

```sql
insert into table_name (column_name1,column_name2,...) values (value1,value2);
```

> 例如：insert into bbs (name,data_year) values ("jack","1993-10-01");

### 查询数据表中的数据：

```sql
select * from table_name;
```

> 例如：select \* from bbs where id=1;

### 修改数据表中的数据：

```sql
update table_name set col_name=new_value where id=1;
```

> 例如：update bbs set name="tom",age=18 where name="jack";

### 增加一个字段：

```sql
alter table table_name add column field_name datatype not null default "1";
```

> 例如：alter table bbs add column tel char(16) not null;

### 增加多个字段：(column 可省略不写）

```sql
alter table table_name add column filed_name1 datatype,add column filed_name2 datatype;
```

> 例如：alter table bbs add column tel char(16) not null,add column address text;

### 删除一个字段：

```sql
alter table table_name drop field_name;
```

> 例如：alter table bbs drop tel;

### 修改字段的数据类型：

```sql
1. alter table table_name modify id int unsigned;
#修改列id的类型为int unsigned
2. alter table table_name change id sid int unsigned;
#修改列id的名字为sid，而且把属性修改为int unsigned
```

### 修改一个字段的默认值：

```sql
alter table table_name modify column_name datatype not null default "";
```

> 例如：alter table test_db modify name char(16) default not null "yourname";

### 对表重新命名：

```sql
alter table table_name rename as new_table_name;
```

> 例如：alter table bbs rename as bbs_table;

```sql
rename table old_table_name to new_table_name;
```

> 例如：rename table test_db to accp;

### 从已经有的表中复制表的结构：

```sql
create table table2 select * from table1 where 1<>1;
```

> 例如：create table test_db select \* from accp where 1<>1;

### 查询时间：

```sql
select now();
```

### 查询当前用户：

```sql
select user();
```

### 查询数据库版本：

```
select version();
```

### 创建索引：

```sql
alter table table1 add index ind_id(id);
create index ind_id on table1(id);
create unique index ind_id on table1(id);//建立唯一性索引
```

### 删除索引：

```sql
drop index idx_id on table1;
alter table table1 drop index ind_id;
```

### 联合字符或者多个列（将 id 与":"和列 name 和"="连接）

```sql
select concat(id，':',name,'=') from table;
```

### limit（选出 10 到 20 条）

```sql
select * from bbs order by id limit 9,10;
```

> （从查询结果中列出第几到几条的记录）

### 增加一个管理员账号：

```sql
grant all on *.* to user@localhost identified by "password";
```

### 创建表是先判断表是否存在

```sql
create table if not exists students(……);
```

### 复制表：

```sql
create table table2 select * from table1;
```

> 例如：create table test_db select \* from accp;

### 授于用户远程访问 mysql 的权限

```sql
grant all privileges on *.* to "root"@"%" identified by "password" with grant option;
# 或者是修改mysql数据库中的user表中的host字段
use mysql;
select user,host from user;
update user set host="%" where user="user_name";
```

### 查看当前状态

```sql
show status;
```

### 查看当前连接的用户

```sql
show processlist;
```

> （如果是 root 用户，则查看全部的线程，得到的用户连接数同 show status;里的 Threads_connected 值是相同的）!
