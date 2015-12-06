title:  Spring集成AIXS2发布webservice
date: 2014-03-01 19:28:00
categories: SOA
tags: [webservice,AIXS2]
toc: true
---
>webservice技术，实现跨平台，跨语言进行数据的交互，系下面主要介绍总线如何整合AXIS2和Spring，发布和调用webservice。
<!--more-->
### **一、简介**
>webservice技术，实现跨平台，跨语言进行数据的交互，系下面主要介绍总线如何整合AXIS2和Spring，发布和调用webservice。

### **二、Spring整合AXIS2的步骤**
1.从官网下载Axis2的jar包
2.建立一个web project，引入axis2相应的依赖包(路径为%\axis2-1.5.4-bin%bin下)放到lib目录下
目录结构如下：
 ![这里写图片描述](http://img.blog.csdn.net/20151206210222676)

 
3.定义工程的包结构如下:
 
 ![这里写图片描述](http://img.blog.csdn.net/20151206210232759)
 
4.在service中定义提供的服务接口service

 ![这里写图片描述](http://img.blog.csdn.net/20151206210241470)
 
 实现类为：
![这里写图片描述](http://img.blog.csdn.net/20151206210314945)
5.定义配置文件，并进行配置
a.在src下建立applicationContext.xml文件,配置如下：
![这里写图片描述](http://img.blog.csdn.net/20151206210343561)
 ![这里写图片描述](http://img.blog.csdn.net/20151206210436640)
b.在WebRoor/WEB-INF/services/目录下建立目录sampleService(这个名字可以随便取)然后建立在其下META-INF目录，然后再在其目录下建立services.xml ,目录结构如下:
 ![这里写图片描述](http://img.blog.csdn.net/20151206210451557)
 
Services.xml的配置信息如下：
 ![这里写图片描述](http://img.blog.csdn.net/20151206210504790)
C.配置web.xml文件，内容如下：
 ![这里写图片描述](http://img.blog.csdn.net/20151206210519127)
 
5.测试webservice的发布
启动tomcat在浏览器中输入
http://localhost:8080/Axis2Service/services.可以看到下内容说明我们的服务已经发布成功了 
 ![这里写图片描述](http://img.blog.csdn.net/20151206210545273)
以上就是spring整合AXIS2的基本过程，


### **三、客户端和服务端的请求和响应**
1.客户端的调用方式有如下几种:
* 使用wsimport命令，生成本地代码（JDK）
* 使用service类来调用webservice（JDK）
* URLConnection来调用webservice（移动端）
* 前端调用：页面（Ajax）（SOAP协议的内容或格式）

2.Axis2调用webservice方式主要调用API为AXIS2包中RPCClient类，主要实现如下:
![这里写图片描述](http://img.blog.csdn.net/20151206210553638)

