title:  核心java系列——反射
date: 2015-10-02 19:47:50
categories: 核心java
tags: [java,反射]
toc: true
---
反射是java中一种很强大的工具，能够动态分析java的类的能力。在运行状态中，对于任意一个类，通过反射都能知道这个类的属性和方法。
这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。反射的概念是由Smith在1982年首次提出的。
<!--more-->
一，反射的简介
-------

反射是java中一种很强大的工具，能够动态分析java的类的能力。在运行状态中，对于任意一个类，通过反射都能知道这个类的属性和方法。
这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。反射的概念是由Smith在1982年首次提出的。

二、反射的作用
-------

java的发射机制提供如下几个方面的作用:
1.在运行时判断任意一个对象所属的类。
2.在运行时构造任意一个类的对象。
3.在运行时构造任意一个类所有的成员变量和方法。
4.在运行时动态创建对象，从一个对象是属性拷贝到另一个对象相应的属性上。
5.创建动态代理。

三、反射中常用的API
-----------

java.lang.Class;                
java.lang.reflect.Constructor; 
java.lang.reflect.Field;        
java.lang.reflect.Method;
java.lang.reflect.Modifier;

四、反射机制API的使用
------------

**1.class类对象**
class类对象是反射机制的根源，由JVM生成（由于class类没有public方法，无法直接new 一个class对象），可以通过Object.getClass()获得class类型的对象
下面介绍如何获取Class对象:
A.通过getClass()方法获取，如String s="agagad";Class c=s.getClass();
B通过Class.forName()获取（基本数据类型除外）。如Class c=Class.forName("java.lang.String");
C.通过.class 如Class c=Person.class;Class c1=String.class等
D.newInstance创建一个类对象

**2.Field的作用**
Field可以描述一个类的成员变量，且可以通过getDeclareFields()获取类的所有成员变量。
 Class c = Class.forName("java.lang.Integer");  
 //获取所有的成员变量
Field[] fs = c.getDeclaredFields(); 
通过setAccessible()设置成员变量值可访问的 
实例如下:

```
Class c=Class.forName("com.greekw.pojo.User");
System.out.println("User类的全路径类名"+c.getName());
System.out.println("User类的类名"+c.getSimpleName());
User user=new User();
//获取类中所有的属性
Field[] fields=c.getDeclaredFields();
for(Field f:fields){
	//获取变量的修饰符
	System.out.println(Modifier.toString(f.getModifiers())+" ");
	//获取变量的类型
	System.out.println(f.getType().getSimpleName());
	//获取变量名
	System.out.println(f.getName());
	//获取成员变量的值
	String fName=f.getName();
	f.setAccessible(true);
	System.out.println(fName+"的值为:"+f.get(user));
}
```

**3.Method类**
Method类可描述类的所有方法，可以通过getDeclaredMethods()获取所有方法
实例如下:

```
//获取一个类的所有方法
Class c=Class.forName("com.greekw.pojo.User");
Method[] methods=c.getDeclaredMethods();
for(Method m:methods){
	//获取方法修饰符
	System.out.println(Modifier.toString(m.getModifiers())+" ");
	//获取方法类型
	System.out.println(m.getReturnType().getName()+" ");
	//获取方法名
	System.out.println(m.getName()+" ");
	//获取方法的参数
	Class[] params=m.getParameterTypes();
	for(int i=0;i<params.length;i++){
		Class pc=params[i];
		System.out.println(pc.getSimpleName()+",");
		
	}
}
```

调用对象的方法,实现如下:
```
//调用对象的方法
m.setAccessible(true);//设置对象的属性和方法是可访问的
if(m.getName().equals("setIntValue")){
	m.invoke(user, 50000);//修改属性的值
}
if(m.getName().equals("setName")){
	m.invoke(user, "测试");
}
```

**4.关于instanceof**
instanceof，用来判断某个对象是否是某种类型，计算结果是一个boolean类型的值
如果一个对象继承了某个类，或实现了某些接口，那么这个对象都是属于哪些类型的 
实例如下:
```
LittleDog ld=new LittleDog();
if(ld instanceof LittleDog){
	System.out.println(ld的对象类型是LittleDog);
}
```
总之，java的反射机制是代码更加灵活，更容易体现面向对象的思想，在设计模式，配置文件读取，以及三大框架中都有用到，是一种构建工具类的利器。