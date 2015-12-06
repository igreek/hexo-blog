title:  python输入和输出
date: 2015-02-05 00:04:00
categories: python开发
tags: [python,输入和输出]
toc: true
---
python的输入和输出可分为普控制台和文件等的读写，下面分析下python输入和输出的实现。
<!--more-->
### **一、格式化输入和输出**

若要对输出格式进行格式化，则控制输出的方式有以下几种:
1.使用字符串切割和连接操作可以创建任何你想要的输出形式
2.使用 str.format() 方法
3.实例:  

```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
'''
Created on 2015-2-4

@author: Administrator
'''
#格式化输入和输出
#1.str和repr函数 将数值转换为字符串
s='hello,world!'
print str(s)
print repr(s)
 
x=10*3.25
y=200*200
s= 'the value of x is'+repr(s)+','+'the value of y is '+str(y)
print s

#2.平方和立方表
# s.rjust(),s.center()等把字符串输出一列，并以某格式对齐
# for x in range(1,10):
#     print repr(x).rjust(2),repr(x*x).rjust(3),
#     print(repr(x*x*x).rjust(4))
#s.format() 调用时使用关键字参数，可以通过参数名来引用值，也可以用 ‘**’ 标志将这个字典以关键字参数的方式传入。
print 'the story of {0},{1} is {other}'.format('bill','maryy', other='jhone')
table={'lu':123,'wu':456,'li':4568}
print 'lu:{lu:d};wu:{wu:d}'.format(**table)
```

### **二、文件的读写**

文件的操作主要包含打开，读写，定位，遍历，关闭等。具体的方法如下实例:
   

```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
'''
Created on 2015-2-4

@author: Administrator
'''
#文件的读写
#f=open('d:/test.txt','r')
f=open('d:/test.txt','wb+')
#f.read()返回读取若干数量的数据并以字符串返回其内部，其中size是可变的整数值,若没有指定size或size的值为负数
#则返回文件中所有的内容,若读取到文件的末尾则f.read()会返回一个空字符串
#result=f.read()
# result=f.read(10)
# print result

'''
f.readline():从文件中读取单独一行，字符串结尾会自动加上一个换行符（ \n ），只有当文件最后一行没有以换行符结尾时，
这一操作才会被忽略.若f.readline返回一个空字符串，则表示达到了文件的末尾
'''
# line=f.readline()
# print line

'''
f.readlines()：返回一个列表，其中包含了文件中所有的数据行.通常用于高效读取文件，避免将整个文件读入内存
'''

# lines=f.readlines()
# print lines

'''
f.write()向文件中写入字符串内容
f.tell() 返回一个整数，代表文件对象在文件中的指针位置
f.seek(offset,from):指针在该操作中从指定的引用位置移动 offset个值，从from 开始移动
'''
# value=('answer:',12)
# s=str(value)
# f.write(s)
# f.write(b"145df454fad")
# print f.seek(5),f.seek(-2,2)

'''
with 处理文件对象是个好习惯。它的先进之处在于文件用完后会自动关闭，就算发生异常也没关系。它是 try-finally 块的简写
'''
# with open('d:test.txt','r') as f:
#     pass
# print f.closed
```

### **三、pickle模块**

假如对文件内容直接用read()输出整数，则需要经过数据类型的转换才能正确获取。而python为了解决这样的麻烦，提供pickle实现。实现方式如下:

```
    
'''
   pickle模块:可以将任何python对象封装为字符串，可以从字符串拆装成对象
    若有一个对象x，以写模式打开文件对象，则pickle可以实现封装，若将f以读模式打开，则可拆装成对象
'''
pickle.dump(x,f)#封装
x=pickle.load(f)#拆装
```

