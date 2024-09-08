---
title: Sql根据不同条件统计总数
urlname: oekruf
date: '2022-06-01 14:25:43 +0000'
tags: []
categories: []
---

tags: [sql,统计总数]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.net

<font style="color:rgb(38, 38, 38);">copyright_url: </font>https://blog.csdn.net/Android_xue/article/details/102802475<font style="color:rgb(38, 38, 38);">  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>SunnyRivers

<font style="color:rgb(33, 37, 41);">cover:</font>

---

<font style="color:rgb(51, 51, 51);">  
</font>

## <font style="color:rgb(79, 79, 79);">前言</font>

<font style="color:rgb(77, 77, 77);">经常会遇到根据不同的条件统计总数的问题，一般有两种写法：count 和 sum 都可以  
</font><font style="color:rgb(77, 77, 77);">数据准备：  
</font>![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1654093561091-7d6274e4-1cf7-41b4-ba48-53a83ebfe2b3.png)

## <font style="color:rgb(79, 79, 79);">方法一 ：Count</font>

<font style="color:rgb(77, 77, 77);">代码：</font>

```plain
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

<font style="color:rgb(77, 77, 77);">结果：  
</font>![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1654093561057-c9fdbc9c-5862-4237-91af-2349aeebc373.png)

## <font style="color:rgb(79, 79, 79);">方法二：sum</font>

<font style="color:rgb(77, 77, 77);">代码：</font>

```plain
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

<font style="color:rgb(77, 77, 77);">结果：  
</font>![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1654093561061-5ad89cb0-dcb8-44e9-9e05-80a5cfd7e033.png)<font style="color:rgb(77, 77, 77);">  
</font><font style="color:rgb(77, 77, 77);">当然也可以和 count 代码一样 ELSE 后面也写为 NULL</font>

```plain
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

## <font style="color:rgb(79, 79, 79);">后记</font>

<font style="color:rgb(77, 77, 77);">其实原理很简单，count 统计的时候有满足条件的就加 1，没有满足的变为 NULL，我们知道聚合函数统计的时候是忽略 null 值的；而 sum 原理和 coun 相似，不过 ELSE 后面可以是 0 或者 NULL。</font>
