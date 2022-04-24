---
title: webman使用Nginx代理
urlname: lnhoy6
date: '2022-04-24 08:05:47 +0000'
tags: []
categories: []
---

当 webman 需要直接提供外网访问时，建议在 webman 前增加一个 nginx 代理，这样有以下好处。

- 静态资源由 nginx 处理，让 webman 专注业务逻辑处理
- 让多个 webman 共用 80、443 端口，通过域名区分不同站点，实现单台服务器部署多个站点
- 能够实现 php-fpm 与 webman 架构共存
- nginx 代理 ssl 实现 https，更加简单高效
- 能够严格过滤外网一些不合法请求

Nginx 示例代码：

```nginx
upstream webman {
    server 127.0.0.1:8877;
}

server {
  server_name 站点域名;
  listen 80;
  root /your/webman/public;

  location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header Host $host;
      if (!-f $request_filename){
          proxy_pass http://webman; #你本地的webman地址，如：127.0.0.1:8787
      }
  }
}
```
