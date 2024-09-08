---
title: composer 默认地址改为中国镜像地址
urlname: bxx1s8
date: '2022-04-12 10:05:31 +0000'
tags:
  - composer
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

<font style="color:rgb(83, 104, 121);">composer 默认地址改为中国镜像地址，以及中国镜像地址还原成默认地址</font>

### 一、查看当前镜像地址

<font style="color:rgb(83, 104, 121);">在命令行输入如下命令，即可查看全局镜像地址：</font>

<font style="color:rgb(125, 163, 60);">$ composer config -g repo.packagist</font>

**<font style="color:rgb(153, 153, 153) !important;background-color:rgb(221, 221, 221) !important;">PHP</font>**

```plain
{
"type":"composer",
"url":"https://packagist.org",
"allow_ssl_downgrade":true
}
```

<font style="color:rgb(83, 104, 121);"></font>

<font style="color:rgb(83, 104, 121);">下面有把地址修改为中国镜像，如果中国镜像出现了问题，那么您可以还原成官方的默认地址，下面是详细。</font>

### 二、启用中国全量镜像服务：

<font style="color:rgb(83, 104, 121);">启用中国全量镜像服务有两种方式，具体配置方法如下：</font>

##### <font style="color:rgb(83, 104, 121);">系统全局配置：</font>

<font style="color:rgb(83, 104, 121);">可以使用 composer config -l -g 查看所有全局配置</font>

<font style="color:rgb(125, 163, 60);">composer config -l -g</font>

**<font style="color:rgb(255, 0, 0);">使用如下命令将地址改为中国镜像地址：</font>**

> composer config -g repo.packagist composer [https://packagist.phpcomposer.com](https://packagist.phpcomposer.com)

**<font style="color:rgb(255, 0, 0);">中国镜像地址还原成默认地址：（注意：这个是将中国镜像还原）</font>**

> composer config -g repo.packagist composer [https://packagist.phpcomposer.com](https://packagist.phpcomposer.com)

##### <font style="color:rgb(83, 104, 121);">单个项目配置：</font>

<font style="color:rgb(83, 104, 121);">在当前项目根目录可以使用 composer config -l 查看当前项目镜像配置</font>

> composer config -l

<font style="color:rgb(83, 104, 121);">即将将配置信息添加到某个项目的 composer.</font>[js](https://xiaohuihui.net.cn/archives/tag/js/)<font style="color:rgb(83, 104, 121);">on 文件中。修改当前项目的 composer.json 配置文件有两种方式，最后都是向文件中添加如下配置信息：</font>

```plain
"repositories": {
"packagist": {
   "type": "composer",
   "url": "https://packagist.phpcomposer.com"
}
}
```

<font style="color:rgb(83, 104, 121);"></font>

<font style="color:rgb(83, 104, 121);">2.1 打开命令行并进入项目的根目录（也就是 composer.json 文件所在目录），执行如下命令：</font>

<font style="color:rgb(125, 163, 60);">将当前项目地址改为中国镜像地址：</font>

```plain
composer config repo.packagist composer https://packagist.phpcomposer.com
```

<font style="color:rgb(83, 104, 121);">该命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息</font>

<font style="color:rgb(125, 163, 60);">将当前项目中国镜像地址还原成默认地址：（注意：这个是将中国镜像还原）</font>

```plain
composer config repo.packagist composer https://packagist.org
```

<font style="color:rgb(83, 104, 121);">2.2 手动向 composer.json 文件中添加以上信息</font>

<font style="color:rgb(83, 104, 121);">默认地址改为中国镜像地址：</font>

```plain
"repositories": {
   "packagist": {
       "type": "composer",
       "url": "https://packagist.phpcomposer.com"
   }
}
```

<font style="color:rgb(83, 104, 121);"></font>

<font style="color:rgb(83, 104, 121);">中国镜像地址还原成默认地址：（注意：这个是将中国镜像还原）  
</font><font style="color:rgb(83, 104, 121);">将 url 的值改为：</font>[https://packagist.org](https://xiaohuihui.net.cn/wp-content/themes/begin/inc/go.php?url=https://packagist.org/)
