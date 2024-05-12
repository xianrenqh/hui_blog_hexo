---
title: 服务器常用命令
urlname: xokyts7y4ek65nvs
date: '2023-07-27 01:59:55 +0000'
tags: []
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

### 查询 ip:

```shell
ipconfig
```

### 查看端口占用情况

```shell
 netstat -anp | grep 8080
#或者
lsof -i:8080
```

### 查看服务

```
ps -ef | grep 服务名 或 ps aux |grep 服务名
```

### 定时任务

```shell
#任务列表
crontab -l

#添加定时任务
crontab -e #编辑cron任务模式

#常用：https://www.runoob.com/w3cnote/linux-crontab-tasks.html
#每分钟执行一次
* * * * * docker exec 2751dfasd8e php /www/hello-world/crontab/gogogo schedule:run >>/docker/nginx/www/hello-world/storage/logs/notice.txt 2>&1

#每小时的第3和第15分钟执行
3,15 * * * * myCommand

#每晚的21:30重启smb
30 21 * * * /etc/init.d/smb restart

#每一小时重启smb
0 */1 * * * /etc/init.d/smb restart
```

### 查询端口是否通：

```shell
ping 域名或ip 如：ping 127.0.0.1
telnet 如：telnet 127.0.0.1 80
```

### 防火墙开放端口（mysql）

```shell
#查看对外开放的端口状态

firewall-cmd --zone=public --list-ports

#开放端口	--permanent 永久生效,没有此参数重启后失效

firewall-cmd --zone=public --add-port=80/tcp --permanent

#重载入添加的端口

firewall-cmd --reload

#查询已开放的端口

netstat -ntulp | grep #端口号：可以具体查看某一个端口号

#查询指定端口是否已开

firewall-cmd --query-port=3306/tcp

#提示 yes，表示开启；no表示未开启。

#查看想开的端口是否已开：

firewall-cmd --query-port=6379/tcp

#添加指定需要开放的端口

firewall-cmd --add-port=5466/tcp --permanent



#查询指定端口是否开启成功

firewall-cmd --query-port=123/tcp

#移除指定端口

firewall-cmd --permanent --remove-port=123/tcp
```

### keepalived 的 VIP 问题

```shell
firewall-cmd --direct --permanent --add-rule ipv4 filter INPUT 0 --in-interface ens33 --destination 127.0.0.18 --protocol vrrp -j ACCEPT
```

### 加入开机自启动

```shell
chkconfig keepalived on
```

### 查看文件大小：

```shell
#查看文件大小
du -h --max-depth=1
#查询该目录下所有资源的大小，包括压缩包的大小
du -ah --max-depth=1
#查看目录的总体大小
du -sh
#查看所有的文件大小
ll -h
```

### 压缩和解压缩

```shell
#zip命令
zip -r myfile.zip ./* #压缩目录
zip -d myfile.zip smart.txt #压缩文件
unzip -o -d /home/sunny myfile.zip  #解压缩
```

### 开关机命令

```shell
#立刻进行关机
shutdown -h now

#现在重新启动计算机
reboot
```

### docker 相关：

#### a、从 docker inspect 查看 docker run 的运行命令参数

```shell
--name在Name表示容器名称；

-p在端口映射情况如在 NetworkSettings.Ports属性下可以明显地看到, 在已建立端口映射的属性下会有 HostIp和HostPort 两个子属性; 在没有建立映射情况下, 子属性为null

-v目录映射在HostConfig.Binds和HostConfig.Mounts中可以找到

--net diynet 自定义网络在HostConfig.NetworkMode和NetworkSettings.Networks中可以找到

--add-host=mirrors.test.com:128.0.0.2 docker中的host配置在HostConfig.ExtraHosts

nginx:latest在Config.Image表示镜像名称

--link php74fpm 和php74fpm的容器互联  在NetworkSettings.Networks.Links可以找到
```

#### b、查看 docker 容器 ip:

```shell
docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container-ID>
#或者
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```

#### c、查看容器 ip:

```shell
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```

查看 python 版本

```shell
#liunx系统默认安装了python2
python -V
#如果安装完python3没配置环境变量
python3 -V
```

## 数据库命令：

### 修改当前登录用户的密码

```
ALTER USER USER() IDENTIFIED BY "password"
```

### 新增用户并赋权

```
#创建用户
CREATE USER 'username'@'%' IDENTIFIED BY 'password';
#用户授权
GRANT ALL ON *.* TO 'username'@'%';
#刷新授权
flush privileges;
```

### 导出/导入数据库

```
#备份mysql命令:（导出）
mysqldump -uroot -proot 库名 > 路径\ceshi_copy.sql

#还原mysql命令：（导入）
mysql> use 数据库;
mysql>source d:\bak\bak.sql
```

## win 系统常用命令

### win 系统 dos 监听端口：

```
netstat -ano|findstr 80
```

### win 系统杀死进程：

```
tskill 进程号
```

查找文件

```shell
dir "new sc."* /s
```
