title:  web.xml配置的元素加载顺序和详解
date: 2014-04-12 23:22
categories: web开发
tags: [web开发,xml]
toc: true
---
>一般的工程都会有用到web.xml文件,但不是必须的，web.xml的主要作用是配置，主要配置filter ,listener,servlet,session等，下面分析下web.xml中给元素的加载顺序。
<!--more-->
#### **一、web.xml加载过程**
web工程中加载跟配置的参数与节点的顺序没有关系,主要加载顺序为ServletContext -> context-param -> listener -> filter -> servlet。并且这些元素可以配置在文件中的任意位置。
**（1）、加载过程如下:**
启动web项目时，会现在加载web.xml文件,读取listener和context-param节点，然后会加载servletcontext(servlet上下文)，这个web项目的所有部分都将共享这个上下文。 
容器将<context-param>转换为键值对，并交给servletContext。 
容器创建<listener>中的类实例，创建监听器。
#### **二、web.xml各节点详细分析:**
schema,按照sun公司的指定来定义，都必须标明这个 web.xml使用的是哪个模式文件。其它的元素都放在web-app的节点之中。

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/j2ee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" version="2.4"
xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
```
**（2）、以下为引用别人对web.xml中其他元素的总结，主要总结如下:** 
display-name:定义了WEB应用的名字   
description: 声明WEB应用的描述信息   
  
context-param:元素声明应用范围内的初始化参数。   
filter:过滤器元素将一个名字与一个实现javax.servlet.Filter接口的类相关联。   
filter-mapping:一旦命名了一个过滤器，就要利用filter-mapping元素把它与一个或多个servlet或JSP页面相关联。   
listener:servlet API的版本2.3增加了对事件监听程序的支持，事件监听程序在建立、修改和删除会话或servlet环境时得到通知。   
                     Listener元素指出事件监听程序类。   
servlet:在向servlet或JSP页面制定初始化参数或定制URL时，必须首先命名servlet或JSP页面。Servlet元素就是用来完成此项任务的。   
servlet-mapping:服务器一般为servlet提供一个缺省的URL：http://host/webAppPrefix/servlet/ServletName。   
              但是，常常会更改这个URL，以便servlet可以访问初始化参数或更容易地处理相对URL。在更改缺省URL时，使用servlet-mapping元素。   
  
session-config:如果某个会话在一定时间内未被访问，服务器可以抛弃它以节省内存。   
          可通过使用HttpSession的setMaxInactiveInterval方法明确设置单个会话对象的超时值，或者可利用session-config元素制定缺省超时值。   
  
mime-mapping:如果Web应用具有想到特殊的文件，希望能保证给他们分配特定的MIME类型，则mime-mapping元素提供这种保证。   
welcome-file-list:指示服务器在收到引用一个目录名而不是文件名的URL时，使用哪个文件。   
error-page:在返回特定HTTP状态代码时，或者特定类型的异常被抛出时，能够制定将要显示的页面。   
taglib:对标记库描述符文件（Tag Libraryu Descriptor file）指定别名。此功能使你能够更改TLD文件的位置，   
                  而不用编辑使用这些文件的JSP页面。   
resource-env-ref:声明与资源相关的一个管理对象。   
resource-ref:声明一个资源工厂使用的外部资源。   
security-constraint:制定应该保护的URL。它与login-config元素联合使用   
login-config:指定服务器应该怎样给试图访问受保护页面的用户授权。它与sercurity-constraint元素联合使用。   
security-role:给出安全角色的一个列表，这些角色将出现在servlet元素内的security-role-ref元素   
                   的role-name子元素中。分别地声明角色可使高级IDE处理安全信息更为容易。   
env-entry:声明Web应用的环境项。   
  
  
  **（3）、相应元素配置**   
   
1、Web应用图标：指出IDE和GUI工具用来表示Web应用的大图标和小图标   

```
<icon>   
<small-icon>/images/app_small.gif</small-icon>   
<large-icon>/images/app_large.gif</large-icon>   
</icon>   
```

2、Web 应用名称：提供GUI工具可能会用来标记这个特定的Web应用的一个名称   

```
<display-name>Tomcat Example</display-name>   
```

3、Web 应用描述： 给出于此相关的说明性文本   

```
<disciption>Tomcat Example servlets and JSP pages.</disciption>   
```

4、上下文参数：声明应用范围内的初始化参数。   
 

```
 <context-param>   
    <param-name>ContextParameter</para-name>   
    <param-value>test</param-value>   
    <description>It is a test parameter.</description>   
  </context-param>   
