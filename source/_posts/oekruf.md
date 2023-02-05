---
title: Sql根据不同条件统计总数
urlname: oekruf
date: '2022-06-01 14:25:43 +0000'
tags:
  - sql
  - 统计总数
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url: 'https://blog.csdn.net/Android_xue/article/details/102802475'
copyright_author: SunnyRivers
cover:
---

## 前言

经常会遇到根据不同的条件统计总数的问题，一般有两种写法：count 和 sum 都可以
数据准备：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1654093561091-7d6274e4-1cf7-41b4-ba48-53a83ebfe2b3.png#clientId=ufdbd7940-8ec2-4&from=paste&id=ud3d43eb6&name=image.png&originHeight=169&originWidth=206&originalType=url∶=1&rotation=0&showTitle=false&size=13537&status=done&style=none&taskId=u04100260-f1e9-4f63-9aac-4ff5d8b022c&title=)

## 方法一 ：Count

代码：

```
SELECT
	COUNT(
		CASE
		WHEN age > 20
		AND age < 25 THEN
			1
		ELSE
			NULL
		END
	) AS cnt0,
	COUNT(
		CASE
		WHEN age >= 25
		AND age < 30 THEN
			1
		ELSE
			NULL
		END
	) AS cnt1
FROM
	USER;

```

结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1654093561057-c9fdbc9c-5862-4237-91af-2349aeebc373.png#clientId=ufdbd7940-8ec2-4&from=paste&id=u825895be&name=image.png&originHeight=61&originWidth=154&originalType=url∶=1&rotation=0&showTitle=false&size=3244&status=done&style=none&taskId=udbb4fbb6-27fa-44ef-8bd8-16d9df6528d&title=)

## 方法二：sum

代码：

```
SELECT
	SUM(
		CASE
		WHEN age > 20
		AND age < 25 THEN
			1
		ELSE
			0
		END
	) AS cnt0,
	SUM(
		CASE
		WHEN age >= 25
		AND age < 30 THEN
			1
		ELSE
			0
		END
	) AS cnt1
FROM
	USER;

```

结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1654093561061-5ad89cb0-dcb8-44e9-9e05-80a5cfd7e033.png#clientId=ufdbd7940-8ec2-4&from=paste&id=u98b08105&name=image.png&originHeight=61&originWidth=151&originalType=url∶=1&rotation=0&showTitle=false&size=3361&status=done&style=none&taskId=u032f97ab-91da-4174-b81c-05891ba2930&title=)
当然也可以和 count 代码一样 ELSE 后面也写为 NULL

```
SELECT
	SUM(
		CASE
		WHEN age > 20
		AND age < 25 THEN
			1
		ELSE
			NULL
		END
	) AS cnt0,
	SUM(
		CASE
		WHEN age >= 25
		AND age < 30 THEN
			1
		ELSE
			NULL
		END
	) AS cnt1
FROM
	USER;

```

## 后记

其实原理很简单，count 统计的时候有满足条件的就加 1，没有满足的变为 NULL，我们知道聚合函数统计的时候是忽略 null 值的；而 sum 原理和 coun 相似，不过 ELSE 后面可以是 0 或者 NULL。
