---
title: 【笔记】优麒麟安装宝塔后使用php无法访问网站的解决方案
urlname: qkb5iu
date: '2022-05-17 02:04:08 +0000'
tags:
  - linux
  - 权限
  - 宝塔
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url:
copyright_author:
cover:
---

#

前言

> 小灰灰把本地系统更换成了优麒麟 Linux 系统做开发 PHP，安装宝塔后始终无法访问正确的站点。整理一下问题所在。

# 问题整理

# 1、伪静态没有正常设置

这种情况最为简单，设置好对应的伪静态即可。

# 2、PHP 版本没有设置正确

开发 PHP 的时候的版本对应设置好即可。

# 3、无法找到路径

这种情况很有可能是从其他地方迁移过来系统后路径不正确（原代码 的根目录里面有个 user.ini 配置文件）。
解决方案：删除掉 user.ini 即可。

# 4、404

明明各种都设置正确，可是访问之后会提示 404。此时很有可能程序的目录文件夹没有权限。
1、更改文件夹权限
2、查看 nginx 配置的用户设置，是否有权限：
![微信截图_20220517101235.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652753589393-7b0189ae-1965-4f0a-a3ff-55de5ab397a1.png#clientId=u1bc814c1-c3b8-4&from=paste&height=600&id=u7921f036&name=%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220517101235.png&originHeight=600&originWidth=652&originalType=binary∶=1&rotation=0&showTitle=false&size=46853&status=done&style=none&taskId=ua1a113fb-56a2-4e46-b3b2-1c8d9550cfb&title=&width=652)

# 5、能访问静态文件，无法访问 php 等文件

Nginx 的 user 和 php 版本里面的 php-fpm 配置里面的 user 不同。
更改：php 设置的-fpm：
![222.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27022430/1652753705278-0d9bedfa-7315-41ef-885f-988ad09991f6.jpeg#clientId=u1bc814c1-c3b8-4&from=paste&height=589&id=ubced6250&name=222.jpg&originHeight=589&originWidth=641&originalType=binary∶=1&rotation=0&showTitle=false&size=42144&status=done&style=none&taskId=ub9443edf-2760-4891-8e6e-a1a16dc6dd3&title=&width=641)
