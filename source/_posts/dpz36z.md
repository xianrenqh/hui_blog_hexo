---
title: 解决nginx反向代理后页面上的js/css文件无法加载
urlname: dpz36z
date: '2022-07-01 00:54:17 +0000'
tags:
  - nginx
  - 反向代理
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url:
copyright_author:
cover:
---

问题现象：
nginx 配置反向代理后，网页可以正常访问，但是页面上的 js、css 和图片等资源都无法访问。

```php
（1）nginx配置如下：
（2）域名访问：js css文件无法加载；
（3）IP访问：js css文件可以正常加载；
（4）CI框架下无法访问
```

配置此例即可：

```php
location / {
                proxy_pass http://127.0.0.1:8000;
                include naproxy.conf;
        }
```

解决方法：
nginx 配置文件中，修改为如下配置：

```php
 location ~ \.php$ {
                proxy_pass http://127.0.0.1:8000;
                include naproxy.conf;
        }
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
                expires      30d;
                proxy_pass http://127.0.0.1:8000;
                include naproxy.conf;
        }

        location ~ .*\.(js|css)?$ {
                expires      12h;
                proxy_pass http://127.0.0.1:8000;
                include naproxy.conf;
        }
```

需要把静态文件也添加反向代理设置。
原因分析：
反向代理的路径下找不到文件，这里需要单独指定 js 和 css 文件的访问路径。
