title:  核心java系列——线程（一）
date: 2015-10-20 15:47:17
categories: 核心java
tags: [java,多线程]
toc: true
---
多线程是程序中有多个线程流在同时调度资源。线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),一个线程可以创建和撤销另一个线程。
<!--more-->
### 一、简介

> 多线程是程序中有多个线程流在同时调度资源。线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),一个线程可以创建和撤销另一个线程。

### 二、线程和进程
1.进程是程序运行的实例，每一个进程都有自己的内存空间，包含内容和数据，不同进程间有相互独立的地址空间
2.线程是CPU调度的基本单位，每一个线程都有顺序执行的，线程有共享资源和锁机制。
3.两者区别和联系 
(1) 划分尺度:线程更小，所以多线程程序并发性更高;
(2) 资源分配：进程是资源分配的基本单位，同一进程内多个线程共享其资源;
(3) 地址空间：进程拥有独立的地址空间，同一进程内多个线程共享其资源;
(4) 处理器调度：线程是处理器调度的基本单位;  
(5) 执行：每个线程都有一个程序运行的入口，顺序执行序列和程序的出口，但线程不能单独执行，必须组成进程，
	一个进程至少有一个主线程。简而言之,一个程序至少有一个进程,一个进程至少有一个线程
#### 三、线程的创建和启动
线程创建有2中方式，一是实现Runnable接口，实现run()方法，然后创建一个Thread对象，将
而是继承Thread类，实现run方法。两者都需要调用start()方法来启动线程。下面是两者方法的程序实现：
1.继承Thread方式
	
```
public class Demo1 extends Thread {
	@Override
	public void run() {
		for(int i=0;i<10;i++){
			System.out.println(Thread.currentThread().getName()+i);
		}
	}
	}
```
2.实现Runnable接口
	
```
public class Demo2 implements Runnable {
	@Override
	public void run() {
		for(int i=0;i<10;i++){
			System.out.println(Thread.currentThread().getName()+i);
		}
	}
}
```
	
### 四、线程的状态
   线程运行时也有它的生命周期，线程会要经历开始（等待），运行，挂起（阻塞）和终止四种不同的状态，且四种状态可以由Thread来自由控制，下面给出Thread类控制各个状态的方法

> 1.线程开始 start()/run();
> 2.线程挂起和唤醒 resume()/suspend()-已过时 sleep()；
> 3.线程终止 stop()不建议使用 interupt()；
> 4.其他与线程状态相关的方法 
> isAlive():判断线程的状态是否还活着
>join():调用某线程的该方法，将当前线程与该线程“合并”，即等待该线程结束，再恢复当前线程的运行；
> yield():线程的让步，即让出当前线程的资源给其他线程使用。

状态图如下:
![这里写图片描述](http://img.blog.csdn.net/20151121203705684)

从图中可看出线程在建立后并不马上执行run方法中的代码，而是处于等待状态。线程处于等待状态时，可以通过Thread类的方法来设置线程不各种属性，如线程的优先级（setPriority）、线程名(setName)和线程的类型（setDaemon）等。

 - 当调用start方法后，线程开始执行run方法中的代码。线程进入运行状态。可以通过Thread类的isAlive方法来判断线程是否处于运行状态。
 - 当线程处于运行状态时，isAlive返回true，
 - 当isAlive返回false时，可能线程处于等待状态，也可能处于停止状态。

***注意:***一但线程开始执行run方法，就会一直到这个run方法执行完成这个线程才退出。但在线程执行的过程中，可以通过两个方法使线程暂时停止执行。这两个方法是suspend和sleep。
在使用suspend挂起线程后，可以通过resume方法唤醒线程。而使用sleep使线程休眠后，只能在设定的时间后使线程处于就绪状态（在线程休眠结束后，线程不一定会马上执行，只是进入了就绪状态，等待着系统进行调度）。
**在使用sleep方法时有两点需要注意：**

> 1. sleep方法有两个重载形式，其中一个重载形式不仅可以设毫秒，而且还可以设纳秒(1,000,000纳秒等于1毫秒)。但大多数操作系统平台上的Java虚拟机都无法精确到纳秒，因此，如果对sleep设置了纳秒，Java虚拟机将取最接近这个值的毫秒。
> 2. 在使用sleep方法时必须使用throws或try{…}catch{…}。因为run方法无法使用throws，所以只能使用try{…}catch{…}。当在线程休眠的过程中，使用interrupt方法中断线程时sleep会抛出一个InterruptedException异常。sleep方法的定义如下：
> publicstaticvoid sleep(long millis) throws InterruptedException
> publicstaticvoid sleep(long millis, int nanos) throws
> InterruptedException

下面的举例为线程的合并的实现：
#### A线程：
```
public class DemoA extends Thread {
	public DemoA(String name){
		super(name);
	}
	@Override
	public void run() {
		for(int i=0;i<10;i++){
			System.out.println(Thread.currentThread().getName()+"-"+i);
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}

}
```

#### B线程:

```
public class DemoB extends Thread {
	private Thread a;
	public DemoB(String name,Thread a){
		super(name);
		this.a=a;
	}
	@Override
	public void run() {
		for(int i=0;i<10;i++){
			System.out.println(Thread.currentThread().getName()+"-"+i);
			try {
				Thread.sleep(1000);
				if(i==4){
					a.join();
				}
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```

### 五、线程的属性
线程的属性包括线程的优先级，守护线程等，明白线程的属性的作用，可以更灵活的设置线程的执行状态。
#### 1.线程的优先级
每一个线程都会对应一个优先级，默认情况下，新创建的线程会继承他父类的优先级。可以利用setPriority方法来修改线程的优先级的高低。修改的范围可以使MIN_PRIORITY和MAX_PRIORITY之间的任意级别
下面为线程优先级设置的实例:
***定义的线程类:***
```
public class Demo3 implements Runnable {
@Override
public void run() {
	for(int i=0;i<20;i++){
		System.out.println(Thread.currentThread().getName()+"-"+i);
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
}
```

***线程优先级的设置:***	

```
//最小级别
	Thread t1=new Thread(new Demo3());
	t1.setPriority(Thread.MIN_PRIORITY);
	t1.start();
	//正常
	Thread t2 =new Thread(new Demo3());
	t2.setPriority(Thread.NORM_PRIORITY);
	t2.start();
	//最大级别
	Thread t3 =new Thread(new Demo3());
	t3.setPriority(Thread.NORM_PRIORITY);
	t3.start();
}
```

注意:在使用线程优先级时，应避免常犯的一个错误，假如高优先级的线程处于非活跃状态，低优先级的线程也不可能会执行，而资源调度线程时会在高优先级的线程中选择，这会是低优先级的线程饿死。
##### 2.守护线程
一般线程要转换为守护线程，可通过setDaemon(true);来设置，守护线程不会去访问固有资源，如文件，数据库等。其作用是为其他线程提供服务，如计时器的例子，守护线程可定时发送
信号给实现计时的线程。
#### 3.未捕获异常的处理器
线程实现run方法时不会抛出可被检测的异常，而抛出的异常不能被检测到会导致线程终止，从而是程序死亡。
java提供了一个未捕获异常的处理器，该处理器为Thread.UncaughTExceptionHandler接口的类。从JSE5.0后，提供了setUncaughTExceptionHandler方法为线程安装处理器.