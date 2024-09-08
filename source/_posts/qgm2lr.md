---
title: mysql实现group by后取各分组的最新一条
urlname: qgm2lr
date: '2022-04-13 06:12:28 +0000'
tags:
  - mysql
  - 分组
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

### <font style="color:rgb(82, 82, 82);">两种实现方式</font>

**<font style="color:rgb(82, 82, 82);">先 order by 之后再分组：</font>**

> <font style="color:rgb(119, 119, 119);">SELECT _ FROM (SELECT _ from tb_dept ORDER BY id descLIMIT 10000) a GROUP BY parent_id;</font>

<font style="color:rgb(82, 82, 82);">不加</font>**<font style="color:rgb(82, 82, 82);">LIMIT</font>**<font style="color:rgb(82, 82, 82);">可能会无效，由于 mysql 的版本问题。但是总觉得这种写法不太正经，因为如果数据量大于 Limit 的值后，结果就不准确了。所以就有了第二种写法。</font>

> <font style="color:rgb(119, 119, 119);">SELECT \* FROM tb_dept td,(SELECT max(id) id FROM tb_dept GROUP BY parent_id) md where </font>[td.id](http://td.id/)<font style="color:rgb(119, 119, 119);"> = </font>[md.id](http://md.id/)<font style="color:rgb(119, 119, 119);">;</font>

<font style="color:rgb(82, 82, 82);">开发过程中真实 sql 语句举例：</font>

```sql
select  * from (select * from guangnian_distribution order by id desc LIMIT 10000) m group by m.guangnian_id,m.user_id order by id desc
```
