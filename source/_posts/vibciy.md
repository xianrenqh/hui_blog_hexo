---
title: 语雀+Hexo+serverless云函数自动同步
urlname: vibciy
date: '2022-04-08 05:37:45 +0000'
tags:
  - 语雀
  - 云函数
  - 自动发布
  - hexo
categories:
  - 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

# 一、搭建 Hexo 博客

本文针对已经搭建好 hexo 博客的，如果没有搭好正常的 hexo 博客的可以去看另一篇博文，搭建很方便的。
使用 hexo 搭建博客地址：
[https://www.yuque.com/xianrenqh/kyeqhg/xo8csy](https://www.yuque.com/xianrenqh/kyeqhg/xo8csy)

# 二、Hexo 同步语雀内容

用到了这个项目：

> https://github.com/x-cold/yuque-hexo

> 安装：npm i -g yuque-hexo

然后把 package.json 的内容添加上下面这些：

```json
  "yuqueConfig": {
    "postPath": "source/_posts",
    "cachePath": "yuque.json",
    "mdNameFormat": "slug",
    "adapter": "hexo",
    "concurrency": 5,
    "baseUrl": "https://www.yuque.com/api/v2",
    "login": "hxfqg9",
    "repo": "web",
    "token": "语雀token",
    "onlyPublished": true,
    "onlyPublic": true
  },
  "devDependencies": {
    "yuque-hexo": "^1.6.0"
  },
  "hexo": {
    "version": "4.2.1"
  },

```

这里需要说明：
baseurl 是固定的
login 和 repo 是如下图这样对应的，个人界面和团队界面都可以
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649396601747-fc1e332a-2b6a-4ad0-bf0b-85b45608f17b.png#clientId=udcfe64c5-982a-4&from=paste&id=u0d64e7b4&originHeight=545&originWidth=1080&originalType=url∶=1&rotation=0&showTitle=false&size=189765&status=done&style=none&taskId=u57aea17f-fb57-435f-8a44-af6dbe6c1cc&title=)

token 是在右上角头像 -> 账户设置 -> Token 添加的，权限的话只给读取就可以
ps.公开的知识库也要设置 Token。
接着，
在 "scripts" 中添加

```
    "sync": "yuque-hexo sync",
    "clean:yuque": "yuque-hexo clean",
```

这样整体下来我的 package.json 内容如下（参考）：

```json
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "build": "hexo generate",
    "clean": "hexo clean",
    "deploy": "hexo deploy",
    "server": "hexo server",
    "sync": "yuque-hexo sync",
    "clean:yuque": "yuque-hexo clean"
  },
  "yuqueConfig": {
    "postPath": "source/_posts",
    "cachePath": "yuque.json",
    "mdNameFormat": "slug",
    "adapter": "hexo",
    "concurrency": 5,
    "baseUrl": "https://www.yuque.com/api/v2",
    "login": "hxfqg9",
    "repo": "web",
    "token": "语雀token",
    "onlyPublished": true,
    "onlyPublic": true
  },
  "devDependencies": {
    "yuque-hexo": "^1.6.0"
  },
  "hexo": {
    "version": "4.2.1"
  },
  "dependencies": {
    "hexo": "^4.2.1",
    "hexo-deployer-git": "^2.1.0",
    "hexo-generator-archive": "^1.0.0",
    "hexo-generator-baidu-sitemap": "^0.1.6",
    "hexo-generator-category": "^1.0.0",
    "hexo-generator-feed": "^2.2.0",
    "hexo-generator-index": "^1.0.0",
    "hexo-generator-json-content": "^4.2.3",
    "hexo-generator-searchdb": "^1.3.1",
    "hexo-generator-sitemap": "^2.0.0",
    "hexo-generator-tag": "^1.0.0",
    "hexo-renderer-ejs": "^1.0.0",
    "hexo-renderer-marked": "^2.0.0",
    "hexo-renderer-stylus": "^1.1.0",
    "hexo-server": "^1.0.0",
    "hexo-wordcount": "^6.0.1"
  }
}
```

这时候，在本地调试的时候使用命令：yuque-hexo sync 就会把语雀的文章给下载下来，下载到 \source_posts
（使用以上命令前先备份本地的\_posts 文件夹下的 md 文章，使用语雀下载的时候会先清空此文件夹）
然后 hexo g && hexo s 就可以访问 127.0.0.1:4000 本地看一下了
手动发布是 hexo g && hexo d

### 针对语雀图片无法正常显示的解决办法

在主题的 layout 文件夹中的 post.ejs 文件中加上一句（不同主题加的位置不同）

```
<meta name="referrer" content="no-referrer" />
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649404362200-7eafd317-d235-4af8-afe9-4f9c54d4dee2.png#clientId=u68da2bd8-69aa-4&from=paste&id=u4d9c66af&originHeight=351&originWidth=841&originalType=url∶=1&rotation=0&showTitle=false&size=46710&status=done&style=none&taskId=ufd914241-d133-43ab-b845-e08191199df&title=)

小灰灰用的是 butterFly 主题，加的位置在：
\themes\butterfly\layout\includes\head.pug 文件：
大概在第 40 行左右添加：
（PS：放在这个位置是因为百度统计的 referrer 问题。只能放在统计后面）

```
meta(name="referrer" content="no-referrer")
#如果上面不生效，提示 referrer被禁用，可以使用以下代码试试
meta(name="referrer" content="no-referrer")
```

如图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649641758436-1971a415-4efc-45ff-87b9-1e47f9617353.png#clientId=u6e1ca8df-731b-4&from=paste&height=700&id=u651955ce&originHeight=773&originWidth=1119&originalType=binary∶=1&rotation=0&showTitle=false&size=134271&status=done&style=none&taskId=u1edc2856-0d56-4f15-9454-c218fcf937a&title=&width=1013.4339987353198)

# **三、github actions 自动更新**

在 github 上创建一个私有仓库（因为会涉及到一些 token 啥的）仓库名字无所谓**（用来存放 hexo 源码）**
**注意**：在仓库里面再放一个仓库是没法把里面那个仓库 push 到 github 的，只会传一个空文件夹，导致后期博客成了空白页面，最简单粗暴的办法就是把你 git clone 的 hexo 主题里的 .git 文件夹给删掉
然后在 hexo 的目录下运行如下命令，把 hexo 的源码传到 github 远程仓库中

```git
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/yichen115/blog.git
git push -u origin master

```

去 github 的 settings 创建一个 token （ps:个人中心的设置，非项目的设置）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649396898326-2a09cbe8-9587-4f19-9f13-ead9e3bd6fc1.png#clientId=udcfe64c5-982a-4&from=paste&id=uac4e5e7e&originHeight=390&originWidth=1080&originalType=url∶=1&rotation=0&showTitle=false&size=89765&status=done&style=none&taskId=uc2174048-c4a7-45e5-8b7e-a72c318831b&title=)

只勾上这一个即可：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649396909568-f0df156b-9310-4954-9350-da49c64404d2.png#clientId=udcfe64c5-982a-4&from=paste&id=u1e211b98&originHeight=627&originWidth=746&originalType=url∶=1&rotation=0&showTitle=false&size=53298&status=done&style=none&taskId=ua795b964-947a-41ce-bafa-a59b4ecd46b&title=)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649396921676-0e84a878-d94d-44d7-bd5b-0d1490dd65c6.png#clientId=udcfe64c5-982a-4&from=paste&id=u4cbbdb00&originHeight=445&originWidth=1015&originalType=url∶=1&rotation=0&showTitle=false&size=45407&status=done&style=none&taskId=u4adc989a-ec2a-4dcc-9a1d-66a0017e2b1&title=)

**生成了 token 之后一定要记下来**，再回来就没法看了
然后来到刚才创建的私有仓库的 settings
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649396944906-f483bf94-9454-4d5b-8fcb-5ad1d02e9003.png#clientId=udcfe64c5-982a-4&from=paste&id=u71d114fa&originHeight=419&originWidth=1080&originalType=url∶=1&rotation=0&showTitle=false&size=91140&status=done&style=none&taskId=u2e8da352-5627-4745-98cb-0fad22bf7af&title=)

添加三个 secret
**GH_REF** 是你博客的仓库地址 github.com/**\*\***/**\*\***.github.io
注意去掉前面 https://
**GE_TOKEN** 是刚才生成的 token
然后来到 actions，点击 set up a workflow yourself
**HEXO_DEPLOY_PRI **是新增的 ssh 密钥
在命令行中输入：

> ssh-keygen -t rsa -C "your_email@example.com"

备注：
同时生成的公钥，要放到 Hexo 生成 page 的项目的公钥下面
（只有密钥和公钥相互匹配，才能推送静态文件到公开的项目下（page 项目））

编辑内容如下：

```yaml
name: Hexo Deploy

on:
  workflow_dispatch:
  push:
    branches:
      - master
  schedule:
    - cron: "0 21 * * 0" # 每周日 UTC 时间 21:00, 北京时间周一凌晨5点

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source # 将仓库内master分支的内容下载到工作目录1
        uses: actions/checkout@v2 # 脚本来自 https://github.com/actions/checkout
        with:
          ref: master
          submodules: "true"

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: "14.x"

      - name: Checkout Submodules
        run: |
          auth_header="$(git config --local --get http.https://github.com/.extraheader)"
          git submodule sync --recursive
          git -c "http.extraheader=$auth_header" -c protocol.version=2 submodule update --init --force --recursive --depth=1

      - name: Setup Hexo
        env:
          HEXO_DEPLOY_PRI: ${{ secrets.HEXO_DEPLOY_PRI }}
        run: |
          mkdir -p ~/.ssh/
          echo "$HEXO_DEPLOY_PRI" > ~/.ssh/id_rsa
          chmod 700 ~/.ssh
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.email "xianrenqh@163.com"
          git config --global user.name "xianrenqh"
          npm install hexo-cli -g
          npm install yuque-hexo -g
          yuque-hexo clean
          yuque-hexo sync

      - name: Setup Yuque #更新yuque 拉取的文章到GitHub仓库
        run: |
          git config --global user.email "xianrenqh@163.com"
          git config --global user.name "xianrenqh"
          git add .
          git commit -m "Refresh yuque json" -a

      - name: Push Yuque #推送修改后的yuque json
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GE_TOKEN }}

      - name: Cache Hexo
        uses: actions/cache@v1
        id: cache
        with:
          path: node_modules
          key: ${{runner.OS}}-${{hashFiles('**/package-lock.json')}}

      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: |
          npm install --save

      - name: Hexo Deploy
        run: |
          hexo clean
          hexo generate
          hexo deploy
