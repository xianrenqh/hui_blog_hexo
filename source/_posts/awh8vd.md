---
title: Github Action 使用云函数调度服务
urlname: awh8vd
date: '2022-04-08 03:49:09 +0000'
categories:
  - 学无止境
tags:
  - github
  - action
  - 云函数
  - 调度
cover: >-
  https://tse1-mm.cn.bing.net/th/id/R-C.5660045af32fb6185303af27f36acf0d?rik=8qDr8OVk%2fmSILA&riu=http%3a%2f%2fblog-images.qiniu.wqf31415.xyz%2fgithub_action_3.png&ehk=ZPqfDfyFjW6CUcyKK4prggpXliv5%2f907uHUt%2b371uZ8%3d&risl=&pid=ImgRaw&r=0
copyright_author_href: 'https://www.xiaohuihui.net'
---

## <font style="color:rgb(79, 79, 79);">如何开启 GitHubAction 的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workflow_dispatch </font><font style="color:rgb(79, 79, 79);">触发器</font>

<font style="color:rgb(77, 77, 77);">在工作流的 </font>**<font style="color:rgb(77, 77, 77);">yml</font>**<font style="color:rgb(77, 77, 77);"> 文件定义的 </font>**<font style="color:rgb(77, 77, 77);">on</font>**<font style="color:rgb(77, 77, 77);"> 中，提供</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workflow_dispatch</font><font style="color:rgb(77, 77, 77);">触发器 【必须】</font>

```yaml
#通过 workflow_dispatch 触发，cron任务部署在云函数
name: 你的工作流名称

on:
  workflow_dispatch:

jobs:
  start:
#以下省略
```

<font style="color:rgb(77, 77, 77);">开启</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workflow_dispatch</font><font style="color:rgb(77, 77, 77);">后，你的工作流中会出现运行工作流的按钮,手动点击</font><font style="color:rgb(51, 51, 51);">Run workflow</font><font style="color:rgb(77, 77, 77);">按钮，将会执行一次工作流：</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649390027837-ee2a2970-3e9f-4557-984b-a7c403e9693c.png)

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">现在可以进入下一步，使用 </font>**<font style="color:rgb(77, 77, 77);">cron</font>**<font style="color:rgb(77, 77, 77);"> 调度触发工作流的手动执行。</font>

## <font style="color:rgb(79, 79, 79);">使用云函数提供 cron 调度服务</font>

<font style="color:rgb(77, 77, 77);">工具的选择上，人们总是会选择便捷且有效。如果恰好是免费的，那就大快人心。  
</font><font style="color:rgb(77, 77, 77);">我选择的</font><font style="color:rgb(77, 77, 77);"> </font>**<font style="color:rgb(77, 77, 77);">cron</font>**<font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">调度是</font><font style="color:rgb(77, 77, 77);"> </font>[腾讯云函数 SCF](https://cloud.tencent.com/product/scf/)<font style="color:rgb(77, 77, 77);">.。简单的 POST 一个链接，不会达到收费标准。  
</font><font style="color:rgb(77, 77, 77);">我们打开云函数的创建界面：</font>

1. <font style="color:rgba(0, 0, 0, 0.75);">选择自定义创建</font>
2. <font style="color:rgba(0, 0, 0, 0.75);">函数名称为默认的 helloworld-1623250991 ，可以自己输入名称。</font>
3. <font style="color:rgba(0, 0, 0, 0.75);">地域使用任何地域即可，这里默认选择了广州</font>
4. <font style="color:rgba(0, 0, 0, 0.75);">运行环境选择 Python3.6 （Python 2.7 版本也行）</font>
5. <font style="color:rgba(0, 0, 0, 0.75);">提交方法选择在线编辑。</font>
6. <font style="color:rgba(0, 0, 0, 0.75);">执行方法使用默认值：index.main_handler</font>
7. <font style="color:rgba(0, 0, 0, 0.75);">函数代码如下：</font>

<font style="color:rgba(0, 0, 0, 0.75);"></font>

**Python 2.7 版本代码：**

```python
# -*- coding: utf8 -*-
import requests
def main_handler(event, context):
    r = requests.post("https://api.github.com/repos/xianrenqh/hui_blog_hexo/actions/workflows/23465728/dispatches",
    json = {"ref": "master"},
    headers = {"User-Agent":'curl/7.52.1',
              'Content-Type': 'application/json',
              'Accept': 'application/vnd.github.v3+json',
              'Authorization': 'token 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'})
    if r.status_code == 204:
        return "This's OK!"
    else:
        return r.status_code

```

