title:  Spring Mvc入门分析
date: 2014-05-16 23:54
categories: web开发
tags: [web开发,SpringMvc]
toc: true
---
Spring Web MVC是一种基于Java的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将web层进行职责解耦，基于请求驱动指的就是使用请求-响应模型，框架的目的就是帮助我们简化开发。
<!--more-->
### **一、什么是spring mvc**
>Spring Web MVC是一种基于Java的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将web层进行职责解耦，基于请求驱动指的就是使用请求-响应模型，框架的目的就是帮助我们简化开发。
Spring Web MVC也是服务到工作者模式的实现，但进行可优化。前端控制器是DispatcherServlet；应用控制器其实拆为处理器映射器(Handler Mapping)进行处理器管理和视图解析器(View Resolver)进行视图管理；页面控制器/动作/处理器为Controller接口（仅包含ModelAndView handleRequest(request, response) 方法）的实现（也可以是任何的POJO类）；支持本地化（Locale）解析、主题（Theme）解析及文件上传等；提供了非常灵活的数据验证、格式化和数据绑定机制；提供了强大的约定大于配置（惯例优先原则）的契约式编程支持。
### **二、spring mvc的工作流程**
**（1）、下图为spring mvc的工作流程图：**
![这里写图片描述](http://img.blog.csdn.net/20151206200108756)

**（2）、具体执行步骤如下：**
1、 首先用户发送请求————>前端控制器，前端控制器根据请求信息（如URL）来决定选择哪一个页面控制器进行处理并把请求委托给它，即以前的控制器的控制逻辑部分；图2-1中的1、2步骤；
2、  页面控制器接收到请求后，进行功能处理，首先需要收集和绑定请求参数到一个对象，这个对象在Spring Web MVC中叫命令对象，并进行验证，然后将命令对象委托给业务对象进行处理；处理完毕后返回一个ModelAndView（模型数据和逻辑视图名）；图2-1中的3、4、5步骤；
3、  前端控制器收回控制权，然后根据返回的逻辑视图名，选择相应的视图进行渲染，并把模型数据传入以便视图渲染；图2-1中的步骤6、7；
4、  前端控制器再次收回控制权，将响应返回给用户，图2-1中的步骤8；至此整个结束。
**（3）、涉及到是API**
 - Dispatcher Servlet分发器
 - Handler Mapping 处理器映射
 - Controller 控制器
 - ModelAndView
 - ViewResolver 视图解析器

### **三、体验spring mvc**
**（1）、开发步骤：**
**1.新建一个webProject**

**2.导入jar**
 依赖包如下：

 - spring-aop-3.2.2.jar 面向切片编程；
 - spring-aspects-3.2.2.jar 提供对AspectJ的支持，以便可以方便的将面向方面的功能集成进IDE中；
 - spring-beans-3.2.2.jar 核心。访问配置文件、创建和管理bean 以及进行IoC/DI操作相关的所有类；
 - spring-context-3.2.2.jar为Spring 核心提供了大量扩展；
 - spring-context-support-3.2.2.jar
 - spring-core-3.2.2.jar Spring 框架基本的核心工具类。外部依赖Commons Logging ；
 - spring-expression-3.2.2.jar 配置对象的注入，它便是SpEL ;
 - spring-web-3.2.2.jar Web 应用开发时，用到Spring 框架时所需的核心类；
 - spring-webmvc-3.2.2.jar  Spring MVC 框架相关的所有类。包括框架的Servlets，Web
   MVC框架，控制器和视图支持,注意spring3.0的包名是 org.spingframework.web.servlet-3.1.0
   RELEASE.jar；
 - commons.logging-1.1.1.jar 日志；
 - aopalliance-1.0.0.jar AOP联盟的API包，里面包含了针对面向切面的接口。

**3.在web.xml配置 DispatcherServlet和映射**

```
<!-- 配置分发器 -->
  <servlet>
  	<servlet-name>action</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:spring-mvc.xml</param-value>
  	</init-param>
  </servlet>
  <!-- 配置映射的类 -->
  <servlet-mapping>
  	<servlet-name>action</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>

```

**4.创建WebController**

```
package cn.com.action;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.AbstractController;

public class WebController extends AbstractController {
	
	private final  Log logger=LogFactory.getLog(getClass());
//	private Properties properties =SysConfig.getProperties("");

	@Override
	public ModelAndView handleRequestInternal(HttpServletRequest resquest,
			HttpServletResponse response) throws Exception {
		logger.info("servlet request......start");
		return new ModelAndView("success");
	}

}
```

**5.在src根目录下创建并配置spring-mvc.xml文件**

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:schemaLocation="http://www.springframework.org/schema/beans 			  
    http://www.springframework.org/schema/beans/spring-beans-3.2.xsd">
 <!-- 控制器 -->
<bean id="/webController.do" class="cn.com.action.WebController">
</bean>
<!-- 视图资源解析器 -->
<bean id="internalResourceViewResolver" 
	class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/pages/"/><!-- 前缀 -->
		<property name="suffix" value=".jsp"/><!-- 后缀 -->
</bean>
</beans>

```

**6.创建jsp页面 /WEB-INF/pages/index.jsp**
**7.进行部署和发布**
以上就是spring mvc最基本和最简单的开发流程，通过请求：http://localhost:8080/springmvc/webController.do，如果页面输出正常,就表示成功了！