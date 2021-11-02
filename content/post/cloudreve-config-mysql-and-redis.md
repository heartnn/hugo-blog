---
title: "Cloudreve迁移到MySQL"
date: 2021-11-02T07:46:23+08:00
categories: 软件
tags: [sqlite,mysql,redis]
---

### 环境 & 起因

- 系统：Debian 10
- 硬件：1 vCPU 512MB
- Cloudreve软件版本：3.3.2 with SQLite

随着文件数的增加，SQLite速度和稳定性都差一些，而且数据库文件越来越大，虽然网盘就我一个人在用，崩了可以重新装，但还是秉着一劳永逸的原则，开始了迁移工作。<!--more-->

### 安装MySQL 5.7并创建数据库

至于为什么不用MariaDB，还是内存占用的问题。

首先安装源，中间缺什么依赖组件可以自行安装：

```
apt-get install curl gnupg
curl -L -o mysql-apt-config.deb https://dev.mysql.com/get/mysql-apt-config_0.8.20-1_all.deb
dpkg -i mysql-apt-config.deb
```

安装完毕后会弹出版本选择的界面，选5.7就可以了：

![](uploads/2021/11/mysql-apt.png)

然后安装即可：

```
apt-get update 
apt-get install mysql-server
```

安装成功后启动MySQL并进行安全性设置：

```
systemctl restart mysql.service
mysql_secure_installation
```

创建一个数据库：

```
mysql -u root -p
# 输入密码后登录到mysql终端

mysql > CREATE DATABASE IF NOT EXISTS cloudreve DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

这样就创建了一个名为cloudreve的数据库。

### 优化MySQL数据库

将下面文本替换到`/etc/mysql/mysql.conf.d/mysqld.cnf`的适当位置：

```
[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
log-error	= /var/log/mysql/error.log
bind-address	= 127.0.0.1
symbolic-links             = 0
performance_schema         = off
table_open_cache           = 100
key_buffer_size            = 8M
thread_stack               = 128K
tmp_table_size             = 32M
max_connections            = 20
table_open_cache_instances = 1
query_cache_limit          = 512K
query_cache_size           = 8M
sort_buffer_size           = 1M
innodb_buffer_pool_size    = 2M
```

### 转换数据库

```
apt-get install python3-pip
pip3 install sqlite3-to-mysql
```

CD到`cloudreve.db`的所在目录，执行：

```
sqlite3mysql -f ./cloudreve.db -t downloads files folders groups policies settings shares tags tasks users webdavs -d cloudreve -u root --mysql-password "$mysql_password" -h 127.0.0.1 -P 3306 -l ./log.log
```

- $mysql_password是之前设置MySQL时root用户的密码。

然后会得到插入MySQL的结果，再将下面的文本插入到Cloudreve目录中的`conf.ini`里：

```
[Database]
Type = mysql
Host = 127.0.0.1
Port = 3306
User = root
Password = <数据库密码>
Name = cloudreve
```

### 配置Redis缓存

先安装Redis并设置自启动：

```
apt-get install redis-server
systemctl enable redis-server.service
systemctl start redis-server.service
```

然后优化`/etc/redis/redis.conf`，注意下面的参数：

```
timeout 30   # 设定超时
databases 2  # 没必要16个数据库
save 3600 1  # 持久化存储
requirepass  # 设定密码
```

然后配置Cloudreve目录下的`conf.ini`：

```
[Redis]
Server = 127.0.0.1:6379
Password = <Redis密码>
DB = 0
```