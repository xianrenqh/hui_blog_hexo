---
title: 'InnoDB: Cannot allocate memory for the buffer pool'
urlname: bcyw110znw9zp2cn
date: '2023-08-14 08:05:55 +0000'
tags: []
categories: []
---

tags: []

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.cc

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">综述：这是一次MySQL启动失败故障排查的过程。核心报错内容是[ERROR] InnoDB: Cannot allocate memory for the buffer pool ，解决方案是修改mysql配置文件里下述参数的值：innodb_buffer_pool_size 、join_buffer_size ，然后重启mysqld服务。对应服务器系统是CentOS 7。</font>

<font style="color:rgb(51, 51, 51);"></font>

## <font style="color:rgb(51, 51, 51);">1、查看 mysql 配置文件</font>

```sql
vi /etc/my.cnf
```

<font style="color:rgb(51, 51, 51);">得到类似如下的内容：</font>

```sql
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
innodb_buffer_pool_size = 512M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
join_buffer_size = 512M
sort_buffer_size = 16M
read_rnd_buffer_size = 16M
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

<font style="color:rgb(51, 51, 51);">在文件底部可看到 log-error=/var/log/mysqld.log ，说明可通过/var/log/mysqld.log 文件查看 mysql 错误日志。</font>

## <font style="color:rgb(51, 51, 51);">2、查看 mysql 错误日志</font>

```sql
vi /var/log/mysqld.log
```

<font style="color:rgb(51, 51, 51);">将日志文件拉到底部查看最新日志，得到如下内容：</font>

```sql
2023-08-14 16:02:25 232892 [Note] Plugin ＇FEDERATED＇ is disabled.
2023-08-14 16:02:25 232892 [Note] InnoDB: Using atomics to ref count buffer pool pages
2023-08-14 16:02:25 232892 [Note] InnoDB: The InnoDB memory heap is disabled
2023-08-14 16:02:25 232892 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2023-08-14 16:02:25 232892 [Note] InnoDB: Memory barrier is not used
2023-08-14 16:02:25 232892 [Note] InnoDB: Compressed tables use zlib 1.2.11
2023-08-14 16:02:25 232892 [Note] InnoDB: Using Linux native AIO
2023-08-14 16:02:25 232892 [Note] InnoDB: Using CPU crc32 instructions
2023-08-14 16:02:25 232892 [Note] InnoDB: Initializing buffer pool, size = 40.0M
2023-08-14 16:02:25 232892 [Note] InnoDB: Completed initialization of buffer pool
2023-08-14 16:02:25 232892 [Note] InnoDB: Highest supported file format is Barracuda.
2023-08-14 16:02:25 232892 [Note] InnoDB: The log sequence numbers 0 and 0 in ibdata files do not match the log sequence number 1600614 in the ib_logfiles!
2023-08-14 16:02:25 232892 [Note] InnoDB: Database was not shutdown normally!
2023-08-14 16:02:25 232892 [Note] InnoDB: Starting crash recovery.
2023-08-14 16:02:25 232892 [Note] InnoDB: Reading tablespace information from the .ibd files...
2023-08-14 16:02:25 232892 [Note] InnoDB: Restoring possible half-written data pages
2023-08-14 16:02:25 232892 [Note] InnoDB: from the doublewrite buffer...
2023-08-14 16:02:25 232892 [Note] InnoDB: 128 rollback segment(s) are active.
2023-08-14 16:02:25 232892 [Note] InnoDB: Waiting for purge to start
2023-08-14 16:02:25 232892 [Note] InnoDB: 5.6.50 started; log sequence number 1600614
2023-08-14 16:02:25 232892 [Note] Recovering after a crash using mysql-bin
2023-08-14 16:02:25 232892 [Note] Starting crash recovery...
2023-08-14 16:02:25 232892 [Note] Crash recovery finished.
2023-08-14 16:02:25 232892 [Note] RSA private key file not found: /www/server/data//private_key.pem. Some authentication plugins will not work.
2023-08-14 16:02:25 232892 [Note] RSA public key file not found: /www/server/data//public_key.pem. Some authentication plugins will not work.
2023-08-14 16:02:25 232892 [Note] Server hostname (bind-address): ＇*＇; port: 3306
2023-08-14 16:02:25 232892 [Note] IPv6 is available.
2023-08-14 16:02:25 232892 [Note]   - ＇::＇ resolves to ＇::＇;
2023-08-14 16:02:25 232892 [Note] Server socket created on IP: ＇::＇.
2023-08-14 16:02:25 232892 [Warning] InnoDB: Cannot open table mysql/slave_master_info from the internal data dictionary of InnoDB though the .frm file for the table exists. See http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting.html for how you can resolve the problem.
2023-08-14 16:02:25 232892 [Warning] Info table is not ready to be used. Table ＇mysql.slave_master_info＇ cannot be opened.
2023-08-14 16:02:25 232892 [Warning] InnoDB: Cannot open table mysql/slave_worker_info from the internal data dictionary of InnoDB though the .frm file for the table exists. See http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting.html for how you can resolve the problem.
2023-08-14 16:02:25 232892 [Warning] InnoDB: Cannot open table mysql/slave_relay_log_info from the internal data dictionary of InnoDB though the .frm file for the table exists. See http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting.html for how you can resolve the problem.
2023-08-14 16:02:25 232892 [Warning] Info table is not ready to be used. Table ＇mysql.slave_relay_log_info＇ cannot be opened.
2023-08-14 16:02:25 232892 [Note] Event Scheduler: Loaded 0 events
2023-08-14 16:02:25 232892 [Note] /www/server/mysql/bin/mysqld: ready for connections.
Version: ＇5.6.50-log＇  socket: ＇/tmp/mysql.sock＇  port: 3306  Source distribution
2023-08-14 16:07:31 233060 [Warning] Buffered warning: Performance schema disabled (reason: init failed).