```

下面那个 user.name 和 user.email 根据自己的情况改一下，注意对齐
弄完之后每当 push 或 repository_dispatch 的时候都会自动的进行更新

# 四、配置 ServerLess 云函数

[Github Action 使用云函数调度服务](https://www.yuque.com/xianrenqh/kyeqhg/awh8vd?view=doc_embed)

# 五、发布博文到服务器（vps）上

小灰灰使用的是 webhooks 的功能同步推送到服务器上的

# ⑥、PS：

小灰灰的 Hexo 博客地址：
https://www.xiaohuihui.net

PS：
听说使用 github action 执行 ssh 可能会出现封号的情况。
另外同步到 github 速度比较慢，
所以小灰灰的博客代码直接使用自己的服务器搭建的，然后加个 webhook 到语雀上，这样语雀更新之后请求 webhook 执行命令直接在服务器上更新了。
宝塔的 webhook 代码：

```
#!/bin/bash
echo ""
#输出当前时间
date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"
echo "Start"

cd "/www/wwwroot/node_js/你的Hexo博客源码文件夹/"
echo "清除缓存"
npx hexo clean

echo "开始下载语雀文章....."
npx yuque-hexo clean
npx yuque-hexo sync

echo "创建文章"
npx hexo g

gitPath="/www/wwwroot/node_js/你的Hexo博客源码文件夹/public"

if [ -d "$gitPath" ]; then
  # 需要先编辑文件
  #vi ~/.bashrc，在alias cp=’cp -i’前加上”#”注释掉这行，:wq! 保存退出，然后重新登陆，使用cp -r -f就可以了
   cp -r -f /www/wwwroot/node_js/你的Hexo博客源码文件夹/public/* /www/wwwroot/你要运行的静态文件夹

echo "完成"
exit
fi
```

> 服务器需要改动，要求 copy 命令的时候不要提示询问。改动方法：
> 执行：vi ~/.bashrc
> 在 alias cp=’cp -i’前加上”#”注释掉这行，:wq! 保存退出，然后重新登陆。
> 最后使用 cp -r -f 就可以了
