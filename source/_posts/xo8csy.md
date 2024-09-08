---
title: 使用Hexo搭建博客
urlname: xo8csy
date: '2022-04-08 05:39:46 +0000'
tags:
  - hexo
  - 博客
  - 安装
categories:
  - 学无止境
cover: >-
  https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fgitee.com%2Fwangbowen97%2FBlogImgs%2Fraw%2Fmaster%2FpostImages%2FHexoCover.jpg&refer=http%3A%2F%2Fgitee.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1652059164&t=ac7f746ad7621718ece964ce547ba635
copyright_author_href: 'https://www.xiaohuihui.<font style="color:rgb(38, 38, 38);">cc</font>'
---

# 一、安装 node.js

- [node.js 官方下载地址](https://nodejs.org/en/)
- <font style="color:rgba(0, 0, 0, 0.75);">从上面的链接下载 node.js，并安装。</font>
  - <font style="color:rgba(0, 0, 0, 0.75);">注意：官方链接可能需要</font>**<font style="color:rgba(0, 0, 0, 0.75);">fanqiang</font>**
  - <font style="color:rgba(0, 0, 0, 0.75);">注意：我的操作系统是</font>**<font style="color:rgba(0, 0, 0, 0.75);">Windows 7 (64bit)</font>**

## <font style="color:rgb(79, 79, 79);">设置 npm 淘宝镜像站</font>

- <font style="color:rgba(0, 0, 0, 0.75);">npm 默认的源的下载速度可能很慢，建议使用淘宝镜像替换。</font>
- <font style="color:rgba(0, 0, 0, 0.75);">执行下面的命令，将 npm 的源设置成淘宝镜像站。</font>

```plain
npm config set registry "https://registry.npm.taobao.org"
```

# <font style="color:rgb(79, 79, 79);">二、安装 hexo</font>

- <font style="color:rgba(0, 0, 0, 0.75);">执行以下命令安装 hexo。</font>

```plain
# 安装hexo
npm install hexo-cli g
# 初始化博客文件夹
hexo init blog
# 切换到该路径
cd blog
# 安装hexo的扩展插件
npm install
# 安装其它插件
npm install hexo-server --save
npm install hexo-admin --save
npm install hexo-generator-archive --save
npm install hexo-generator-feed --save
npm install hexo-generator-search --save
npm install hexo-generator-tag --save
npm install hexo-deployer-git --save
npm install hexo-generator-sitemap --save

```

初探 hexo  
第一次使用 hexo，在本地创建服务器使用。

# 生成静态页面

hexo generate

# 三、开启本地服务器

```plain
# 生成静态页面
hexo generate
# 开启本地服务器
hexo s
```

打开浏览器，地址栏中输入：http://localhost:4000/,应该可以看见刚刚创建的博客了。  
问题：为什么访问 http://localhost:4000/，无反应？  
解决方法：可能是由于端口问题引起的。使用 Ctrl+C 中断本地服务，使用命令 hexo s -p 5000 重新开启本地服务，访问[**http://localhost:5000/**](http://localhost:5000/)可以看到博客页面了。

# <font style="color:rgb(79, 79, 79);">四、hexo 命令缩写</font>

- <font style="color:rgba(0, 0, 0, 0.75);">hexo 支持命令缩写，如下所示。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hexo g</font><font style="color:rgba(0, 0, 0, 0.75);">等价于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hexo generate</font>

```plain
hexo g：hexo generate
hexo c：hexo clean
hexo s：hexo server
hexo d：hexo deploy
```

# <font style="color:rgb(79, 79, 79);">hexo 组合命令</font>

```plain
# 清除、生成、启动
hexo clean && hexo g -s
# 清除、生成、部署
hexo clean && hexo g -d
```

# <font style="color:rgb(79, 79, 79);">常见问题</font>

## <font style="color:rgb(79, 79, 79);">hexo deploy 没有反应？</font>

- <font style="color:rgba(0, 0, 0, 0.75);">修改配置文件：</font>**<font style="color:rgba(0, 0, 0, 0.75);">\_config.yml</font>**<font style="color:rgba(0, 0, 0, 0.75);">时，冒号后面没加空格。</font>

## <font style="color:rgb(79, 79, 79);">hexo s 网站打不开？</font>

- <font style="color:rgba(0, 0, 0, 0.75);">端口占用，换个端口就好了。执行命令</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hexo s -p 5000</font><font style="color:rgba(0, 0, 0, 0.75);">，并在浏览器地址栏输入 http://localhost:5000，回车访问。</font>

## <font style="color:rgb(79, 79, 79);">如何换主题？</font>

- <font style="color:rgba(0, 0, 0, 0.75);">将主题下载后，放到 themes 文件夹中即可。例如，下面命令安装 next 主题：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">git clone https://github.com/iissnan/hexo-theme-next themes/next</font><font style="color:rgba(0, 0, 0, 0.75);">。</font>
