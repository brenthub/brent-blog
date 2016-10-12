layout: post
categories:
- database

tags: 
- database
- mysql

comments: true
title: rpm安装mysql
permalink: rpm-install-mysql
date: 2016-10-12 10:03:15
---

linux的下mysql安装有很多种方式，如rpm以及直接编译安装（时间太长），本文主要介绍Linux下rpm方式安装mysql的一些常用配置

## 删除依赖
查看是否有安装
```shell
rpm -qa | grep -i mysql
```

当有其它依赖时需要删除（mysql-lib也算）

```shell
rpm -ev MySQL-lib
```

## 安装
```shell
rpm -ivh MySQL-server-5.1.72-1.glibc23.x86_64.rpm --nodeps
rpm -ivh MySQL-client-5.1.72-1.glibc23.x86_64.rpm --nodeps
rpm -ivh MySQL-devel-5.1.72-1.glibc23.x86_64.rpm --nodeps
```
## 查看是否安装成功
```shell
netstat -atln 
```
## 登录
```shell
mysql [-u username] [-h host] [-p[password]] [dbname]
```
## 目录
1、数据库目录
/var/lib/mysql/
2、配置文件
/usr/share/mysql   （mysql.server命令及配置文件）
3、相关命令
/usr/bin   (mysqladmin mysqldump等命令)
4、启动脚本
/etc/rc.d/init.d/   （启动脚本文件mysql的目录）

## 修改登录密码
```shell
usr/bin/mysqladmin -u root password 'root'
```
## 启动与停止
```shell
service mysql start
service mysql restart
service mysql stop
```
## 设置字符集
MySQL的默认编码是Latin1，不支持中文，要支持需要把数据库的默认编码修改为gbk或者utf8。
1.在/etc/下找到my.cnf，如果没有就把/usr/share/mysql目录下的my-medium.cnf复制到/etc/下并改名为my.cnf即可
2.打开my.cnf以后，在[client]和[mysqld]下面均加上default-character-set = utf8，保存并关闭
```shell
default-character-set = utf8
```
3.重新启动MySQL服务

查询字符集
```sql
show variables like '%set%';
```

## 新增非本机访问root帐号
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
```