title:  Hibernate概述和增删改查入门
date: 2014-03-31 01:00
categories: web开发
tags: [web开发,hiberante]
toc: true
---
Hibernate是一种轻量级的javaEE持久层解决方案，是一种关系型数据库的ORM映射框架，是完全的对数据库SQL进行封装，使用java对象操作，生成相应的SQL，完成对底层数据的操作.
<!--more-->
### **一、hiberante框架概述**
(1).Hibernate是一种轻量级的javaEE持久层解决方案，是一种关系型数据库的ORM映射框架，是完全的对数据库SQL进行封装，使用java对象操作，生成相应的SQL，完成对底层数据的操作.
(2).使用Hibernate的好处
1.简化编码操作
2.应用其内部的优化机制(一级，二级缓存,抓取策略等),来提高数据的操作性能
3.可使用java对象操作数据库,能对底层数据操作进行完全的封装

### **二、Hibernate入门开发**
**1.搭建Hibernate环境，导入相应的jar包**
可从http://sourceforge.net/projects/hibernate/files    下载开发包 
所需jar包如下:
![这里写图片描述](http://img.blog.csdn.net/20151206202233839)
一些包的描述:

 - hibernate3.jar—— 核心jar包
 - hibernate-jpa-2.0-api-1.0.1.Final.jar——hibernate注解、web项目中依赖 包
 - slf4j, slf4j-log4j12-1.7.2.jar——hibernate框架使用默认日志技术,需要与log4j整合实现

**2.建立相依的实体类,以及相应的hbm(映射文件)**
A.创建实体类Person.java,包含的属性为id,name,city.其对应的映射文件为（Person.hbm.xml）
B.主要配置如下
	

```
	<hibernate-mapping>
		<!-- 
		     name 完整类名,table 表名,catalog 数据库名
		 -->
		<class name="com.w2cboy.model.Person" table="person" catalog="ceshidb">
			<!-- 主键生成策略配置 -->
			<!-- 
			 name Perosn类中生成注解 属性名
			 column 表中列名
			 type 数据类型  （Java数据类型、Hibernate数据类型 、 SQL数据类型 ）
			 -->
			<id name="id" column="id" type="int">
			     <!-- 使用本地策略 -->
			     <generator class="native"></generator>
			</id>
			<!-- 配置其它属性 -->
			<property name="name" column="name" type="java.lang.String"></property>
			<!-- SQL类型 必须编写 column 元素-->
			<property name="city" column="city" type="string"></property>
		</class>
	</hibernate-mapping>
```


**3.创建并配置hibernate核心配置文件**
在src目录下创建hibernate.cfg.xml文件，主要配置JDBC连接信息，其配置信息如下：
    

```
    <hibernate-configuration>
		<!-- Hibernate中数据库连接 通过 session对象进行管理  -->
		<session-factory>
			<!-- 连接数据库，必须配置 JDBC 四个基本连接参数 -->
			<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
			<property name="hibernate.connection.url">jdbc:mysql:///ceshidb</property>
			<property name="hibernate.connection.username">root</property>
			<property name="hibernate.connection.password">123</property>
			
			<!-- 配置数据库方言 不同 数据库 SQL 不完全一样-->
			<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
	
			<!-- 在控制台打印生成的SQL语句 -->
			<property name="hibernate.show_sql">true</property>
			<!-- 对控制台输出SQL语句进行格式化，为了方便阅读 -->
			<property name="hibernate.format_sql">true</property>
			<!-- 可以自动根据类生成DDL 建表语言，完成自动建表 -->
			<property name="hibernate.hbm2ddl.auto">update</property>
			
			<!-- 在核心配置文件中 加载 hbm 映射文件 -->
			<mapping resource="com/w2cboy/model/Person.hbm.xml"/>
		</session-factory>
           	</hibernate-configuration>	
	
```


**4.编写测试用例(实现CRUD操作)**
1.session工具类:
	//实例化配置对象，加载配置文件 hibernate.cfg.xml
	Configuration configuration = new Configuration().configure();
	//创建会话连接工厂
	SessionFactory sessionFactory = configuration.buildSessionFactory();
	//创建会话	
	Session session = sessionFactory.openSession();
	//开启事务
	Transaction transaction = session.beginTransaction();
	//提交事务，释放资源		
	transaction.commit();
	session.close();
	sessionFactory.close();

2.CRUD操作

1) 根据id 查询 通过 Session类 提供 get/load 方法 ;
	 

```
    int id = 1;
	     Person person= (Person ) session.get(person .class, id);
```

2）修改update方法， 默认根据 id 修改 表其它所有字段  通过 Session类 提供 update;
	   

```
  person person = new Person ();
	     person .setName("张三");
	     person .setCity("上海");
	     session.update(person);
```

3）删除操作， 根据id 删除， 通过Session类提供 delete ;
	   

```
  person person = new Person ();
  person.setId(1);
  session.delete(perosn);
```

4） 查询所有数据 
	通过Query 接口完成， Query接口 接收HQL 查询语句 
	Query提供子接口 SQLQuery ， 接收SQL 查询语句 
	Hibernate框架内部提供，格式和SQL语句非常类似，区别SQL 面向数据表查询语句， HQL面向Java类和对象查询语句 

	

```
	HQL查询
	   String hql = "from Person";//面向类和对象
	   Query query = session.createQuery(hql); //获得Query对象
           List<Person> persons= query.list();//执行查询

	SQL 查询
	   String sql = "select * from person";
	   SQLQuery sqlQuery = session.createSQLQuery(sql);
```

所以总的来说，hiberante的配置文件，和映射文件是实现的关键。


