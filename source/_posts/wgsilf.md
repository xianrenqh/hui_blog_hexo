---
title: 关于lnmp mysql的一个坑记录一下
urlname: wgsilf
date: '2022-04-13 06:11:06 +0000'
tags:
  - mysql
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

**Mysql5.7 及以上 only_full_group_by 以及其他关于 sql_mode 原因报错详细解决方案**

经过我们一番百度之后，获取的结果是关于 only_full_group_by ，但是按照教程所说，只要修改了 my.cnf，
在 my.cnf 添加如下配置
（或者在 my.ini 里配置新增）

```sql
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```
