title: python基础详解
date: 2015-10-22 15:47:17
categories: python开发
tags: [python]
toc: true
---
Python是一种简单易学，功能强大的编程语言，它有高效率的高层数据结构，能简单而有效地实现面向对象编程。Python简洁的语法和对动态输入的支持，再加上解释性语言的本质，使得它在大多数平台上的很多领域都是一个理想的脚本语言，特别适用于快速的应用程序开发。
<!--more-->
## 一、语句和语法
		python具有一些基本的规则和特殊字符，说明如下：
		#:用于注释内容
		\n:用于分隔符
		\:继续上一行
		如 if(A==1) and \
				(B==2):
						return    
						
		特殊情况为:'''(三引号下的内容可跨行) 2.给一些变量赋值
		
		;将两个语句连接在一行中  
		如:import sys;x='foo' sys.stdout.write(x) 此方式不提倡
		
		:将代码块的头和体分开
		缩进相同的一组语句构成代码块，如if while def class等 以(:)结束 ，则该行后的一行或多行称为代码组
		代码块(语句):用缩进的方式体现
		py文件是以模块的方式组织
		每一个python脚本可以称为一个模块，以文件的方式存储。模块的中内容可以是可直接执行脚本，一些类库函数 或被导入的调用
		
## 二、变量
		(1)赋值操作(注意pyton赋值不是将一个值赋给另一个变量 而是将该对象的引用赋值给变量)   
				如 aList=[1,2,3,'abc',23.44] aStr='hello'		
		(2)增量操作
				等号和一个算术操作符构成一个增量操作符 如x+=1  -= %= **= 
				但python不支持类似x++ 或x--等自增或自减
		(3)多重赋值
		   如x=y=z=1
		   另一种为将多个变量同时赋值.如 x,y,z=1,3,'abc'
		   交换两个变量的赋值可以通过多元赋值实现
		   如:x,y=1,2
		   x,y=y,x
		   
    **标识符**
    标识符是有效字符串的集合 其中有一部分构成保留字 不能用于其他用途
    pyhon标识符规则:
     1.第一个字符必须是字母或下划线
     2.其他字符需是字母，下划线或数字组成
     3.大小写敏感
    (2)关键字
    (3)内建
    (4)专用下划线标识符
    	python可以用下划线作为变量的前缀或后缀指定特殊变量符，说明如下
    	1._xxx 不要from module import * 导入
    	2._XXX_ 系统定义的名字
    	3._xxx 类中的私有变量 在模块或类外不可以使用
     		   
## 三、编程风格
		1.注释
		2.文档
		 python 提供了一种机制，可以用_doc_特殊变量 动态获取文档内容 ，在类 模块或函数声明中第一个没有赋值的字符串可用属性obj._doc_来访问
		3.缩进
	(2).模块的结构和布局
		用模块组织python的代码 将其应用到每一个文件，如
		1.起始行
		2.模块文档
			说明模块的功能和全局变量的含义。可以通过module._doc_访问
		3.模块导入
		4.变量定义
			次变量定义为全局变量，在本模块的所有函数都可以使用，但通常使用局部变量代替全局变量
		5.类定义 (类的文档变量是class._doc_)
		6.函数定义(函数的文档变量是function._doc_)
		7.主程序
			无论是被导入的模块还是作为直接执行的脚本，此部分都会执行，是根据执行模式调用不同的函数
			python可以用如下方式检测该模块是被导入的模块还是被直接执行
			A.若模块是被导入，__name__的值为模块的名字
			B.若模块是直接执行,__name__的值为__main__
			
## 四、内存管理
		变量无须事先声明
		变量无须指定类型
		不需要关心内存回收
		del语句可直接释放资源
## 五、入门例子

```
#!/usr/bin/env python

'makeTextFile.py -- create text file'

import os

# get filename
while True:
    fname = raw_input('Enter file name: ')
    if os.path.exists(fname):
        print"*** ERROR: '%s' already exists" % fname
    else:
        break

# get file content (text) lines
all = []
print "\nEnter lines ('.' by itself to quit).\n"

# loop until user terminates input
while True:
    entry = raw_input('> ')
    if entry == '.':
        break
    else:
        all.append(entry)

# write lines to file with NEWLINE line terminator
fobj = open(fname, 'w')
fobj.write('\n'.join(all))
fobj.close()
print 'DONE!'

```

## 六、相关的模块和开发工具 
调试器:pdb
记录器:logging
性能测试器:profile hotshot 