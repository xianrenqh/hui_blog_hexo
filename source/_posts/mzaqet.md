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

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649645235333-50c0c21c-f619-40d2-a2cc-04e39d484716.png)

## <font style="color:rgb(83, 104, 121);">常用的语句</font>

### <font style="color:rgb(83, 104, 121);">查询 </font>

> <font style="color:rgb(83, 104, 121);">select \* from bbs where id=1;</font>

### <font style="color:rgb(83, 104, 121);">增加 </font>

> <font style="color:rgb(83, 104, 121);">insert into bbs (name,data_year) values ("jack","1993-10-01");</font>

### <font style="color:rgb(83, 104, 121);">修改 </font>

> <font style="color:rgb(83, 104, 121);">update bbs set name="tom",sex=1,age=18 where name="jack";</font>

### <font style="color:rgb(83, 104, 121);">删除 </font>

> <font style="color:rgb(83, 104, 121);">delete form bbs where id=2;</font>

<font style="color:rgb(83, 104, 121);"></font>

## <font style="color:rgb(83, 104, 121);">更多语句</font>

### <font style="color:rgb(83, 104, 121);">进入数据库：</font>

```sql
mysql -u root -p
mysql -h localhost -u root -p database_name
```

### <font style="color:rgb(83, 104, 121);">列出数据库：</font>

```sql
show databases;
```

### <font style="color:rgb(83, 104, 121);">选择数据库：</font>

```sql
use databases_name;
```

<font style="color:rgb(83, 104, 121);"></font>

### <font style="color:rgb(83, 104, 121);">列出数据表：</font>

```sql
show tables;
```

### <font style="color:rgb(83, 104, 121);">显示表格列的属性：</font>

```sql
show columns from table_name;
describe table_name;
```

### <font style="color:rgb(83, 104, 121);">导出整个数据库：</font>

```sql
mysqldump -u user_name -p database_name > /tmp/file_name
```

> <font style="color:rgb(83, 104, 121);">例如：mysqldump -u root -p test_db > d:/test_db.sql</font>

### <font style="color:rgb(83, 104, 121);">导出一个表：</font>

```sql
mysqldump -u user_name -p database_name table_name > /tmp/file_name
```

> <font style="color:rgb(83, 104, 121);">例如：mysqldump -u root -p test_db table1 > d:/table1.sql</font>

### <font style="color:rgb(83, 104, 121);">导出一个数据库结构：</font>

```sql
mysqldump -u user_name -p -d --add--table database_name > file_name
```

> <font style="color:rgb(83, 104, 121);">例如：mysqldump -u root -p -d --add-drop-table test_db > test_db.sql</font>

### <font style="color:rgb(83, 104, 121);">导入数据库：</font>

```sql
source file_name;
# 或者
mysql -u user_name -p database_name < file_name
```

> <font style="color:rgb(83, 104, 121);">例如：</font>
>
> <font style="color:rgb(83, 104, 121);">source /tmp/bbs.sql；</font>
>
> <font style="color:rgb(83, 104, 121);">source d:/bbs.sql；</font>
>
> <font style="color:rgb(83, 104, 121);">mysql -u root -p bbs < "d:/bbs.sql"</font>
>
> <font style="color:rgb(83, 104, 121);">mysql -u root -p bbs < "/tmp/bbs.sql"</font>
>
> <font style="color:rgb(83, 104, 121);"></font>

### <font style="color:rgb(83, 104, 121);">将文本文件导入数据表中（excel 与之相同）</font>

```sql
load data infile "tables.txt" into table table_name;
```

> <font style="color:rgb(83, 104, 121);">例如：</font>
>
> <font style="color:rgb(83, 104, 121);">load data infile "/tmp/bbs.txt" into table bbs；</font>
>
> <font style="color:rgb(83, 104, 121);">load data infile "/tmp/bbs.xls" into table bbs；</font>
>
> <font style="color:rgb(83, 104, 121);">load data infile "d:/bbs.txt" into table bbs；</font>
>
> <font style="color:rgb(83, 104, 121);">load data infile "d:/bbs.xls" into table bbs；</font>

### <font style="color:rgb(83, 104, 121);">将数据表导出为文本文件（excel 与之相同）</font>

```sql
select * into outfile "path_file_name" from table_name;
```

