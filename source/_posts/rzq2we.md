---
title: php - api接口跨域问题
urlname: rzq2we
date: '2022-04-13 06:08:54 +0000'
tags:
  - api
  - 跨域
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

### Nginx 伪静态配置为：

```nginx
location / {
  add_header Access-Control-Allow-Origin '*';
  add_header Access-Control-Allow-Methods 'POST,PUT,GET,DELETE';
  add_header Access-Control-Allow-Headers 'version, access-token, user-token, Accept, apiAuth, User-Agent, Keep-Alive, Origin, No-Cache, X-Requested-With, If-Modified-Since, Pragma, Last-Modified, Cache-Control, Expires, Content-Type, ATJ-Token,ATJ-Device-Type';

  if ($request_method = 'OPTIONS') {
    return 204;
  }
  if (!-e $request_filename){
    rewrite  ^(.*)$  /index.php?s=$1  last;   break;
  }
}
```

### 程序入口文件增加：

```php
header("Access-Control-Allow-Origin:*");
header("Access-Control-Allow-Methods:GET, POST, OPTIONS, DELETE");
header("Access-Control-Allow-Headers:DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type, Accept-Language, Origin, Accept-Encoding,ATJ-Device-Type,ATJ-Token");
```

其中，Access-Control-Allow-Headers 里的值为 header 传递的自定义字段值

### 接口公共方法里增加

```php
protected function initialize()
{
    header("Access-Control-Allow-Origin: *");
    header("Access-Control-Allow-Methods:POST,GET");
    header("Access-Control-Allow-Headers:ATJ-Device-Type,ATJ-Token");
    header("Content-type:text/json;charset=utf-8");
}
```
