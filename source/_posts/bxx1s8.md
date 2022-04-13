---
title: composer 默认地址改为中国镜像地址
urlname: bxx1s8
date: '2022-04-12 10:05:31 +0000'
tags:
  - composer
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

composer 默认地址改为中国镜像地址，以及中国镜像地址还原成默认地址

### 一、查看当前镜像地址

在命令行输入如下命令，即可查看全局镜像地址：
$ composer config -g repo.packagist

**PHP**

```
{
"type":"composer",
"url":"https://packagist.org",
"allow_ssl_downgrade":true
}
```

下面有把地址修改为中国镜像，如果中国镜像出现了问题，那么您可以还原成官方的默认地址，下面是详细。

### 二、启用中国全量镜像服务：

启用中国全量镜像服务有两种方式，具体配置方法如下：

##### 系统全局配置：

可以使用 composer config -l -g 查看所有全局配置
composer config -l -g
**使用如下命令将地址改为中国镜像地址：**

> composer config -g repo.packagist composer [https://packagist.phpcomposer.com](https://packagist.phpcomposer.com)

**中国镜像地址还原成默认地址：（注意：这个是将中国镜像还原）**

> composer config -g repo.packagist composer [https://packagist.phpcomposer.com](https://packagist.phpcomposer.com)

##### 单个项目配置：

在当前项目根目录可以使用 composer config -l 查看当前项目镜像配置

> composer config -l

即将将配置信息添加到某个项目的 composer.[js](https://xiaohuihui.net.cn/archives/tag/js/)on 文件中。修改当前项目的 composer.json 配置文件有两种方式，最后都是向文件中添加如下配置信息：

```
"repositories": {
"packagist": {
   "type": "composer",
   "url": "https://packagist.phpcomposer.com"
}
}
```

2.1 打开命令行并进入项目的根目录（也就是 composer.json 文件所在目录），执行如下命令：
将当前项目地址改为中国镜像地址：

```
composer config repo.packagist composer https://packagist.phpcomposer.com
```

该命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息
将当前项目中国镜像地址还原成默认地址：（注意：这个是将中国镜像还原）

```
composer config repo.packagist composer https://packagist.org
```

2.2 手动向 composer.json 文件中添加以上信息
默认地址改为中国镜像地址：

```
"repositories": {
   "packagist": {
       "type": "composer",
       "url": "https://packagist.phpcomposer.com"
   }
}
```

中国镜像地址还原成默认地址：（注意：这个是将中国镜像还原）
将 url 的值改为：[https://packagist.org](https://xiaohuihui.net.cn/wp-content/themes/begin/inc/go.php?url=https://packagist.org/)