```
  在servlet里面可以通过getServletContext().getInitParameter("context/param")得到   
  
5、过滤器配置：将一个名字与一个实现javaxs.servlet.Filter接口的类相关联。   
  

```
<filter>   
        <filter-name>setCharacterEncoding</filter-name>   
        <filter-class>com.myTest.setCharacterEncodingFilter</filter-class>   
        <init-param>   
            <param-name>encoding</param-name>   
            <param-value>GB2312</param-value>   
        </init-param>   
  </filter>   
  <filter-mapping>   
        <filter-name>setCharacterEncoding</filter-name>   
        <url-pattern>/*</url-pattern>   
  </filter-mapping>   
```

6、监听器配置   
  

```
<listener>   
      <listerner-class>listener.SessionListener</listener-class>   
  </listener>  
```

 
7、Servlet配置   
   基本配置   
  

```
 <servlet>   
      <servlet-name>snoop</servlet-name>   
      <servlet-class>SnoopServlet</servlet-class>   
   </servlet>   
   <servlet-mapping>   
      <servlet-name>snoop</servlet-name>   
      <url-pattern>/snoop</url-pattern>   
   </servlet-mapping>   
```

   高级配置   
   

```
<servlet>   
      <servlet-name>snoop</servlet-name>   
      <servlet-class>SnoopServlet</servlet-class>   
      <init-param>   
         <param-name>foo</param-name>   
         <param-value>bar</param-value>   
      </init-param>   
      <run-as>   
         <description>Security role for anonymous access</description>   
         <role-name>tomcat</role-name>   
      </run-as>   
   </servlet>   
   <servlet-mapping>   
      <servlet-name>snoop</servlet-name>   
      <url-pattern>/snoop</url-pattern>   
   </servlet-mapping>   
```

   元素说明   

```
 <servlet></servlet> 用来声明一个servlet的数据，主要有以下子元素：   
     <servlet-name></servlet-name> 指定servlet的名称   
     <servlet-class></servlet-class> 指定servlet的类名称   
     <jsp-file></jsp-file> 指定web站台中的某个JSP网页的完整路径   
     <init-param></init-param> 用来定义参数，可有多个init-param。在servlet类中通过getInitParamenter(String name)方法访问初始化参数   
     <load-on-startup></load-on-startup>指定当Web应用启动时，装载Servlet的次序。   
                                 当值为正数或零时：Servlet容器先加载数值小的servlet，再依次加载其他数值大的servlet.   
                                 当值为负或未定义：Servlet容器将在Web客户首次访问这个servlet时加载它   
     <servlet-mapping></servlet-mapping> 用来定义servlet所对应的URL，包含两个子元素   
       <servlet-name></servlet-name> 指定servlet的名称   
       <url-pattern></url-pattern> 指定servlet所对应的URL   
```

8、会话超时配置（单位为分钟）   
  

```
 <session-config>   
      <session-timeout>120</session-timeout>   
   </session-config>   
```

9、MIME类型配置   
 

```
  <mime-mapping>   
      <extension>htm</extension>   
      <mime-type>text/html</mime-type>   
   </mime-mapping>
```

   
10、指定欢迎文件页配置   
   

```
<welcome-file-list>   
      <welcome-file>index.jsp</welcome-file>   
      <welcome-file>index.html</welcome-file>   
      <welcome-file>index.htm</welcome-file>   
   </welcome-file-list>   
```

11、配置错误页面   
 通过错误码来配置error-page   
   

```
<error-page>   
      <error-code>404</error-code>   
      <location>/NotFound.jsp</location>   
   </error-page>   
```

  上面配置了当系统发生404错误时，跳转到错误处理页面NotFound.jsp。   
通过异常的类型配置error-page   
   

```
<error-page>   
       <exception-type>java.lang.NullException</exception-type>   
       <location>/error.jsp</location>   
   </error-page>   
```

  上面配置了当系统发生java.lang.NullException（即空指针异常）时，跳转到错误处理页面error.jsp   
12、TLD配置   
  

```
 <taglib>   
       <taglib-uri>http://jakarta.apache.org/tomcat/debug-taglib</taglib-uri>   
       <taglib-location>/WEB-INF/jsp/debug-taglib.tld</taglib-location>   
   </taglib>   
   如果MyEclipse一直在报错,应该把<taglib> 放到 <jsp-config>中   
   <jsp-config>   
      <taglib>   
          <taglib-uri>http://jakarta.apache.org/tomcat/debug-taglib</taglib-uri>   
          <taglib-location>/WEB-INF/pager-taglib.tld</taglib-location>   
      </taglib>   
   </jsp-config>   
```

13、资源管理对象配置   
  

```
 <resource-env-ref>   
       <resource-env-ref-name>jms/StockQueue</resource-env-ref-name>   
   </resource-env-ref>   
```

14、资源工厂配置   
   

```
<resource-ref>   
       <res-ref-name>mail/Session</res-ref-name>   
       <res-type>javax.mail.Session</res-type>   
       <res-auth>Container</res-auth>   
   </resource-ref>   
```

   配置数据库连接池就可在此配置：   
  

```
 <resource-ref>   
       <description>JNDI JDBC DataSource of shop</description>   
       <res-ref-name>jdbc/sample_db</res-ref-name>   
       <res-type>javax.sql.DataSource</res-type>   
       <res-auth>Container</res-auth>   
   </resource-ref>   
```

15、安全限制配置   
   

```
<security-constraint>   
      <display-name>Example Security Constraint</display-name>   
      <web-resource-collection>   
         <web-resource-name>Protected Area</web-resource-name>   
         <url-pattern>/jsp/security/protected/*</url-pattern>   
         <http-method>DELETE</http-method>   
         <http-method>GET</http-method>   
         <http-method>POST</http-method>   
         <http-method>PUT</http-method>   
      </web-resource-collection>   
      <auth-constraint>   
        <role-name>tomcat</role-name>   
        <role-name>role1</role-name>   
      </auth-constraint>   
   </security-constraint>  
```

 
16、登陆验证配置   
   

```
<login-config>   
     <auth-method>FORM</auth-method>   
     <realm-name>Example-Based Authentiation Area</realm-name>   
     <form-login-config>   
        <form-login-page>/jsp/security/protected/login.jsp</form-login-page>   
        <form-error-page>/jsp/security/protected/error.jsp</form-error-page>   
     </form-login-config>   
   </login-config>   
```

17、安全角色：security-role元素给出安全角色的一个列表，这些角色将出现在servlet元素内的security-role-ref元素的role-name子元素中,分别地声明角色可使高级IDE处理安全信息更为容易。   

```
  <security-role>   
     <role-name>tomcat</role-name>   
  </security-role>   
```

18、Web环境参数：env-entry元素声明Web应用的环境项   
 

```
 <env-entry>   
     <env-entry-name>minExemptions</env-entry-name>   
     <env-entry-value>1</env-entry-value>   
     <env-entry-type>java.lang.Integer</env-entry-type>   
  </env-entry>   
```

19、EJB 声明   

```
  <ejb-ref>   
     <description>Example EJB reference</decription>   
     <ejb-ref-name>ejb/Account</ejb-ref-name>   
     <ejb-ref-type>Entity</ejb-ref-type>   
     <home>com.mycompany.mypackage.AccountHome</home>   
     <remote>com.mycompany.mypackage.Account</remote>   
  </ejb-ref>   
```

20、本地EJB声明   
  

```
<ejb-local-ref>   
     <description>Example Loacal EJB reference</decription>   
     <ejb-ref-name>ejb/ProcessOrder</ejb-ref-name>   
     <ejb-ref-type>Session</ejb-ref-type>   
     <local-home>com.mycompany.mypackage.ProcessOrderHome</local-home>   
     <local>com.mycompany.mypackage.ProcessOrder</local>   
  </ejb-local-ref> 
```

  
21、配置DWR   
 

```
 <servlet>   
      <servlet-name>dwr-invoker</servlet-name>   
      <servlet-class>uk.ltd.getahead.dwr.DWRServlet</servlet-class>   
  </servlet>   
  <servlet-mapping>   
      <servlet-name>dwr-invoker</servlet-name>   
      <url-pattern>/dwr/*</url-pattern>   
  </servlet-mapping>   
```

22、配置Struts   
   

```
 <display-name>Struts Blank Application</display-name>   
    <servlet>   
        <servlet-name>action</servlet-name>   
        <servlet-class>   
            org.apache.struts.action.ActionServlet   
        </servlet-class>   
        <init-param>   
            <param-name>detail</param-name>   
            <param-value>2</param-value>   
        </init-param>   
        <init-param>   
            <param-name>debug</param-name>   
            <param-value>2</param-value>   
        </init-param>   
        <init-param>   
            <param-name>config</param-name>   
            <param-value>/WEB-INF/struts-config.xml</param-value>   
        </init-param>   
        <init-param>   
            <param-name>application</param-name>   
            <param-value>ApplicationResources</param-value>   
        </init-param>   
        <load-on-startup>2</load-on-startup>   
    </servlet>   
    <servlet-mapping>   
        <servlet-name>action</servlet-name>   
        <url-pattern>*.do</url-pattern>   
    </servlet-mapping>   
    <welcome-file-list>   
        <welcome-file>index.jsp</welcome-file>   
    </welcome-file-list> 
```

23、配置Spring（基本上都是在Struts中配置的）   
  
 

```
  <!-- 指定spring配置文件位置 -->   
   <context-param>   
      <param-name>contextConfigLocation</param-name>   
      <param-value>   
       <!--加载多个spring配置文件 -->   
        /WEB-INF/applicationContext.xml, /WEB-INF/action-servlet.xml   
      </param-value>   
   </context-param>   
  
   <!-- 定义SPRING监听器，加载spring -->   
  
  <listener>   
     <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>   
  </listener>   
  
  <listener>   
     <listener-class>   
       org.springframework.web.context.request.RequestContextListener   
     </listener-class>   
  </listener>   
```




### **三、映射规则**
当一个请求发送到servlet时，容器会根据当前的url减去context上下文后剩余的部分去匹配，比如请求地址为http://localhost/test/LoginServlet，我的应用上下文是test，容器会将http://localhost/test去掉，剩下的/LoginServlet部分拿来做servlet的映射匹配。这个映射匹配过程是有顺序的，而且当有一个servlet匹配成功以后，就不会去理会剩下的servlet了。
**匹配规则**
1.精确路径匹配。例子：比如servletA 的url-pattern为 /test，servletB的url-pattern为 /* ，这个时候，如果我访问的url为http://localhost/test ，这个时候容器就会先 进行精确路径匹配，发现/test正好被servletA精确匹配，那么就去调用servletA，也不会去理会其他的servlet了。
2.最长路径匹配。例子：servletA的url-pattern为/test/*，而servletB的url-pattern为/test/a/*，此时访问http://localhost/test/a时，容器会选择路径最长的servlet来匹配，也就是这里的servletB。
3.扩展匹配，如果url最后一段包含扩展，容器将会根据扩展选择合适的servlet。例子：servletA的url-pattern：*.action
以”/’开头和以”/*”结尾的是用来做路径映射的。以前缀”*.”开头的是用来做扩展映射的。所以，为什么定义”/*.action”
这样一个看起来很正常的匹配会错？因为这个匹配即属于路径映射，也属于扩展映射，导致容器无法判断。