> 例如：
>
> <font style="color:rgb(83, 104, 121);">select</font><font style="color:rgb(83, 104, 121);"> \* into outfile "/tmp/bbs.txt" from bbs；</font>
>
> <font style="color:rgb(83, 104, 121);">select \* into outfile "/tmp/bbs.xls" from bbs where id=1;</font>
>
> <font style="color:rgb(83, 104, 121);">select \* into outfile "d:/bbs.txt" from bbs;</font>
>
> <font style="color:rgb(83, 104, 121);">select \* into outfile "d:/bbs.xls" from bbs where id=1;</font>

### <font style="color:rgb(83, 104, 121);">创建数据库时先判断数据库是否存在：</font>

```sql
create database if not exists database_name;
```

> <font style="color:rgb(83, 104, 121);">例如：create database if not exists bbs</font>

### <font style="color:rgb(83, 104, 121);">创建数据库：</font>

```sql
create database database_name;
```

> <font style="color:rgb(83, 104, 121);">例如：create database bbs;</font>

### <font style="color:rgb(83, 104, 121);">删除数据库：</font>

```sql
drop database database_name;
```

> <font style="color:rgb(83, 104, 121);">例如：drop database bbs;</font>

### <font style="color:rgb(83, 104, 121);">创建数据表：</font>

```sql
mysql> create table <table_name> ( <column 1 name> <col. 1 type> <col. 1 details>,<column 2 name> <col. 2 type> <col. 2 details>, ...);
```

> <font style="color:rgb(83, 104, 121);">例如：create table (id int not null auto_increment primary key,name char(16) not null default "jack",date_year date not null);</font>

### <font style="color:rgb(83, 104, 121);">删除数据表中数据：</font>

```sql
delete from table_name;
```

> <font style="color:rgb(83, 104, 121);">例如：</font>
>
> <font style="color:rgb(83, 104, 121);">delete from bbs;</font>
>
> <font style="color:rgb(83, 104, 121);">delete from bbs where id=2;</font>

### <font style="color:rgb(83, 104, 121);">删除数据库中的数据表：</font>

```sql
drop table table_name;
```

