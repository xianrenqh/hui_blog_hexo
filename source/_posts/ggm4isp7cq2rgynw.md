---
title: 宝塔面板进程守护管理器（Supervisor）踩坑日记
urlname: ggm4isp7cq2rgynw
date: '2024-05-22 03:25:46 +0000'
tags:
  - 守护进程
  - supervisor
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

> 宝塔面板，正常使用过程中，突然守护进程异常报错，无论重启还是停止均无法执行。
> 这里记录一下，以便后期参考使用

## 找到错误日志：

![d2e06cd87d88f75fa02fd0bad240037.png](https://cdn.nlark.com/yuque/0/2024/png/27022430/1716348561561-38e57629-0f27-4c02-8e74-7b8ea2884690.png#averageHue=%2354514f&clientId=u5e8e42e4-989a-4&from=paste&height=709&id=u28b1112f&originHeight=709&originWidth=1549&originalType=binary∶=1&rotation=0&showTitle=false&size=123584&status=done&style=none&taskId=u287102a5-d45a-4f76-a577-ddf23ecfec6&title=&width=1549)
排查看到：
**Failed to start supervisord.service:Unit is not loaded properly: Is a directory.**
提示 **supervisord.service **单元是一个目录。
找到对应位置：
/usr/lib/systemd/system 并搜索：**supervisord.service**
可以看到 搜索到的是一个目录而非文件。
![c12343cbeefa0ae544142b7cbb51e57.png](https://cdn.nlark.com/yuque/0/2024/png/27022430/1716348991148-de5860cd-b407-4ae8-927b-27eb7e219e99.png#averageHue=%23fafafa&clientId=u5e8e42e4-989a-4&from=paste&height=815&id=u72342fce&originHeight=815&originWidth=1252&originalType=binary∶=1&rotation=0&showTitle=false&size=75986&status=done&style=none&taskId=ue0fe2e79-4c8a-495c-b07a-c17c111c152&title=&width=1252)

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
