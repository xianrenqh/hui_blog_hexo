---
title: 宝塔使用webhook更新服务器代码
urlname: wwsg5w
date: '2022-04-14 06:34:00 +0000'
tags:
  - webhook
  - 宝塔
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

# <font style="color:rgb(82, 82, 82);">安装 git</font>

```git
yum install git
```

# <font style="color:rgb(82, 82, 82);">安装 webhook 插件</font>

<font style="color:rgb(82, 82, 82);">添加 shell 脚本，如上图，点击添加，数据名称 和 执行脚本(此处执行脚本框中 直接输入 shell 脚本可能会被过滤，所以可以先随便添加点东西，然后再重新添加 shell 脚本)，shell 脚本如下</font>

```shell
#!/bin/bash
echo ""
#输出当前时间
date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"
echo "Start"
#判断宝塔WebHook参数是否存在
if [ ! -n "$1" ]; then
                echo "param参数错误"
        echo "End"
        exit
fi
#git项目路径
gitPath="/www/wwwroot/$1"
#git 网址
gitHttp="https://gitee.com/xianrenqh/huicmf_tp6"

echo "Web站点路径：$gitPath"

#判断项目路径是否存在

if [ -d "$gitPath" ]; then
        cd $gitPath
        #判断是否存在git目录
        if [ ! -d ".git" ]; then
                echo "在该目录下克隆 git"
                git clone $gitHttp gittemp
                mv gittemp/.git .
rm -rf gittemp
        fi
        #拉取最新的项目文件
        #git reset --hard origin/2.0
        git pull
        #设置目录权限
        #chown -R www:www $gitPath
        echo "End"
        exit
else
        echo "该项目路径不存在"
        echo "End"
        exit
fi

```

<font style="color:rgb(82, 82, 82);">点击查看密钥</font>

> <font style="color:rgb(82, 82, 82);">获取地址：http://面板/hook?...</font>

# <font style="color:rgb(82, 82, 82);">添加公钥</font>

<font style="color:rgb(82, 82, 82);">如何生成 public key</font>

```git
cd root/.ssh
cat id\_rsa.pub
ssh-keygen
cat id\_rsa.pub
```

<font style="color:rgb(82, 82, 82);">\*\*</font>

<font style="color:rgb(82, 82, 82);">第一步，检查本机是否存在 SSH key</font>

<font style="color:rgb(82, 82, 82);">如下图调出 Git Bash 窗口，输入下面的命令 ls -al ~/.ssh ，如果有文件</font><font style="color:rgb(82, 82, 82);background-color:rgb(247, 247, 247);">id_rsa.pub</font><font style="color:rgb(82, 82, 82);"> 或 </font><font style="color:rgb(82, 82, 82);background-color:rgb(247, 247, 247);">id_dsa.pub</font><font style="color:rgb(82, 82, 82);">，则直接进入步骤 3 将 SSH key 添加到 GitHub 中，否则进入第二步生成 SSH key</font>

<font style="color:rgb(82, 82, 82);"></font>

<font style="color:rgb(82, 82, 82);">第二步：  
</font><font style="color:rgb(82, 82, 82);">在命令行中输入 ssh-keygen -t rsa -C</font><font style="color:rgb(82, 82, 82);"> </font>["your_emial@examle.com](mailto:%22your_emial@examle.com)<font style="color:rgb(82, 82, 82);">"</font>

<font style="color:rgb(82, 82, 82);">默认会在相应路径下（/c/Users/Administrator/.ssh/id_rsa）生成 id_rsa 和 id_rsa.pub 、known_hosts 三个文件，如下面代码所示</font>

```shell
$ ssh-keygen -t rsa -C "your_emial@examle.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
/c/Users/Administrator/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa.
Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:X.........................oo your_emial@examle.com
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|       o .       |
|      = +        |
|     . * o       |
|  o o = S .      |
| = = = + .       |
|o B = + o        |
|o= = =B=.*       |
|E.o ++=@O.o      |
+----[SHA256]-----+
```

<font style="color:rgb(82, 82, 82);">在码云中进行对应操作，找到对应项目，在'Webhooks 设置'右侧点击添加，然后输入地址，默认选中 Push，密码为空，提交。</font>

<font style="color:rgb(82, 82, 82);"></font>

<font style="color:rgb(82, 82, 82);"></font>

<font style="color:rgb(82, 82, 82);">在登录服务器相关目录（wwwroot）用 git 命令 克隆一般码云中的代码：</font>

```git
git clone https://gitee.com/xianrenqh/huicmf_tp6
```

<font style="color:rgb(82, 82, 82);">拉取项目代码测试一下（首次需要执行前两行命令）master 为分支名称</font>

```git
git init
git remote add origin "你的码云或coding项目地址（ssh或https）"
git pull origin master
```

<font style="color:rgb(82, 82, 82);">解决每次都需要输入密码的问题：（进行一次 push or pull,输入一次密码以后就可以保存凭证了。）</font>

```git
git config --global credential.helper store
```
