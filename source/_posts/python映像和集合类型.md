title:  python映像和集合类型
date: 2015-05-10 19:41:00
categories: python开发
tags: [python,映像和集合]
toc: true
---
字典是python中唯一的映射类型，映射类型对象里哈希值和指向对象值是1:n的关系
字典对象是可变的，可以认为是一个容器类型，能存储任意个python对象.
<!--more-->

**字典**
------

一、字典的概述
字典是python中唯一的映射类型，映射类型对象里哈希值和指向对象值是1:n的关系
字典对象是可变的，可以认为是一个容器类型，能存储任意个python对象
字典对象和序列对象的区别：
		1.存储和访问数据的方式不同
		2.映射类型的数据是无序排序
		3.序列类型只能是数字类型的键，映射类型可以是其他类型的键
		4.映射类型不要求用数字值做索引从一个容器中取对应的数据。可以用键直接映射到值
（1）.字典的创建和赋值
	创建字典只需要把一个字典赋值给一个变量
	如:dict={'name':'earth','port':80}
		2.可用工厂方法dict()创建
		如:fdict=dict((['x','1'],['y',2]))
		3.可用fromkeys()创建一个默认的字典
		如:ddict ={}.fromkeys(('x','y'),-1)

(2):字典的访问
1.循环其键值。如: 
for key in dict.keys():
	print 'key=%s,value=%s' %(key,dict[key])
2.用迭代器遍历字典,只需要知道字典的名字即可，如
	dict={'name':'zhang','port':80}
	for key in dict:
			print 'key=%s,value=%s' %(key,dict[key])
3.获得字典的某个元素值
	dict[key]  如:dict[name]='zhang'
	若需检查字典是否有某一个key 可以用in 或not in 等
(3):字典的更新
1.添加一个新数据项或新元素 
  可以用字典变量名[key]=value；如 test={age:20.} test[name] ='student' 则可得到tet={age:20,name:'student'}
2.改变一个已存在的数据项
  字典的修改和添加类似，可以用字典变量名[key]=新value 
3.删除一个已存在的数据项
  del dict[key]:删除对应的键值，若用字典变量名则删除整个字典.如  del dict 

  dict.clear() :清空字典内容
  pop():删除对对应的值
  
二、映射类型操作符
(1).字典的键查找操作符[]
 1.键查找操作符是唯一用于字典类型的操作符，和序列中的单一元素切片相似
 2.字典是用键查询，序列用索引做唯一参数或下标获取
 
 (2).成员关系操作（in not in）
 in 和not in 检查某个键是否在字典中
 如:name in dict 

三、内建函数和工厂函数
(1).标准类型函数
    cmp()
    type()
    str(0
(2).映射相关函数
    dict():用于创建字典，若不提供参数，则会生成空字典。
    如 dict(zip(('x','y'),(1,2))) => {'x':1,'y':2}
    或者 dict(['x',1],['y',2]) => {'x':1,'y':2}
    len():返回所有元素的数目
    如 dict={'x':1,'y':2} 则len(dict)=2
    hash()'
   
(3):内建方法
	   1.keys():返回一个列表包含字典所有的键   dict.keys() =>['x','y']
	   2.values():返回一个列表，包含字典所有的值 如dict.values => [1,2]
	   3.items():返回一个列表包含所有含有(键，值)的元组  如dict.items() => [('x',1),('y',2)]
	   4.update() 如dict.update(dict2) 将字典dict2的key-value添加到字典dict 如dict2={'x':1,'y':2,'z':3} dict.update(dict2) 
			=>   {'x':1,'y':2,'z':3}
	   5.setdefault() 和set()类似，若字典不存在key 由dict[key]=default为它赋值  
	   6.clear() 删除字典中所有元素  如  dict.clear() => {}
	   7.copy()  返回一个字典的副本  如dict2=dict.copy() => {'x':1,'y':2}
   
四、字典的键
	字典的键必须是不可变数据类型，如果用元组做键，那必须要保证元组内的对像也是不可变类型。可变数据类型对像不能做键。

**集合**
------

一、集合类型
set是不同元素组成的集合，python中的集合对象是一组无序排列的可hash的值。
集合分为两种类型.可变集合(set)和不可变集合(frozenset)。可变集合可以删除或添加元素，不可变集合不充许;可变集合不是可			哈希的 不能用作字典的键,也不能用作其他集合中的元素,不可变集合真好相反
(1)、集合的创建
  主要有set()和frozenset()方法
  如:s=set('abcd')   => set(['a','b','c','d']);  frozenset('BOOK') =>frozenset(['B','O','O','K'])  
	
(2).集合的访问
  可以遍历和检查某项元素是否是一个集合的元素
  如:  'K' in s
(3).更新集合
  可以用集合内建方法进行添加，删除集合的成员
  如:
	  s.add('z')
	  s.update('pypj')
	  s-=set('pypj')
	  s.remove()
  
(4).如何删除集合中的成员和集合
 A.集合超出其作用范围
 B.调用del将他们直接清除当前的名称空间
二、集合类型操作符
	set集合是无序的，不能通过索引和切片来做一些操作
三、内建函数和内建方法
    (1).内建函数:
	    len():返回集合的基数
	    set()和frozenset()返回可变集合和不可变集合 若不提供参数则会生成空集合
   
   (2).内建方法:可分为适用于所有的集合方法和适用于可变集合的方法
    	1.仅适用于可变集合的如下:
 			s.update(t)  s.add(obj)  s.remove(obj) s.discard(obj) s.pop():删除集合s中的任意一个对象
 			s.clear() 删除集合s中的所有元素

本节例子实现:
-------

该例子用于模拟管理登录系统
```
#!/usr/bin/env python
#-*-coding:utf-8 -*-
db = {}
def newuser():
    prompt = 'login desired'
    while True:
        name = raw_input(prompt)
        if db.has_key(name):
            prompt = 'name taken, try anothers'
            continue
        else:
            break
    pwd = raw_input('password:')
    db[name] = pwd

def olduser():
    name = raw_input('login: ')
    pwd = raw_input('passwd: ')
    passwd = db.get(name)
    if passwd == pwd:
        print 'welcome back',name
    else:
        print 'login incorrect'

def showmenu():
    prompt = '''
(N)ew User Login
(E)xisting User Login
(Q)uit
Enter choice:
'''
    done = False
    while not done:
        chosen = False
        while not chosen:
            try:
                choice = raw_input(prompt).strip()[0].lower()
            except (EOFError, KeyboardInterrupt):
                choice = 'q'
            print '\nyou picked:[%s]' %choice
            if choice not in 'neq':
                print 'invalid option, try again'
            else:
                chosen = True

        if choice == 'q' :done = True
        if choice == 'n' :newuser()
        if choice == 'e' :olduser()

if __name__ == '__main__':
    showmenu()
```