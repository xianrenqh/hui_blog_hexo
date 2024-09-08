---
title: php-fpm优化方法 pm.min_spare_servers、pm.max_spare_servers 的真实意义
urlname: tre5aq
date: '2022-05-26 01:25:51 +0000'
tags: []
categories: []
---

tags: [php,fpm,php-fpm]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.net

<font style="color:rgb(38, 38, 38);">copyright_url: </font>https://blog.csdn.net/a5816138/article/details/53670597<font style="color:rgb(38, 38, 38);">  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

# <font style="color:rgb(51, 51, 51);"></font><font style="color:rgb(0, 0, 0);">php-fpm 进程池优化方法</font>

<font style="color:rgb(0, 0, 0);">php-fpm 进程池</font><font style="color:rgb(0, 0, 0);">开启进程</font><font style="color:rgb(0, 0, 0);">有两种方式，一种是</font><font style="color:rgb(0, 0, 0);">static，</font><font style="color:rgb(0, 0, 0);">直接开启指定数量的 php-fpm 进程，不再增加或者减少；</font><font style="color:rgb(77, 77, 77);">  
</font><font style="color:rgb(0, 0, 0);">另一种则是</font><font style="color:rgb(0, 0, 0);">dynamic，</font><font style="color:rgb(0, 0, 0);">开始时开启一定数量的 php-fpm 进程，当请求量变大时，动态的增加 php-fpm 进程数到上限，当空闲时自动释放空闲的进程数到一个下限。</font><font style="color:rgb(77, 77, 77);">  
</font><font style="color:rgb(0, 0, 0);">这两种不同的执行方式，可以根据服务器的实际需求来进行调整。</font>

<font style="color:rgb(0, 0, 0);">要用到的一些参数，分别是 pm、pm.max_children、pm.start_servers、pm.min_spare_servers 和 pm.max_spare_servers。</font>

<font style="color:rgb(0, 0, 0);">pm 表示使用那种方式，有两个值可以选择，就是 static（静态）或者 dynamic（动态）。</font>

<font style="color:rgb(0, 0, 0);">下面 4 个参数的意思分别为：</font>

<font style="color:rgb(0, 0, 0);">（这里要注意 pm.max_spare_servers 的值只能小于等于 pm.max_children）</font>

> **<font style="color:rgb(0, 0, 0);">pm.max_children</font>**<font style="color:rgb(0, 0, 0);">：静态方式下开启的 php-fpm 进程数量，在动态方式下他限定 php-fpm 的最大进程数</font>  
> **<font style="color:rgb(0, 0, 0);">pm.start_servers</font>**<font style="color:rgb(0, 0, 0);">：动态方式下的起始 php-fpm 进程数量。</font>  
> **<font style="color:rgb(0, 0, 0);">pm.min_spare_servers</font>**<font style="color:rgb(0, 0, 0);">：动态方式空闲状态下的最小 php-fpm 进程数量。</font>  
> **<font style="color:rgb(0, 0, 0);">pm.max_spare_servers</font>**<font style="color:rgb(0, 0, 0);">：动态方式空闲状态下的最大 php-fpm 进程数量。</font>

<font style="color:rgb(0, 0, 0);">如果 dm 设置为 static，那么其实只有 pm.max_children 这个参数生效。系统会开启</font><font style="color:rgb(0, 0, 0);">参数</font><font style="color:rgb(0, 0, 0);">设置数量的 php-fpm 进程。</font>

<font style="color:rgb(0, 0, 0);">如果 dm 设置为 dynamic，4 个参数都生效。系统会在 php-fpm 运行开始时启动 pm.start_servers 个 php-fpm 进程，然后根据系统的需求动态在 pm.min_spare_servers 和 pm.max_spare_servers 之间调整 php-fpm 进程数。</font>

# <font style="color:rgb(0, 0, 0);">P.S</font>

<font style="color:rgb(0, 0, 0);">pm.min_spare_servers、</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(0, 0, 0);">pm.max_spare_servers 这 2 个参数一开始我以为是指空闲进程，但是后来服务器给我报了一个错误：</font><font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(0, 0, 0);">pm.start_servers(70) must not be less than pm.min_spare_servers(15) and not greater than pm.max_spare_servers(60)</font><font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(0, 0, 0);">要求 pm.start_servers 的值在 pm.min_spare_servers 和 pm.max_spare_servers 之间  
</font><font style="color:rgb(0, 0, 0);">经过测试，得出上述结论。</font>
