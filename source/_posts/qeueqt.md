---
title: 使用nodejs 打包网址为exe端
urlname: qeueqt
date: '2022-04-14 01:38:43 +0000'
tags:
  - nodejs
  - 打包
  - exe
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

# 安装

```shell
npm install nativefier -g
```

# 使用

<font style="color:rgb(82, 82, 82);">设置 npm 源为</font>

```shell
npm config set registry https://registry.npm.taobao.org/
```

<font style="color:rgb(82, 82, 82);">在 nativefier 后加上需要转换的网站地址, 比如:</font>

```shell
 nativefier "https://www.zhihu.com/"
```

<font style="color:rgb(82, 82, 82);">第一次打包需要下载 Eletron 框架, 很慢, 要有耐心......</font>

```shell
nativefier --name "恒星C端" "https://www.toushivip.com/tshxs"
nativefier --name "恒星C端" "C:\Users\Administrator\Desktop\icon.ico"
```

```shell
nativefier –name “blog” “https://www.leixuesong.cn/”

nativefier –icon <path>：设置图标
icon参数
Windows环境下为.ico文件
Linux下为.png
Mac下 icon参数可以是a .icns或.png文件

--app-copyright ：应用的版权信息

-p, --platform <value>：指定输出不同系统的应用，可选参数linux、windows、osx。

-m, –show-menu-bar：指定是否应该显示菜单栏。

--disable-context-menu：禁用上下文菜单

--disable-dev-tools:停用Chrome开发者工具

--clear-cache:防止应用程序在两次启动之间保留缓存。

--tray:托盘,防止用户点击右上角关闭按钮后直接关闭程序，而是缩小到右下角的托盘中。

--always-on-top：总是在最前面显示。

--maximize：开始的时候最大化。

--full-screen：使打包的应用全屏启动。

--app-version <value>：应用程序的发行版本。

–width <value>：打包应用程序的宽度，默认为1280px。

–height <value>：打包应用程序的高度，默认为800px。

–min-width <value>：打包应用程序的最小宽度，默认为0。

–min-height <value>：打包应用程序的最小高度，默认为0。

–max-width <value>：打包应用程序的最大宽度，默认为无限制。

–max-height <value>：打包应用程序的最大高度，默认为无限制。

–x <value>：打包的应用程序窗口的X位置。

–y <value>：打包的应用程序窗口的Y位置。

-a, --arch <value> 处理器架构

示例：

 nativefier
	--arch "x64"
	--platform "windows"
	--icon D:\temp\favicon.ico //一定要有图片，不然会报错
	--name "weixin"
	"https://mp.weixin.qq.com/" //网站地址
	--maximize	//开始最大化
	--always-on-top //最前端显示
	--clear-cache  //防止缓存
	--app-copyright "在这里填自己的就行了，也没找到在哪里显示"
	--app-version 1  //这里好像只能填数字，不过也没啥用，没找到在哪里显示
	--show-menu-bar //英文的，感觉没啥用，还挺丑
	--disable-dev-tools
	--tray //比较有用的
	D:\temp\  //最后指定文件的输出目录

	cmd不能换行执行一句，会出错...

	nativefier --arch "x64" --platform "windows" --icon D:\temp\favicon.ico --name "weixin" "https://mp.weixin.qq.com/" --maximize --app-copyright "微信公众号" --app-version 1 --show-menu-bar --disable-dev-tools --tray D:\temp\


```
