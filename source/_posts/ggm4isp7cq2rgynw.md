---
title: 宝塔面板进程守护管理器（Supervisor）踩坑日记
urlname: ggm4isp7cq2rgynw
date: '2024-05-22 03:25:46 +0000'
tags: []
categories: []
---

tags: [守护进程,supervisor]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.cc

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

> <font style="color:rgb(51, 51, 51);">宝塔面板，正常使用过程中，突然守护进程异常报错，无论重启还是停止均无法执行。</font>
>
> <font style="color:rgb(51, 51, 51);">这里记录一下，以便后期参考使用</font>

## <font style="color:rgb(51, 51, 51);">找到错误日志：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1716348561561-38e57629-0f27-4c02-8e74-7b8ea2884690.png)

排查看到：

**<font style="color:#DF2A3F;">Failed to start supervisord.service:Unit is not loaded properly: Is a directory.</font>**

提示 **<font style="color:#DF2A3F;">supervisord.service </font>**单元是一个目录。

找到对应位置：

/usr/lib/systemd/system 并搜索：**<font style="color:#DF2A3F;">supervisord.service</font>**

可以看到 搜索到的是一个目录而非文件。

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1716348991148-de5860cd-b407-4ae8-927b-27eb7e219e99.png)

## 修复方案：

### 一、

找到：软件商店》守护进程》点击修复。之后正常

二、

找到 unit 目录 /usr/lib/systemd/system ，删除文件夹 supervisord.service 新建文件 supervisord.service，

并创建文件内容：

```shell
[Unit]
Description=Process Monitoring and Control Daemon
After=rc-local.service nss-user-lookup.target

[Service]
Type=forking
ExecStart=/www/server/panel/pyenv/bin/python /www/server/panel/pyenv/bin/supervisord -c /etc/supervisor/supervisord.conf
ExecStop=/www/server/panel/pyenv/bin/python /www/server/panel/pyenv/bin/supervisord  shutdown
ExecReload=/www/server/panel/pyenv/bin/python /www/server/panel/pyenv/bin/supervisord  reload
KillMode=process
Restart=on-failure
RestartSec=42s
[Install]
WantedBy=multi-user.target
```

之后正常。
