# Mac安装MySQL

<!--more-->

## 前言
最近在学习MySQL，需要在Mac上安装MySQL，记录一下安装使用过程。

## 安装
### 1.通过brew安装
```shell
brew install mysql
```
### 2.启动MySQL
```shell
brew services start mysql
```
### 3.设置密码
```shell
mysql_secure_installation
```
### 4.登录MySQL
```shell
mysql -uroot -p
```
### 5.修改密码
```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```
### 6.退出MySQL
```shell
exit
```
### 7.重启MySQL
```shell
brew services restart mysql
```
### 8.登录MySQL
```shell
mysql -uroot -p
```
### 9.创建数据库
```shell
create database test;
```
### 10.查看数据库
```shell
show databases;
```
### 11.删除数据库
```shell
drop database test;
```
### 12.MySQL 服务器管理命令
```toml
brew services start mysql #: 启动 MySQL 服务器，并设置为自启动。
brew services stop mysql #: 停止 MySQL 服务器，并设置为不自启动。
brew services run mysql #: 只启动 MySQL 服务器。
mysql.server start #: 启动 MySQL 服务器。
mysql.server stop #: 停止 MySQL 服务器
```

## 使用(以mysite为例)

执行上面的安装和启动后，就可以使用MySQL了。
### 1.创建数据库
```shell
create database mysite DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```
### 2.查看数据库
```shell
show databases;
```
### 3.选择数据库
```shell
use mysite;
```
### 4.创建表
```shell
CREATE TABLE `app_userinfo` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `email` varchar(255) NOT NULL,
  `is_active` tinyint(1) NOT NULL,
  `is_staff` tinyint(1) NOT NULL,
  `is_superuser` tinyint(1) NOT NULL,
  `date_joined` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `username` (`username`),
  UNIQUE KEY `email` (`email`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
```
### 5.查看表
```shell
desc app_userinfo;
```



## 参考
{{<link href="https://www.sjkjc.com/mysql/install-on-macos/" content="【Mac安装MySQL】">}}
