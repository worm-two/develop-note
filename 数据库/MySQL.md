# 基础篇

## 一 入门介绍

### 概述

###### MySQL安装方式

* 软件仓库(YUM或APT)
* RPM或DEB文件
* 通用二进制文件
* Docker

###### 登录MySQL服务器的方式

mysql -h <ip> -P <port> -u <username> -p<password>

> 1. -P参数用于指定端口
> 2. -p参数用户指定密码,==且没有空格==

###### 忘记密码如何重置

###### RDBMS是什么

关系数据库管理系统

###### MySQL操作分类

* DML(Data Manage Language)

 数据操作语言,包括

* DDL
* DCL
* DML

## 数据类型

### 整数型



### 浮点数

### 字符串

### JSON

## SQL语法

### 不常用语法



### 数据库

```mys
# 要查找连接到了哪个数据库
SELECT DATABASE() ;

# 切换数据库

# 查询所有数据库
SHOW DATABASES ;

```



```mysql
INSERT IGNORE INTO customers(first_name,last_name,country)
VALUES
（Mike ’，’ Chr 工 stensen ’，『 USA 『 ） ，
（Andy 『，’ Hollands ’,’ Australia ’ ) ,
(Ravi ’,’ Vedantam ?, ’ I ndia ’ ) ,
( ’ Ra ] iv ’ , ’ Perera ’,’ s ri Lanka ' ) ;
```









### 数据表

### 插入

### 修改

### 删除

### 查询



## 三  



# 中级篇



## 一 安装部署

### 单机版

1. 解压缩文件

```shell
tar -xvf mysql-8.4.2-linux-glibc2.28-x86_64.tar.xz -C /java/software/mysql

mv mysql-8.4.2-linux-glibc2.28-x86_64.tar.xz app
```

2. 初始化

```shell
# 初始化密码在error.log
./mysqld --initialize --user=java --basedir=/java/software/mysql/app --datadir=/java/software/mysql/data --log_error=/java/software/mysql/log/error.log
```

3. 配置文件my.cnf

```shell
[mysqld]
# 端口
port=3306
# 用户
user=java
# 线程id
pid-file=/java/software/mysql/config/mysqld.pid
# 套接字
socket=/java/software/mysql/config/mysql.sock
# 安装位置
basedir=/java/software/mysql/app
# 数据存储位置
datadir=/java/software/mysql/data
# 存储引擎
default-storage-engine=INNODB
# 编码
character-set-server=utf8mb4
collation-server=utf8mb4_general_ci
# 网络连接
max_connections=200
max_connect_errors=10
wait_timeout=28800
interactive_timeout=28800
max_allowed_packet=64M
# 日志设置
log_error=/java/software/mysql/log/error.log
innodb_redo_log_capacity=64M
binlog_expire_logs_seconds=86400
max_binlog_size=104857600
# 慢查询
long_query_time=1
log_queries_not_using_indexes=1
slow_query_log=1
slow_query_log_file=/java/software/mysql/log/mysql_slow_query.log
# 缓冲池
key_buffer_size=32M
innodb_buffer_pool_size=512M

[mysql]
# 编码
default-character-set=utf8mb4

[client]
# 套接字
socket=/java/software/mysql/config/mysql.sock
# 编码
default-character-set=utf8mb4
port=3306
```

4. 启动MySQL

```shell
./mysqld_safe --defaults-file=/java/software/mysql/config/my.cnf --daemonize
```

5. 客户端进入MySQL

```shell
# 指定socket
./mysql -uroot -p -S /java/software/mysql/config/mysql.sock

# 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

6. 远程连接

```shell
create USER 'root'@'%' IDENTIFIED BY 'root';
flush privileges;

grant all privileges on *.* to 'root'@'%';
flush privileges;
```

7. 服务自启动

```shel
# cd /etc/systemd/system/

sudo vim mysql.service

[Unit]
Description=MySQL Server
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target

[Service]
Type=notify
User=java
Group=java
LimitNOFILE=6553
ExecStart=/java/software/mysql/app/bin/mysqld --defaults-file=/java/software/mysql/config/my.cnf


# sudo systemctl daemon-reload
```







```shell
2025-10-02T02:09:09.071857Z 0 [System] [MY-015017] [Server] MySQL Server Initialization - start.
2024-10-02T02:09:09.074537Z 0 [System] [MY-013169] [Server] /java/software/mysql/app/bin/mysqld (mysqld 8.4.2) initializing of server in progress as process 11987
2024-10-02T02:09:09.085700Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2024-10-02T02:09:09.519712Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2024-10-02T02:09:12.825341Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: mTSMB;PhZ3pd
2024-10-02T02:09:20.261587Z 0 [System] [MY-015018] [Server] MySQL Server Initialization - end.
2024-10-02T02:16:03.426984Z 0 [System] [MY-015015] [Server] MySQL Server - start.
2024-10-02T02:16:03.731114Z 0 [System] [MY-010116] [Server] /java/software/mysql/app/bin/mysqld (mysqld 8.4.2) starting as process 12053
2024-10-02T02:16:03.745507Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2024-10-02T02:16:04.105391Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2024-10-02T02:16:04.461613Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2024-10-02T02:16:04.461681Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2024-10-02T02:16:04.470915Z 0 [ERROR] [MY-000067] [Server] unknown variable 'default_authentication_plugin=caching_sha2_password'.
2024-10-02T02:16:04.472311Z 0 [ERROR] [MY-010119] [Server] Aborting
2024-10-02T02:16:05.947689Z 0 [System] [MY-010910] [Server] /java/software/mysql/app/bin/mysqld: Shutdown complete (mysqld 8.4.2)  MySQL Community Server - GPL.
2024-10-02T02:16:05.947724Z 0 [System] [MY-015016] [Server] MySQL Server - end.
2024-10-02T02:17:21.508723Z 0 [System] [MY-015015] [Server] MySQL Server - start.
2024-10-02T02:17:21.769420Z 0 [System] [MY-010116] [Server] /java/software/mysql/app/bin/mysqld (mysqld 8.4.2) starting as process 12266
2024-10-02T02:17:21.782759Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2024-10-02T02:17:22.059276Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2024-10-02T02:17:22.314586Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2024-10-02T02:17:22.314647Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2024-10-02T02:17:22.346767Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /tmp/mysqlx.sock
2024-10-02T02:17:22.346974Z 0 [System] [MY-010931] [Server] /java/software/mysql/app/bin/mysqld: ready for connections. Version: '8.4.2'  socket: '/java/software/mysql/config/mysql.sock'  port: 3306  MySQL Community Server - GPL.
~
```









# 高级篇

