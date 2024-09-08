---
title: 【笔记】优麒麟安装宝塔后使用php无法访问网站的解决方案
urlname: qkb5iu
date: '2022-05-17 02:04:08 +0000'
tags: []
categories: []
---

tags: [linux,权限,宝塔]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.<font style="color:rgb(38, 38, 38);">cc</font>

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

# <font style="color:rgb(51, 51, 51);">

</font><font style="color:rgb(51, 51, 51);">前言</font>

> <font style="color:rgb(51, 51, 51);">小灰灰把本地系统更换成了优麒麟 Linux 系统做开发 PHP，安装宝塔后始终无法访问正确的站点。整理一下问题所在。</font>

# <font style="color:rgb(51, 51, 51);">问题整理</font>

# <font style="color:rgb(51, 51, 51);">1、伪静态没有正常设置</font>

<font style="color:rgb(51, 51, 51);">这种情况最为简单，设置好对应的伪静态即可。</font>

# <font style="color:rgb(51, 51, 51);">2、PHP 版本没有设置正确</font>

<font style="color:rgb(51, 51, 51);">开发 PHP 的时候的版本对应设置好即可。</font>

# <font style="color:rgb(51, 51, 51);">3、无法找到路径</font>

<font style="color:rgb(51, 51, 51);">这种情况很有可能是从其他地方迁移过来系统后路径不正确（原代码 的根目录里面有个 user.ini 配置文件）。</font>

<font style="color:rgb(51, 51, 51);">解决方案：删除掉 user.ini 即可。</font>

# <font style="color:rgb(51, 51, 51);">4、404</font>

<font style="color:rgb(51, 51, 51);">明明各种都设置正确，可是访问之后会提示 404。此时很有可能程序的目录文件夹没有权限。</font>

<font style="color:rgb(51, 51, 51);">1、更改文件夹权限</font>

<font style="color:rgb(51, 51, 51);">2、查看 nginx 配置的用户设置，是否有权限：</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652753589393-7b0189ae-1965-4f0a-a3ff-55de5ab397a1.png)

# 5、能访问静态文件，无法访问 php 等文件

Nginx 的 user 和 php 版本里面的 php-fpm 配置里面的 user 不同。

更改：php 设置的-fpm：

![](https://cdn.nlark.com/yuque/0/2022/jpeg/27022430/1652753705278-0d9bedfa-7315-41ef-885f-988ad09991f6.jpeg)