2023-08-14 16:07:31 233060 [Note] Plugin ＇FEDERATED＇ is disabled.
2023-08-14 16:07:31 233060 [Note] InnoDB: Using atomics to ref count buffer pool pages
2023-08-14 16:07:31 233060 [Note] InnoDB: The InnoDB memory heap is disabled
2023-08-14 16:07:31 233060 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2023-08-14 16:07:31 233060 [Note] InnoDB: Memory barrier is not used
2023-08-14 16:07:31 233060 [Note] InnoDB: Compressed tables use zlib 1.2.11
2023-08-14 16:07:31 233060 [Note] InnoDB: Using Linux native AIO
2023-08-14 16:07:31 233060 [Note] InnoDB: Using CPU crc32 instructions
2023-08-14 16:07:31 233060 [Note] InnoDB: Initializing buffer pool, size = 40.0M
2023-08-14 16:07:31 233060 [Note] InnoDB: Completed initialization of buffer pool
2023-08-14 16:07:31 233060 [Note] InnoDB: Highest supported file format is Barracuda.
2023-08-14 16:07:31 233060 [Note] InnoDB: The log sequence numbers 0 and 0 in ibdata files do not match the log sequence number 1600624 in the ib_logfiles!
2023-08-14 16:07:31 233060 [Note] InnoDB: Database was not shutdown normally!
2023-08-14 16:07:31 233060 [Note] InnoDB: Starting crash recovery.
2023-08-14 16:07:31 233060 [Note] InnoDB: Reading tablespace information from the .ibd files...
2023-08-14 16:07:32 233060 [Note] InnoDB: Restoring possible half-written data pages
2023-08-14 16:07:32 233060 [Note] InnoDB: from the doublewrite buffer...
2023-08-14 16:07:32 233060 [Note] InnoDB: 128 rollback segment(s) are active.
2023-08-14 16:07:32 233060 [Note] InnoDB: Waiting for purge to start
2023-08-14 16:07:32 233060 [Note] InnoDB: 5.6.50 started; log sequence number 1600624
2023-08-14 16:07:32 233060 [Note] Recovering after a crash using mysql-bin
2023-08-14 16:07:32 233060 [Note] Starting crash recovery...
2023-08-14 16:07:32 233060 [Note] Crash recovery finished.
2023-08-14 16:07:32 233060 [Note] RSA private key file not found: /www/server/data//private_key.pem. Some authentication plugins will not work.
2023-08-14 16:07:32 233060 [Note] RSA public key file not found: /www/server/data//public_key.pem. Some authentication plugins will not work.
2023-08-14 16:07:32 233060 [Note] Server hostname (bind-address): ＇*＇; port: 3306
2023-08-14 16:07:32 233060 [Note] IPv6 is available.
2023-08-14 16:07:32 233060 [Note]   - ＇::＇ resolves to ＇::＇;
2023-08-14 16:07:32 233060 [Note] Server socket created on IP: ＇::＇.
2023-08-14 16:07:32 233060 [Warning] InnoDB: Cannot open table mysql/slave_master_info from the internal data dictionary of InnoDB though the .frm file for the table exists. See http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting.html for how you can resolve the problem.
2023-08-14 16:07:32 233060 [Warning] Info table is not ready to be used. Table ＇mysql.slave_master_info＇ cannot be opened.
2023-08-14 16:07:32 233060 [Warning] InnoDB: Cannot open table mysql/slave_worker_info from the internal data dictionary of InnoDB though the .frm file for the table exists. See http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting.html for how you can resolve the problem.
2023-08-14 16:07:32 233060 [Warning] InnoDB: Cannot open table mysql/slave_relay_log_info from the internal data dictionary of InnoDB though the .frm file for the table exists. See http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting.html for how you can resolve the problem.
2023-08-14 16:07:32 233060 [Warning] Info table is not ready to be used. Table ＇mysql.slave_relay_log_info＇ cannot be opened.
2023-08-14 16:07:32 233060 [Note] Event Scheduler: Loaded 0 events
2023-08-14 16:07:32 233060 [Note] /www/server/mysql/bin/mysqld: ready for connections.
```

<font style="color:rgb(51, 51, 51);">发现：</font>

```sql
2023-08-14 16:07:32 233060 0 [Note] InnoDB: Initializing buffer pool, total size = 512M, instances = 1, chunk size = 128M
2023-08-14 16:07:32 233060 0 [ERROR] InnoDB: mmap(137428992 bytes) failed; errno 12
2023-08-14 16:07:32 233060 0 [ERROR] InnoDB: Cannot allocate memory for the buffer pool
```

<font style="color:rgb(51, 51, 51);">应该是内存不够启动 mysql 服务了。</font>

<font style="color:rgb(51, 51, 51);">注意：报错信息里所需内存大小为：137428992 bytes = 137428992 / 1024 / 1024 M = 131.0625 M</font>

## <font style="color:rgb(51, 51, 51);">3、查询内存情况</font>

```sql
# 以M为单位显示内存情况
free -m
```

<font style="color:rgb(51, 51, 51);">得到：</font>

```sql
# free -m
total used free shared buff/cache available
Mem: 1946 1829 16 4 100 70
Swap: 1023 1001 22
```

<font style="color:rgb(51, 51, 51);">发现此时服务器相关内存情况为：</font>

- <font style="color:rgb(51, 51, 51);">free: 16 M</font>
- <font style="color:rgb(51, 51, 51);">buff/cache: 100 M</font>
- <font style="color:rgb(51, 51, 51);">available: 70 M</font>

<font style="color:rgb(51, 51, 51);">这几个数据量都很可怜了。所以我们可以增大可用内存，停掉一些不用的服务，或者减少 mysql 所需内存。</font>

## <font style="color:rgb(51, 51, 51);">4、修改 mysql 配置文件</font>

```sql
vi /etc/my.cnf
```

<font style="color:rgb(51, 51, 51);">然后：</font>

- <font style="color:rgb(51, 51, 51);">将</font><font style="color:rgb(51, 51, 51);">innodb_buffer_pool_size = 512M</font><font style="color:rgb(51, 51, 51);"> 修改为</font><font style="color:rgb(51, 51, 51);">innodb_buffer_pool_size = 64M</font>
- <font style="color:rgb(51, 51, 51);">将</font><font style="color:rgb(51, 51, 51);">join_buffer_size = 512M</font><font style="color:rgb(51, 51, 51);"> 改为</font><font style="color:rgb(51, 51, 51);">join_buffer_size = 64M</font>

<font style="color:rgb(51, 51, 51);">然后，重启 mysql 服务：</font>

```sql
service mysqld restart
```

## <font style="color:rgb(51, 51, 51);">5、验证 mysql 服务是否正常</font>

<font style="color:rgb(51, 51, 51);">打开网站，发现数据展示正常</font>

<font style="color:rgb(51, 51, 51);">完毕</font>
