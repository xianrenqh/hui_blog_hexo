---
title: Nginx的优化和压力测试
urlname: hgr8sf
date: '2022-07-04 02:25:56 +0000'
tags: []
categories: []
---

tags: [压测,nginx]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.net

<font style="color:rgb(38, 38, 38);">copyright_url: </font>https://blog.csdn.net/qq_30038111/article/details/79794377<font style="color:rgb(38, 38, 38);">  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

<font style="color:rgb(77, 77, 77);">我们要测试 </font>nginx<font style="color:rgb(77, 77, 77);"> 的负载能力，需要借助压力测试工具。本博客是使用 Apache 服务器自带的一个 web 压力测试工具 ApacheBench ，简称 ab。ab 是一个命令行工具，即通过 ab 命令行，模拟多个请求同时对某一 URL 地址进行访问，因此可以用来测试目标服务器的负载压力。</font><font style="color:rgb(51, 51, 51);">  
</font>

## <font style="color:rgb(79, 79, 79);">ab 的安装</font>

ab 的安装可以去官网下载，如果不想安装 apache，又想使用 ab 命令，可以直接安装工具包 httpd-tools，该工具包会将 ab 命令安装到 /usr/bin 下，因此在任何地方都可以调用：

> yum -y install httpd-tools

检查 ab 的安装结果

```shell
ab -V

# 显示下面信息表示安装成功
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
```

#

ab 的简单使用

```shell
# 查看 ab 的命令及参数
ab -help
```

```shell
# 最简单的使用
# -c：一次并发请求的数量；-n：请求总次数
ab -c 5000 -n 200000 http://192.168.222.101:80/index.html
```

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1656901686719-232b21a9-be96-433a-9922-d3da97cd4a03.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1656901712665-f366e7d2-1b3a-4c35-9f3f-07c9558c4cea.png)

## <font style="color:rgb(79, 79, 79);">优化思路</font>

<font style="color:rgb(77, 77, 77);">每个请求都需要建立 socket 连接，那么影响并发量的因素之一：</font>

1. <font style="color:rgba(0, 0, 0, 0.75);">客户端不允许一次性创建过多的连接</font>
2. <font style="color:rgba(0, 0, 0, 0.75);">服务端不允许一次性创建过多的连接  
   </font><font style="color:rgba(0, 0, 0, 0.75);">每个请求都要访问一些资源，那么影响并发量的因素之一：</font>
3. <font style="color:rgba(0, 0, 0, 0.75);">服务端不允许一个文件在同一时间点被访问 N 次，相当于一个文件在服务端打开 N 次</font>

<font style="color:rgb(77, 77, 77);">我们在使用 ab 模拟并发访问后，执行 dmesg 命令，查看请求的信息</font>

```shell
dmesg
# 执行结果的最后一行为：possible SYN flooding on port 80. Sending cookies.
# 解释：在同一时间有过多的请求访问 80 端口，导致系统误认为遭受了洪水攻击
```

<font style="color:rgb(77, 77, 77);">从上面的优化分析中，我们可以从 socket 和文件两个层面进行 Nginx 的高并发优化。</font>

**<font style="color:rgb(77, 77, 77);">socket：分为系统层面和 nginx 层面：</font>**

- <font style="color:rgba(0, 0, 0, 0.75);">系统层面</font>

```shell
# 禁止洪水抵御，这个操作在重启之后失效
命令：more /proc/sys/net/ipv4/tcp_syncookies
结果：1
命令：echo 0 > /proc/sys/net/ipv4/tcp_syncookies
# 最大连接数，这个操作在重启之后失效
命令：more /proc/sys/net/core/somaxconn
结果：128
命令：echo 50000 > /proc/sys/net/core/somaxconn
# 加快 tcp 连接的回收，这个操作在重启之后失效
命令：cat /proc/sys/net/ipv4/tcp_tw_recycle
结果：0
命令：echo 1 > /proc/sys/net/ipv4/tcp_tw_recycle
# 使空的 tcp 连接重新被利用，这个操作在重启之后失效
命令：cat /proc/sys/net/ipv4/tcp_tw_reuse
结果：0
命令：echo 1 > /proc/sys/net/ipv4/tcp_tw_reuse

# 完整的命令
echo 50000 > /proc/sys/net/core/somaxconn
echo 0 > /proc/sys/net/ipv4/tcp_syncookies
echo 1 > /proc/sys/net/ipv4/tcp_tw_recycle
echo 1 > /proc/sys/net/ipv4/tcp_tw_reuse
```

- <font style="color:rgba(0, 0, 0, 0.75);">nginx</font>

```shell
# 子进程允许打开的连接数（nginx.conf）及 IO 选择
events {
    # 子进程连接数
    worker_connections 65535;
    # 使用 linux 下的多路复用 IO，是 poll 增强版本
    use epoll;
}

# 将 keepalive_timeout 设置为 0，高并发网站中，一般不超过 2s。此参数在 F12 调试页面时，在 Network 的 Headers 的 Response Headers 下可以看到 Connection:keep-alive，如果设置为 0，那么为 Connection:close
keepalive_timeout 0;
```

**<font style="color:rgb(77, 77, 77);">文件：分为系统层面和 nginx 层面：</font>**

- <font style="color:rgba(0, 0, 0, 0.75);">系统层面</font>

```shell
# 设置同一个文件同一时间点可以打开 N 次，这个操作在重启之后失效
命令：ulimit -n
返回：128
命令：ulimit -n 20000
```

- <font style="color:rgba(0, 0, 0, 0.75);">nginx 层面</font>

```shell
# nginx 进程数，按照 CPU 数目指定
worker_processes 8;
# nginx 子进程允许打开的文件次数
worker_rlimit_nofile 102400;
```

## <font style="color:rgb(79, 79, 79);">ab 使用时的注意点</font>

<font style="color:rgb(77, 77, 77);">如果使用 A 和 B 两台虚拟机测试，用 B 上的 ab 测试 A 的 nginx ，即 A 为服务端，B 为客户端。此时需要在 B 上配置下面的参数，并且两个参数至少要等于 A 配置的值。</font>

```shell
ulimit -n 20000
echo 50000 > /proc/sys/net/core/somaxconn
```

## <font style="color:rgb(79, 79, 79);">Nginx 添加统计模块及配置</font>

<font style="color:rgb(77, 77, 77);">nginx 在安装时可以添加访问的统计模块。</font>

```php
./configure --prefix=/usr/local/nginx --with-http_stub_status_module
```

```shell
# 在 nginx.conf 中配置统计模块
location /status{
    # 开启状态
    stub_status on;
    # 不需要日志
    access_log off;
    # 只允许此 ip 访问
    allow 192.168.222.101;
    # 其他 ip 禁止访问
    deny all;
}
```

## <font style="color:rgb(79, 79, 79);">使用 Nginx 的统计模块查看状态</font>

```php
# ab 测试并发
ab -c 1000 -n 100000 http://192.168.222.101:80/index.html
```

```php
# 浏览器打开
http://192.168.222.101:80/status
```
