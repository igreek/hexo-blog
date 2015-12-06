title: python+mysqldb数据库环境搭建和入门
date: 2014-10-30 00:55:00
categories: python开发
tags: [python]
toc: true
---
python+mysqldb数据库环境搭建需要安装相关插件和配置，下面具体分析如何搭建和配置。
<!--more-->
### **一、工具安装和配置**
我选择的是python2.7版本(32位)，eclipse，PyDev 2.7.5 和mysqldb(32位)
各文件的下载路径为:
python2.7地址:http://download.csdn.net/detail/xhwwc110/8098095
或者https://www.python.org/downloads/release/python-278/
PyDev 2.7.5地址:http://sourceforge.net/projects/pydev/files/pydev/
eclipse地址:http://www.eclipse.org/downloads/
mysqldb(32位)地址：http://download.csdn.net/detail/xhwwc110/8098105
或http://www.codegood.com/archives/129
   **(1).python和eclipe的安装很简单，直接运行安装文件即可**
   **( 2).pydev集成在eclipse中，可以有在线安装和离线安装。**
    1.在线安装方式为:打开Eclipse，找到Help菜单栏，进入Install New Software选项。但因版本冲突问题没有安装成功。  参考如下:
![这里写图片描述](http://img.blog.csdn.net/20151206215136277)
![这里写图片描述](http://img.blog.csdn.net/20151206215155938)

   2.选择离线安装方法，离线安装方式为:
   A.把下载好的pydev压缩文件解压后,得到features和plugins两个文件夹，然后将两个文件夹中的文件分别拷贝到Eclipse安装目录下的features和plugins目录中。
   B.打开eclipse，点击windows-preferences选择pydev,选择Interpreter-Python，点击new，添加python
   C.选择好libraries下的配置依赖包，点击OK，安装完成
 **(3).python配置MySQLdb**
     1.首先需要安装好MySQLdb,具体的安装可参考如下:
	![这里写图片描述](http://img.blog.csdn.net/20151206215234353)
   2.在python shell 用import MySQLdb进行测试，没有异常,则提示安装成功
     若出现如下信息:
 

```
import MySQLdb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python25\Lib\site-packages\MySQLdb\__init__.py", line 19, in <module>
    import _mysql
```

ImportError: DLL load failed: 找不到指定的程序。则为安装失败。
解决方法:
A.下载libmmd.dll,libmySQL.dll和libguide40.dll文件并复制到python安装目录的Lib\site- packages下
   下载地址为:
B.检查下python和mysqldb的版本属性。若不一致(32位对应64位)会出现"DLL load failed: 找不到指定的程序"
   **(4).在eclipse中配置MySQLdb**
    1.选择pydev-Interpreter-Python,在Forced Builtins下面手工添加 MySQLdb 字段。
Apply之后，可以看到 Libraries 下面添加了 MySQLdb的目录。如果没有，则手工在Libraries 下面添加MySQLdb的目录，再次强制编译即可。
![这里写图片描述](http://img.blog.csdn.net/20151206215334919)

### **二、python数据库编程例子**
 (1).以下是一个连接mysql创建数据库和创建一个表的简单例子.可以测试下MySQLdb是否能正常使用。

```
 #!usr/bin/env python
# -*- coding:utf-8 -*-
import MySQLdb
try:
   conn=MySQLdb.connect(host='localhost',user='root',passwd='root',port=3306)
   cur=conn.cursor()
    
   cur.execute('create database if not exists python')
   conn.select_db('python')
   cur.execute('create table student(id int,info varchar(20))')
    
   conn.commit()
   cur.close()
   conn.close()
 
except MySQLdb.Error,e:
   print "Mysql Error %d: %s" % (e.args[0], e.args[1]) 
```

   
 执行完成后，检查mysql中是否存在该数据库和表。


  
   