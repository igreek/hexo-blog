title:  python标准库介绍
date: 2015-02-07 23:36:00
categories: python开发
tags: [python,标准库]
toc: true
---
python标准库包含操作系统接口，文件通配符，命令行参数。字符串正在表达式，日期和时间,数学函数，互联网访问接口，数据压缩以及质量和性能的控制等。接口丰富，提供开始效率。下面以例子的方式说明下各个库中的内容:
<!--more-->
python标准库包含操作系统接口，文件通配符，命令行参数。字符串正在表达式，日期和时间,数学函数，互联网访问接口，数据压缩以及质量和性能的控制等。接口丰富，提供开始效率。下面以例子的方式说明下各个库中的内容:

操作系统接口
------

import os
print os.getcwd()
print dir(os)
print help(os)

文件通配符
-----

import glob
print glob.glob("*.py")

命令行参数
-----

import sys
print sys.argv

字符串正则表达式
--------

import re
'tea for too'.replace('tea', 'water')
re.findall(r'\b[a-z]*','which foot or hand fell fastest')

数学计算
----

import math
print math.__package__
print math.log(1023,2)

随机函数
----

import random
print random.choice(['apple','orange','banana'])
print random.random()
print random.randrange(6)
print random.sample(xrange(100),10)

日期和时间
-----

from datetime import date
print date.today()
now= date.today()
print now.strftime('%m-%d-%y. %d %b %Y is a %A on the %d day of %B.')

数据压缩
----

import zlib
s=b'witch which has which witches wrist watch'
print len(s)

b=zlib.compress(s)
print len(b)

c=zlib.decompress(b)
print c

性能度量和质量控制
---------

1.doctest 模块和unittest 模块