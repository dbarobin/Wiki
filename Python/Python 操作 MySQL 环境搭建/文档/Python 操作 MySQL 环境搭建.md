# Python 操作 MySQL 环境搭建

| 日期 | 作者 | 文档概要 | 版本 |
|:------------|:-----|:---------------|:-----|
| 2015.12.26 | Robin | Python 操作 MySQL 环境搭建介绍 | v1.0 | 

## 0x00 简介

> 部署 Python 操作 MySQL 环境，用于某项目的改造。

环境中使用到的版本：

| 名称 | 版本 |
|:-----|:-----|
| Python | 2.7.9 |
| setuptools | 0.6c11 |
| MySQL-python | 1.2.3 |
| PyMySQL | 0.6.7 |

## 0x01 命令备忘

## 1.1 Python 安装

``` bash
yum install zlib zlib-devel -y
tar -xvf Python-2.7.9.tar.xz
cd Python-2.7.9
./configure --prefix=/usr/local
make && make install
```

## 1.2 setuptools 安装

``` bash
tar -zxvf setuptools-0.6c11.tar.gz
cd setuptools-0.6c11
python setup.py build
python setup.py install
```

## 1.3 MySQL-Python 安装

``` bash
yum install mysql mysql-devel.i686 mysql-devel.x86_64 -y
tar -zxvf MySQL-python-1.2.3.tar.gz
cd MySQL-python-1.2.3
python setup.py build
python setup.py install
```

## 1.4 PyMySQL 安装

``` bash
tar -zxvf PyMySQL-0.6.7.tar.gz
cd PyMySQL-0.6.7
python setup.py build
python setup.py install
```

## 0x02 说明

软件下载点击：[本文档涉及软件下载地址](https://github.com/dbarobin/Wiki/tree/master/Python/Python%20%E6%93%8D%E4%BD%9C%20MySQL%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/%E8%BD%AF%E4%BB%B6)

Enjoy!
2015.12.26, By Robin Wen.
