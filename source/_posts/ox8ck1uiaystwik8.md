---
title: 使用 Frp 为你的 Web 服务添加 https 支持
urlname: ox8ck1uiaystwik8
date: '2023-12-20 02:46:17 +0000'
tags:
  - frp
  - https
categories: '<font style="color:rgb(38, 38, 38);">学无止境</font>'
copyright_author_href: 'https://www.xiaohuihui.cc'
'</font><font style="color:rgb(38, 38, 38);">copyright_author': </font>
'<font style="color:rgb(33, 37, 41);">cover': </font>
<font style="color:rgb(38, 38, 38);">copyright_url:
---

> <font style="color:rgb(119, 119, 119);"> frp 是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp 协议，为 http 和 https 应用协议提供了额外的能力，且尝试性支持了点对点穿透。</font>

<font style="color:#DF2A3F;">本</font>**<font style="color:#DF2A3F;">教程是根据 ftp-win 客户端 3.\*版本</font>**

# <font style="color:rgb(0, 0, 0);">下载 frp</font>

<font style="color:rgb(51, 51, 51);">前往 GitHub 下载 frp：</font>

- [Releases · fatedier/frp](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fgithub.com%2Ffatedier%2Ffrp%2Freleases&source=article&objectId=1581948)

<font style="color:rgb(51, 51, 51);">有适用于各种不同操作系统的 frp，如果你对外提供的公网服务器和实际提供 Web 服务的服务器不是同一台机器的话，需要为各自机器下载对应版本的 frp。</font>

# <font style="color:rgb(0, 0, 0);">准备好 Web 服务和 SSL 证书</font>

<font style="color:rgb(51, 51, 51);">你可以用任何方式开发你的 Web 服务，注意你的 Web 服务需要监听一个本机端口。</font>

<font style="color:rgb(51, 51, 51);">对于本文的后续内容，你需要将证书导出成 Nginx 格式，即一个 .pem 文件和一个 .key 文件。</font>

# <font style="color:rgb(0, 0, 0);">配置 frp</font>

## 配置 frps.ini

```properties
[common]
bind_port = 5443
vhost_http_port = 80
vhost_https_port = 443
token = ******

```

## 配置代理 http

编辑 frpc.ini

```properties
[common]
# 服务器地址
server_addr = *.*.*.*
# 服务器端口
server_port = 5443
# 服务器连接凭证
token = **********

[web_webman1]
type = http
local_port = 8787
remote_port = 9980
custom_domains = qianxun.local.com

[web_webman2] #将http转发为https
type = https
local_port = 8787
remote_port = 9980
custom_domains = qianxun.local.com
plugin = https2http	#FRP的http转https服务
plugin_local_addr = 127.0.0.1:8787
plugin_host_header_rewrite = 127.0.0.1
# 指定代理方式为 frp
plugin_header_X-From-Where = frp
use_encryption = true
# 强制跳转https
force_https = true
# 指定成你在前面部分导出的证书的路径
plugin_crt_path = ./ssl/fullchain.pem
plugin_key_path = ./ssl/privkey.key

```

## <font style="color:rgb(74, 74, 74);">配置 Nginx 代理</font>

在本地的 nginx 服务里，新增个站点，并配置 nginx 代理：

```nginx
# frp的接收https请求的反向代理
server {
        listen       443 ssl http2;
        listen       [::]:443 ssl http2;
        # 访问的域名
        server_name qianxun.local.com;
		# 配置https证书
        ssl_certificate "/etc/nginx/ssl_files/server.pem";
        ssl_certificate_key "/etc/nginx/ssl_files/server.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        location / {
                # 7171对应vhost_http_port
                proxy_pass http://127.0.0.1:7171;
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";
                # Expect for doris
                proxy_set_header Expect $http_expect;

                proxy_connect_timeout 7d;
                proxy_send_timeout 7d;
                proxy_read_timeout 7d;
        }
}
```

其中 qianxun.local.com 为需要开通的 https 的域名。

最后将对应的域名证书放好。

然后分别启动：

```nginx
./frps.exe -c ./frps.ini


 ./frpc -c ./frpc.ini
```
