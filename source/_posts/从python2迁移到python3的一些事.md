---
title: 从python2迁移到python3的一些事儿
date: 2018-09-08 11:57:55
tags: - python
---

最近，需要将一个python2的项目重构。介于到2020年开始python官方将正式停止对python2的支持，索性决定先将项目迁移到python3再进行重构。期间，遇到的一些问题在此做下记录。

## 2to3

Python 3自带了一个叫做2to3的实用脚本，这个脚本会将你的Python 2程序源文件作为输入，然后自动将其转换到Python 3的形式。

命令：

```
$ 2to3 example.py
```

它会将python2的print变成函数;unicode编码转换成str等。
[官方文档](https://docs.python.org/3.0/library/2to3.html)

## MySQL-python的替代者

MySQL-python 又叫 MySQLdb，是 Python 连接 MySQL 最流行的一个驱动，很多框架都也是基于此库进行开发，但是它只支持Python2.x。
对于python3而言有两种选择。

### PyMySQL
PyMySQL 是纯 Python 实现的驱动，速度上比不上 MySQLdb，最大的特点可能就是它的安装方式没那么繁琐，同时也兼容 MySQL-python。但是使用的时候需要加入额外的代码。

```python
pymysql.install_as_MySQLdb()
```

### mysqlclient
在一位牛X同事的推荐下，我最后选择了mysqlclient。这是MySQL-python的fork版本，支持python3。安装方式和MySQL-python一样。完全不需要其他任何修改，非常方便。
[参考网站](https://foofish.net/python-mysql.html)

## open函数
python2中open文件句柄是str(原始二进制)的，而python3中是str(unicode字符)，因此以下代码在python3中会报错：

```python
with open('test','w') as f:
	f.write(os.urandom(10))

TypeError: write() argument must be str, not bytes
```

可以改成

```python
with open('test','wb') as f:
	f.write(os.urandom(10))
```