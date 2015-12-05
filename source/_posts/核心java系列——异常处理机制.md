title:  核心java系列——异常处理机制
date: 2015-11-24 15:47:17
categories: 核心java
tags: [java]
toc: true
---
异常是程序中一种错误，可能是读取文件错误，网络连接错误，也可能是数组越界错误，或试图使用一个没有被赋值的对象而引起的错误。如果由于异常或错误使程序操作没有操作完成，则程序返回一个安全状态或捕获异常的处理结果，保证程序的健壮性。
<!--more-->
### **一、什么是异常**

> 异常是程序中一种错误，可能是读取文件错误，网络连接错误，也可能是数组越界错误，或试图使用一个没有被赋值的对象而引起的错误。如果由于异常或错误使程序操作没有操作完成，则程序返回一个安全状态或捕获异常的处理结果，保证程序的健壮性。

####（1）.异常分类
java语言中，定义了许多的异常类，所有异常类都派生Throwable类，而Throwable类又有两个子类Error和Exception，分别表示错误和异常，
其中Exception又分为运行时异常和非运行时异常。下面为异常的体系结构图：
![这里写图片描述](http://img.blog.csdn.net/20151124185612756)

####（2）.下面将详细说明图中相关异常的区别和联系
**1.Error和Exception**
  Error是程序无法处理的错误，通常有内存溢出异常(outofMemoryError)，线程终止等，出现这类异常java虚拟机会选择终止程序。
 Exception是程序可处理的异常，它又有两个分支运行时异常和非运行时异常，出现这类异常是程序需要处理。
**2.运行时异常和非运行时异常**
 运行时异常都是RuntimeException类及其子类异常，如NullPointerException、 IndexOutOfBoundsException等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引 起的，程序应该从逻辑角度尽可能避免这类异常的发生。
 非运行时异常是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，
如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常。
#### (3).什么时候抛出异常
如果遇到无法处理的问题时，该方法可以选择抛出异常，至于何时需要在方法中用throws声明异常，若该方法需要抛出多个异常，则每个异常间用逗号分隔，通常有以下情况需要抛出异常：
1.调用一个已抛出异常的方法，如FileInputStream构造器。
2.程序运行过程错误，并用throw抛出一个检查出的异常。
3.程序出现错误。
4.java虚拟机和运行时库出现的内部异常。
总之一个方法必须声明所有可能抛出的异常已检查异常，所有未检查出的异常要么不可控制，要么就应该避免发生。
#### (4).创建异常
创建异常通常只需要定义派生于Exception类或派生于Exception子类的类。例如定义一个派生于IOException的类，通常定义两个构造器，一个是默认构造器，一个是带有详细描述信息的构造器。如:

```
public class MyExcetion extends IOException {
	private static final long serialVersionUID = 1L;

	public MyExcetion() {
	}
	public MyExcetion(String message) {
		super(message);
	}

	public MyExcetion(Throwable cause) {
		super(cause);
	}

	public MyExcetion(String message, Throwable cause) {
		super(message, cause);
	}

}
```

### **二、异常处理**
程序运行过程中，若出现异常没有及时处理，则会终止程序的执行。可以用捕获异常或抛出异常来处理。
若try语句代码块出现异常，程序会在catch中寻找匹配的异常，然后调用匹配的异常处理器进行处理。运行过程中程序未找到匹配的异常处理器，程序就会终止。
**(1).try...catch模式**
该模式的定义形式如下
try{ 
  代码块；
  }catch(Exception e){ 
  }
  Catch(Exception e2){ }

匹配的原则是：如果抛出的异常对象属于catch子句的异常类，或者属于该异常类的子类，则认为生成的异常对象与catch块捕获的异常类型相匹。
**(2).try...catch...finally模式**	
该模包含finally,无论程序有无异常发生，且不管try –catch间是否顺利执行，都会执行finally语句。
**(3).实例说明：**
1.各模块的作用
try块:用于捕获异常，后面可以跟一个或多个catch，finally可有可无，但必须有一个catch块。
catch块:用于处理异常。
finally块是程序是否有无异常，都需要执行的部分。
但在以下3中特殊情况下不会执行：
A．在finally 语句中发生异常。
B．程序所在线程死亡。
C．在前面的代码中出现了system.exit()退出程序。
2.当捕获到一个异常对象之后，你可以调用其getMessage()方法来获取异常消息，或者调用printStackTrace()方法来打印执行栈的内容
关于getMessage()方法
当你通过new Exception(“message”)或new RuntimeException(“message”)来创建一个异常对象的时候，传给构造方法的这个参数就是消息，所以，当你捕获到异常对象之后，可以通过getMessage()方法把消息拿出来！
关于printStackTrace()方法它能把执行栈的内容打印出来
### **三、异常的转换**
所谓异常转换，即截获一个异常（往往是checked exception）之后，将其转换为另外一个异常（往往是unchecked exception）抛出。
比如很多情况下，当我们操作数据库的时候或操作文件的时候，JDK类库中的相关操作方法会给我们抛出SQLException或IOException，这些都是checked exception，这些异常一般情况下我们是无法进行处理的，所以，我们需要继续向上抛出异常，假如我们继续抛出SQLException或IOException的话，会导致更上层的程序处理起来非常困难（而且也需要在方法中进行声明）。
因此，大多数情况下，我们在截获到SQLException或IOException之后，会将它转换为一种RuntimeException重新抛出，这是非常常见的异常应用技巧！
### **四、异常链**
所谓异常链，即当我们在截获一个异常，在把它转换为另外一个异常的时候，记得把原来那个异常对象设置到新的异常对象中即可。我们不妨观察Throwable类（所有异常类的基类）的构造方法:
public Throwable()
public Throwable(String message)
public Throwable(String message, Throwable cause)
public Throwable(Throwable cause)
我们注意到其中的Throwable类型的参数，它就是用来创建异常链的。
下面使用一个实例来说明:
**1.自定义异常类：**

```
public class MyException extends RuntimeException {
	private static final long serialVersionUID = 1L;
	private String errorCode;

	public MyException() {
	}

	public MyException(String message,String errorCode) {
		super(message);
		this.errorCode=errorCode;
	}
	//用于创建异常链
	public MyException(String message,String errorCode,Throwable cause) {
		super(message, cause);
		this.errorCode=errorCode;
	}

	public String getErrorCode() {
		return errorCode;
	}

}
```
**2.调用该类**

```
public class FileUtils {
	public static String readFile(String filePath){
		StringBuffer sb=new StringBuffer();
		try {
			BufferedReader reader=new BufferedReader(new FileReader(filePath));
			String line=null;
			while((line=reader.readLine())!=null){
				sb.append(line);
			}
		} catch (FileNotFoundException e) {
			//将原始对象放到新对象中去
			throw new MyException("文件没有找到！", "1", e);
		} catch (IOException e) {
			throw new MyException("文件读取有误！", "2", e);
		}
		return sb.toString();
	}
}
```

### **五、注意事项**
1)  必须在 try 之后添加 catch 或 finally 块。try 块后可同时接 catch 和 finally 块，但至少有一个块。
2) 必须遵循块顺序：若代码同时使用 catch 和 finally 块，则必须将 catch 块放在 try 块之后。
3) 一个 try 块可能有多个 catch 块。若如此，则执行第一个匹配块
即Java虚拟机会把实际抛出的异常对象依次和各个catch代码块声明的异常类型匹配，如果异常对象为某个异常类型或其子类的实例，就执行这个catch代码块，不会再执行其他的 catch代码块。
4) 可嵌套 try-catch-finally 结构。
5) 在 try-catch-finally 结构中，可重新抛出异常。
6)多个catch的顺序一定要遵循子类在上父类在下的规则。
7)一个方法被重写，被重写的方法必须抛出相同的异常或异常的子类。
8)若父类方法抛出多个异常，那子类重写该方法必须抛出哪些异常的一个子集。
