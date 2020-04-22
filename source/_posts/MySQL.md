---
layout: SQL
title: MySQL
date: 2020-03-24 12:09:05
tags: MySQL
categories:
- SQL
---
## MySQL安装
- 压缩版
- 解压
- 配置环境变量,bin目录
- 新建my.ini
```ini
[mysqld]
basedir=D:/Environment/mysql-5.7
datadir=D:/Environment/mysql-5.7/data
port=3306
skip-grant-tables
```
- 管理员模式cmd bin目录下
- 安装mysql服务
- 初始化数据库文件
```
mysqld -install
mysqld --initialize-insecure --user=root
```
- 启动服务
```
net start mysql
```
- 启动mysql修改密码
```
update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';
```
- 刷新flush privileges,删除my.ini中的skip-grant-tables
- 重启mysql服务
  - net stop mysql
  - net start mysql