---
title: php-fpm优化方法 pm.min_spare_servers、pm.max_spare_servers 的真实意义
urlname: tre5aq
date: '2022-05-26 01:25:51 +0000'
tags:
  - php
  - fpm
  - php-fpm
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url: 'https://blog.csdn.net/a5816138/article/details/53670597'
copyright_author:
cover:
---

# php-fpm 进程池优化方法

php-fpm 进程池开启进程有两种方式，一种是 static，直接开启指定数量的 php-fpm 进程，不再增加或者减少；
另一种则是 dynamic，开始时开启一定数量的 php-fpm 进程，当请求量变大时，动态的增加 php-fpm 进程数到上限，当空闲时自动释放空闲的进程数到一个下限。
这两种不同的执行方式，可以根据服务器的实际需求来进行调整。
要用到的一些参数，分别是 pm、pm.max_children、pm.start_servers、pm.min_spare_servers 和 pm.max_spare_servers。
pm 表示使用那种方式，有两个值可以选择，就是 static（静态）或者 dynamic（动态）。
下面 4 个参数的意思分别为：
（这里要注意 pm.max_spare_servers 的值只能小于等于 pm.max_children）

> **pm.max_children**：静态方式下开启的 php-fpm 进程数量，在动态方式下他限定 php-fpm 的最大进程数
> **pm.start_servers**：动态方式下的起始 php-fpm 进程数量。
> **pm.min_spare_servers**：动态方式空闲状态下的最小 php-fpm 进程数量。
> **pm.max_spare_servers**：动态方式空闲状态下的最大 php-fpm 进程数量。

如果 dm 设置为 static，那么其实只有 pm.max_children 这个参数生效。系统会开启参数设置数量的 php-fpm 进程。
如果 dm 设置为 dynamic，4 个参数都生效。系统会在 php-fpm 运行开始时启动 pm.start_servers 个 php-fpm 进程，然后根据系统的需求动态在 pm.min_spare_servers 和 pm.max_spare_servers 之间调整 php-fpm 进程数。

# P.S

pm.min_spare_servers、 pm.max_spare_servers 这 2 个参数一开始我以为是指空闲进程，但是后来服务器给我报了一个错误：
pm.start_servers(70) must not be less than pm.min_spare_servers(15) and not greater than pm.max_spare_servers(60)
要求 pm.start_servers 的值在 pm.min_spare_servers 和 pm.max_spare_servers 之间
经过测试，得出上述结论。
