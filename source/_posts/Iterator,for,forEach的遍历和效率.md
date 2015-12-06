title:  核心java系列——循环效率的比较
date: 2014-03-24 19:47
categories: 核心java
tags: [java]
toc: true
---
>java中对集合或数组等类型的数据遍历方式有很多，每种方式的时间复杂度和空间复杂度都各有差异，下面介绍下Iterator,for,forEach三种方式的遍历和执行效率问题。
<!--more-->
java中对集合或数组等类型的数据遍历方式有很多，每种方式的时间复杂度和空间复杂度都各有差异，下面介绍下Iterator,for,forEach三种方式的遍历和执行效率问题。
### 1.代码如下:

```

public class Test {

	private static final int COUNT = 10000;  
    	private static List<Person> persons = new ArrayList<Person>();  
  
	public static void init(){
		//初始化，生成对象个数
		Person person=null;
		for(int i=0;i<COUNT;i++){
			person=new Person(i,"张三"+i,i+"");
			persons.add(person);
		}
	}
	//Iterator遍历
	public static long testIterator(){
		//开始编译执行时间
		long start =System.nanoTime();
		Person person=null;
		for (Iterator<Person> iterator = persons.iterator(); iterator.hasNext();) {
			person = (Person) iterator.next();
			
		}
		//执行完后的时间
		long end =System.nanoTime();
		return (end- start) / (1000);
	}
	//foEach循环遍历
	public static long testForEach(){
		//开始编译执行时间
		long start =System.nanoTime();
		Person person=null;
		for(Person p:persons){
			person=p;
		}
		//执行完后的时间
		long end =System.nanoTime();
		return (end- start) / (1000);
	}
	//for循环遍历
	public static long testFor(){
		//开始编译执行时间
		long start =System.nanoTime();
		Person person=null;
		for(int i=0;i<persons.size();i++){
			person=persons.get(i);
		}
		//执行完后的时间
		long end =System.nanoTime();
		return (end- start) / (1000); 
	}
	public static void testRegxp(){
	}
	public static void main(String[] args) {
		init();
		System.out.println("Iterator迭代遍历的消耗时间为:"+testIterator());
		System.out.println("ForEach遍历的消耗时间为:"+testForEach());
		System.out.println("For循环遍历的消耗时间为:"+testFor());
		
	}


}
```

### 2.测试结果：

A.此结果是在10万级下测试的,此时forEach循环遍历耗时较少,For和Iterator耗时较多。
![这里写图片描述](http://img.blog.csdn.net/20151206195359945)


B.此结果是在1万级下测试的,此时for循环遍历耗时较少,ForEach和Iterator耗时较多。
![这里写图片描述](http://img.blog.csdn.net/20151206195412493)


综上若遍历的数据较多时，forEach效率较高，数量级较少是for循环效率较高。