> <font style="color:rgb(83, 104, 121);">例如：</font>
>
> <font style="color:rgb(83, 104, 121);">drop table test_db;</font>
>
> <font style="color:rgb(83, 104, 121);">rm -f database_name/table_name.\* (linux 下）</font>
>
> <font style="color:rgb(83, 104, 121);">例如：</font>
>
> <font style="color:rgb(83, 104, 121);">rm -rf bbs/accp.\*</font>

### <font style="color:rgb(83, 104, 121);">向数据库中添加数据：</font>

```sql
insert into table_name set column_name1=value1,column_name2=value2;
```

> <font style="color:rgb(83, 104, 121);">例如：insert into bbs set name="jack",date_year="1993-10-01";</font>

```sql
insert into table_name values (column1,column2,...);
```

> <font style="color:rgb(83, 104, 121);">例如：insert into bbs ("2","jack","1993-10-02")</font>

```sql
insert into table_name (column_name1,column_name2,...) values (value1,value2);
```

> <font style="color:rgb(83, 104, 121);">例如：insert into bbs (name,data_year) values ("jack","1993-10-01");</font>

<font style="color:rgb(83, 104, 121);"></font>

### <font style="color:rgb(83, 104, 121);">查询数据表中的数据：</font>

```sql
select * from table_name;
```

> <font style="color:rgb(83, 104, 121);">例如：select \* from bbs where id=1;</font>

### <font style="color:rgb(83, 104, 121);">修改数据表中的数据：</font>

```sql
update table_name set col_name=new_value where id=1;
```

> <font style="color:rgb(83, 104, 121);">例如：update bbs set name="tom",age=18 where name="jack";</font>

### <font style="color:rgb(83, 104, 121);">增加一个字段：</font>

```sql
alter table table_name add column field_name datatype not null default "1";
```

> <font style="color:rgb(83, 104, 121);">例如：alter table bbs add column tel char(16) not null;</font>

### <font style="color:rgb(83, 104, 121);">增加多个字段：(column 可省略不写）</font>

```sql
alter table table_name add column filed_name1 datatype,add column filed_name2 datatype;
```

> <font style="color:rgb(83, 104, 121);">例如：alter table bbs add column tel char(16) not null,add column address text;</font>

### <font style="color:rgb(83, 104, 121);">删除一个字段：</font>

```sql
alter table table_name drop field_name;
```

> <font style="color:rgb(83, 104, 121);">例如：alter table bbs drop tel;</font>

### <font style="color:rgb(83, 104, 121);">修改字段的数据类型：</font>

```sql
1. alter table table_name modify id int unsigned;
#修改列id的类型为int unsigned
2. alter table table_name change id sid int unsigned;
#修改列id的名字为sid，而且把属性修改为int unsigned
```

### <font style="color:rgb(83, 104, 121);">修改一个字段的默认值：</font>

```sql
alter table table_name modify column_name datatype not null default "";
```

> <font style="color:rgb(83, 104, 121);">例如：alter table test_db modify name char(16) default not null "yourname";</font>

### <font style="color:rgb(83, 104, 121);">对表重新命名：</font>

```sql
alter table table_name rename as new_table_name;
```

> <font style="color:rgb(83, 104, 121);">例如：alter table bbs rename as bbs_table;</font>

```sql
rename table old_table_name to new_table_name;
```

> <font style="color:rgb(83, 104, 121);">例如：rename table test_db to accp;</font>

### <font style="color:rgb(83, 104, 121);">从已经有的表中复制表的结构：</font>

```sql
create table table2 select * from table1 where 1<>1;
```

> <font style="color:rgb(83, 104, 121);">例如：create table test_db select \* from accp where 1<>1;</font>

### <font style="color:rgb(83, 104, 121);">查询时间：</font>

```sql
select now();
```

### <font style="color:rgb(83, 104, 121);">查询当前用户：</font>

```sql
select user();
```

### <font style="color:rgb(83, 104, 121);">查询数据库版本：</font>

```plain
select version();
```

### <font style="color:rgb(83, 104, 121);">创建索引：</font>

```sql
alter table table1 add index ind_id(id);
create index ind_id on table1(id);
create unique index ind_id on table1(id);//建立唯一性索引
```

### <font style="color:rgb(83, 104, 121);">删除索引：</font>

```sql
drop index idx_id on table1;
alter table table1 drop index ind_id;
```

### <font style="color:rgb(83, 104, 121);">联合字符或者多个列（将 id 与":"和列 name 和"="连接）</font>

```sql
select concat(id，':',name,'=') from table;
```

### <font style="color:rgb(83, 104, 121);">limit（选出 10 到 20 条）</font>

```sql
select * from bbs order by id limit 9,10;
```

> <font style="color:rgb(83, 104, 121);">（从查询结果中列出第几到几条的记录）</font>

### <font style="color:rgb(83, 104, 121);">增加一个管理员账号：</font>

```sql
grant all on *.* to user@localhost identified by "password";
```

### <font style="color:rgb(83, 104, 121);">创建表是先判断表是否存在</font>

```sql
create table if not exists students(……);
```

### <font style="color:rgb(83, 104, 121);">复制表：</font>

```sql
create table table2 select * from table1;
```

> <font style="color:rgb(83, 104, 121);">例如：create table test_db select \* from accp;</font>

### <font style="color:rgb(83, 104, 121);">授于用户远程访问 mysql 的权限</font>

```sql
grant all privileges on *.* to "root"@"%" identified by "password" with grant option;
# 或者是修改mysql数据库中的user表中的host字段
use mysql;
select user,host from user;
update user set host="%" where user="user_name";
```

### <font style="color:rgb(83, 104, 121);">查看当前状态</font>

```sql
show status;
```

### <font style="color:rgb(83, 104, 121);">查看当前连接的用户</font>

```sql
show processlist;
```

> <font style="color:rgb(83, 104, 121);">（如果是 root 用户，则查看全部的线程，得到的用户连接数同 show status;里的 Threads_connected 值是相同的）!</font>

### <font style="color:rgb(83, 104, 121);">替换字段中的部分数据</font>

> UPDATE 你的表名 SET 你要查找的字段明=REPLACE(你要查找的字段明,'你要替换的字符串','你需要替换成什么');

案例代码：

```sql
UPDATE cmf_article SET title=REPLACE(title,'hello','你好');
```
