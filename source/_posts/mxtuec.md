---
title: PHP获取端操作系统类型 客户端和服务器端
urlname: mxtuec
date: '2022-04-12 10:12:44 +0000'
tags:
  - php
  - 操作系统类型
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

<font style="color:rgb(83, 104, 121);">如何使用</font>[php](https://xiaohuihui.net.cn/archives/tag/php/)<font style="color:rgb(83, 104, 121);">获取当前操作系统类型呢？严格来说这里分两种情况:</font>

<font style="color:rgb(83, 104, 121);">一种情况是获取服务器端的操作系统类型，</font>

<font style="color:rgb(83, 104, 121);">一种是获取客户端的操作系统类型。</font>

<font style="color:rgb(83, 104, 121);">下面作者将对如何使用</font>[php](https://xiaohuihui.net.cn/archives/tag/php/)<font style="color:rgb(83, 104, 121);">获取这两种情况下的操作系统类型和大家做一些分享。</font>

<font style="color:rgb(83, 104, 121);">（1）php 获取服务器端的操作系统类型</font>

<font style="color:rgb(83, 104, 121);">这个时候可以使用 php 系统自带的常量 PHP_OS 或者系统函数 php_uname('s')。关于这两者返回的值可能的情况基本有如下几种情况：</font>

- <font style="color:rgb(153, 153, 153);">CYGWIN_NT-5.1</font>
- <font style="color:rgb(153, 153, 153);">Darwin</font>
- <font style="color:rgb(153, 153, 153);">FreeBSD</font>
- <font style="color:rgb(153, 153, 153);">HP-UX</font>
- <font style="color:rgb(153, 153, 153);">IRIX64</font>
- <font style="color:rgb(153, 153, 153);">Linux</font>
- <font style="color:rgb(153, 153, 153);">NetBSD</font>
- <font style="color:rgb(153, 153, 153);">OpenBSD</font>
- <font style="color:rgb(153, 153, 153);">SunOS</font>
- <font style="color:rgb(153, 153, 153);">Unix</font>
- <font style="color:rgb(153, 153, 153);">WIN32</font>
- <font style="color:rgb(153, 153, 153);">WINNT</font>
- <font style="color:rgb(153, 153, 153);">Windows</font>
- <font style="color:rgb(153, 153, 153);">CYGWIN_NT-5.1</font>
- <font style="color:rgb(153, 153, 153);">IRIX64</font>
- <font style="color:rgb(153, 153, 153);">SunOS</font>
- <font style="color:rgb(153, 153, 153);">HP-UX</font>
- <font style="color:rgb(153, 153, 153);">OpenBSD</font>

<font style="color:rgb(83, 104, 121);">不过根据具体情况读者还是自行打印出来结果看看最好，也许获得的结果不在上述之列。</font>

<font style="color:rgb(83, 104, 121);">（2）php 获取客户端的操作系统类型，这里分享一个函数，比网上流传的那些判断更加精准，而且没有 bug</font>

<font style="color:rgb(83, 104, 121);">代码如下：</font>

<font style="color:rgb(83, 104, 121);"></font>

```php
function get_os(){
    $os='';
    $Agent=$_SERVER['HTTP_USER_AGENT'];
    if (eregi('win',$Agent)&&strpos($Agent, '95')){
        $os='Windows 95';
    }elseif(eregi('win 9x',$Agent)&&strpos($Agent, '4.90')){
        $os='Windows ME';
    }elseif(eregi('win',$Agent)&&ereg('98',$Agent)){
        $os='Windows 98';
    }elseif(eregi('win',$Agent)&&eregi('nt 5.0',$Agent)){
        $os='Windows 2000';
    }elseif(eregi('win',$Agent)&&eregi('nt 6.0',$Agent)){
        $os='Windows Vista';
    }elseif(eregi('win',$Agent)&&eregi('nt 6.1',$Agent)){
        $os='Windows 7';
    }elseif(eregi('win',$Agent)&&eregi('nt 5.1',$Agent)){
        $os='Windows XP';
    }elseif(eregi('win',$Agent)&&eregi('nt',$Agent)){
        $os='Windows NT';
    }elseif(eregi('win',$Agent)&&ereg('32',$Agent)){
        $os='Windows 32';
    }elseif(eregi('linux',$Agent)){
        $os='Linux';
    }elseif(eregi('unix',$Agent)){
        $os='Unix';
    }else if(eregi('sun',$Agent)&&eregi('os',$Agent)){
        $os='SunOS';
    }elseif(eregi('ibm',$Agent)&&eregi('os',$Agent)){
        $os='IBM OS/2';
    }elseif(eregi('Mac',$Agent)&&eregi('PC',$Agent)){
        $os='Macintosh';
    }elseif(eregi('PowerPC',$Agent)){
        $os='PowerPC';
    }elseif(eregi('AIX',$Agent)){
        $os='AIX';
    }elseif(eregi('HPUX',$Agent)){
        $os='HPUX';
    }elseif(eregi('NetBSD',$Agent)){
        $os='NetBSD';
    }elseif(eregi('BSD',$Agent)){
        $os='BSD';
    }elseif(ereg('OSF1',$Agent)){
        $os='OSF1';
    }elseif(ereg('IRIX',$Agent)){
        $os='IRIX';
    }elseif(eregi('FreeBSD',$Agent)){
        $os='FreeBSD';
    }elseif($os==''){
        $os='Unknown';
    }
    return $os;
}
```