**Python 3.6 版本代码：**

```python
import requests
import json

def run():
        payload = json.dumps({"ref": "master"})
        header = {'Authorization': 'token GitHubToken',
                  "Accept": "application/vnd.github.v3+json"}
        response_decoded_json = requests.post(
            f'https://api.github.com/repos/Github账号/Github项目名/actions/workflows/GithubAction工作流名称或ID/dispatches',
            data=payload, headers=header)

# 云函数入口
def main_handler(event, context):
    return run()

```

8. <font style="color:rgba(0, 0, 0, 0.75);">函数代码中的 </font>**<font style="color:rgba(0, 0, 0, 0.75);">GitHubToken、Github 账号、Github 项目名、GithubAction 工作流名称或 ID</font>**<font style="color:rgba(0, 0, 0, 0.75);"> 需要根据自己的账号及项目填写。具体的 API 调用规则可参考: </font>[GithubDoc](https://docs.github.com/en/rest/reference/actions#create-a-workflow-dispatch-event)<font style="color:rgba(0, 0, 0, 0.75);">.</font>

<font style="color:rgba(0, 0, 0, 0.75);"></font>

**<font style="color:rgba(0, 0, 0, 0.75);">获取 GithubAction 工作流 ID 接口：</font>**

**<font style="color:rgba(0, 0, 0, 0.75);">url: </font>**https://api.github.com/repos/你的用户名/你的项目名/actions/workflows

method: get

header:

| Authorization | token 你的 token               |
| ------------- | ------------------------------ |
| Accept        | application/vnd.github.v3+json |

body: {ref:master'}

**<font style="color:rgba(0, 0, 0, 0.75);">PHP 请求案例：</font>**

```php
// Generated by ApiPost: https://www.apipost.cn/
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, 'https://api.github.com/repos/你的用户名/你的项目名/actions/workflows');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'GET');

curl_setopt($ch, CURLOPT_POSTFIELDS, "{ref:master'}'");

$headers = array();
$headers[] = 'User-Agent: Apipost client Runtime/+https://www.apipost.cn/';
$headers[] = 'Authorization: token 你的TOKEN';
$headers[] = 'Accept: application/vnd.github.v3+json';
$headers[] = 'Content-Type: application/json';
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$result = curl_exec($ch);
if (curl_errno($ch)) {
    echo 'Error:' . curl_error($ch);
}
curl_close($ch);

```

<font style="color:rgba(0, 0, 0, 0.75);">Python 请求案例：</font>

```python
import requests

headers = {
    'User-Agent': 'Apipost client Runtime/+https://www.apipost.cn/',
    'Authorization': 'token 你的token',
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type': 'application/json',
}

data = '{ref:master}'

response = requests.post('https://api.github.com/repos/你的用户名/你的项目名/actions/workflows', headers=headers, data=data)

```

<font style="color:rgba(0, 0, 0, 0.75);"></font>

9. <font style="color:rgba(0, 0, 0, 0.75);">具体填写如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649401818211-43e43680-9183-420c-af31-9011030b7073.png)<font style="color:rgba(0, 0, 0, 0.75);">  
</font>

10. <font style="color:rgba(0, 0, 0, 0.75);">在高级配置中，将执行超过时间设置为合适的时间，这里我设置为最大值</font>**<font style="color:rgba(0, 0, 0, 0.75);">900</font>**<font style="color:rgba(0, 0, 0, 0.75);">秒：</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649390134784-9f7e3870-5676-4dab-b85e-fffd90989239.png)

11. <font style="color:rgba(0, 0, 0, 0.75);">触发器配置选择默认流量 - 定时触发 - 自定义触发周期，并填入合适的 Cron 表达式，这里的</font>**<font style="color:rgba(0, 0, 0, 0.75);">Cron</font>**<font style="color:rgba(0, 0, 0, 0.75);">当前以 UTC +8 中国标准时间 （China Standard Time）运行，即北京时间。我输入的：</font>

> 0 0 16 \* \* \* \*

<font style="color:rgb(77, 77, 77);">表示了每天 16 点执行 1 次。</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649390185893-a04dcb9c-cb6d-4c29-825e-91b9c250b807.png)

触发器也可以选择 api 调用，创建好后会生成一个 api url （webhook 地址）。粘贴到对应的位置调用即可。
