---
title: Nginx配置导致的503 Service Temporarily Unavailable问题
urlname: iae66lf20in42fhq
date: '2023-07-27 09:03:03 +0000'
tags: []
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_author: 小灰灰
copyright_url:
cover:
---

#

问题场景：
这两天由于公司人自己请求网站之后，导致网站服务器超负载，为了避免这种攻击我将 Nginx 配置了单个 IP 的并发限制，当天还没有问题，但是第二天早上网站访问量多的时候，浏览器出现页面 503 Service Temporarily Unavailable，然后我多次刷新，有时候刷新页面正常显示，有时候刷新页面出现样式未加载出来（样式错乱，因为加载 css 文件返回 503 状态）的情况，有时候刷新再次出现 503 Service Temporarily Unavailable。

# 问题分析：

503 Service Temporarily Unavailable 的意思是服务暂时不可用，结合我上次的配置，我猜测应该是小站并发高了之后导致单个 IP 超过了我所配置的单个 IP 所允许并发数量。
至于为什么全站的并发高了之后才出现 503 Service Temporarily Unavailable 呢，刚刚配置单个 IP 并发限制我访问页面是正常的。这个原因应该是**全站的访问并发高了之后服务器负载高，服务器处理不过来，所以对于单个 IP 请求一个页面的时候还未处理完成，再次刷新或者跳转别的页面的时候又加入的新的多次请求（包括各种静态文件的请求），所以服务器存在的请求超出了单个 IP 并发配置的限制**，而全站并发不高的时候意味着访问量少，服务器负载低，单个 IP 请求的并发也相对较少，则不容易出现返回 503 Service Temporarily Unavailable 的情况。

# 解决方案：

修改 nginx 单个 IP 并发限制的配置，将单个 IP 并发数限制提升到 20，**这个数字取决于你访问一个页面最多需要走多少次 nginx，一般看一次页面有多少次通过 nginx 的请求，包括 css 文件、图片文件、字体文件和 js 文件等等**。
配置后重新 reload 配置，就再也没有出现过页面显示 503 Service Temporarily Unavailable 的情况，当然可能是负载突然下降了的原因，还需要进一步测试，暂时先这样处理。
本次将 nginx.conf 配置文件的 server 部分配置修改：

```
// 单个IP并发数限制
server{
    limit_conn perip 20;
    limit_conn perserver 20;
}
```

# Nginx 限制并发连接数与下载速度配置

ngx_http_limit_conn_module 模块用于限制每个定义密钥的连接数，特别是来自单个 IP 地址的连接数。而 ngx_http_core_module 则可以限制下载速度，这两个均是[Nginx](https://www.xgss.net/tag/nginx)内置模块，不需要额外安装。

### ngx_http_limit_conn_module 限制连接数

```
#需要写在http段内
limit_conn_zone $binary_remote_addr zone=addr:10m;

server {
location /download/ {
        limit_conn addr 10;
}
```

- $binary_remote_addr : nginx 变量，指的是客户端 IP
- zone : 域的名字，随便填写，这里设置的是 addr，后面会再次用到
- 10m : 设置共享内存我的理解是客户端的 IP 会被放入这个内存中，总共享内存不能超过 10M，不知道对不对。
- limit_conn addr 10 : 限制 addr 这个域的最大连接数为 10

但是在 HTTP/2 中每个并发请求被视为单独的连接，如果网站启用了 HTTP/2 上面的设置就没有作用了，可以继续改进一下。以下配置将限制每个客户端 IP 与服务器的连接数，同时限制与虚拟服务器的连接总数。

```
#写在http段内
limit_conn_zone $binary_remote_addr zone=perip:10m;
limit_conn_zone $server_name zone=perserver:10m;

server {
    ...
    #限制perip域（客户端IP）的连接数为10
    limit_conn perip 10;
    #限制perserver域（当前虚拟服务器）的连接数为100
    limit_conn perserver 100;
}
```

更多详细说明可参考[Nginx](https://www.xgss.net/tag/nginx)官方文档：[http://nginx.org/en/docs/http/ngx_http_limit_conn_module.html](https://www.xgss.net/wp-content/themes/xgssv1/go.php?url=aHR0cDovL25naW54Lm9yZy9lbi9kb2NzL2h0dHAvbmd4X2h0dHBfbGltaXRfY29ubl9tb2R1bGUuaHRtbA==)

### ngx_http_core_module 限制下载速度

```
#数据达到100M后再限制速度（注意：这里指的是单个连接达到100M）
limit_rate_after 100M;
#限制单个连接速度为10k/s
limit_rate 10k;
```

- limit_rate_after : 指的是请求的数据达到指定大小后才开始限速（这里设置的是 100M）
- limit_rate ： 设置单个连接限速值，这里设置的是 10k/s，如果限制同一 IP 最大连接数为 10 的话，那么总的下载速度不能超过 100k/s

更多说明参考[Nginx](https://www.xgss.net/tag/nginx)官方文档：[http://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate](https://www.xgss.net/wp-content/themes/xgssv1/go.php?url=aHR0cDovL25naW54Lm9yZy9lbi9kb2NzL2h0dHAvbmd4X2h0dHBfY29yZV9tb2R1bGUuaHRtbCNsaW1pdF9yYXRl)

### 同时限制连接数和下载速度

将上面的配置整合一下，我们既要限制单 IP 的最大连接数，也需要限制下载速度。

```
#写在http段内
limit_conn_zone $binary_remote_addr zone=perip:10m;
limit_conn_zone $server_name zone=perserver:10m;

#写在server段内
limit_conn perip 10;
limit_conn perserver 100;
limit_rate_after 100M;
limit_rate 10k;
```

上面配置的含义是限制单个 IP 最大连接数为 10 个，同时限制单个虚拟服务器的连接总数为 100 个。当请求的数据达到 100M 后（指单个连接达到 100M）限制连接速度为为 10k/s，如果产生了 10 个连接，最大速度不能超过 100k/s

### 写在最后

配置修改后建议用 nginx -t 先检查语法，确保没有问题，别忘记重载 Nginx 使其生效。
