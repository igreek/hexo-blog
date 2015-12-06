title:  核心java系列——线程（二）
date: 2015-10-21 15:47:17
categories: 核心java
tags: [java,多线程]
toc: true
---
在大多数实际应用中，常常存在两个或两个以上的线程共享对同一数据的存储。如果多个线程去对同一对象的数据进行修改，则会引起线程竞争资源，导致数据被修改错误的问题，
比如在ATM机上取款，多个用户对同一账户进行操作就会很容易发生。所以下面分析如何解决多线程共享资源引起竞争导致数据破坏问题。
<!--more-->
### **一、锁的原理**
java中为每一个对象都提供了锁机制，用synchronized关键字修饰，它可以修饰方法和代码块。当一个对象获得该锁时，只能充许一个线程对该对象进行操作，其他线程处于等待状态，直到该线程释放锁。
锁时可以重复利用的，锁有一个持有计数器来记录被利用的情况。线程可以根据计数器去加锁和释放锁。
条件对象可以理解为临界区，线程只有在满足某一条件后才能使用它，一个锁对象可以有一个或多个相关的条件对象。以下为同步实例的实现:

```
public class FooThread extends Thread {
	private int x=100;
	
	public FooThread(String name) {
		super(name);
	}
	@Override
	public void run() {
		for(int i=0;i<4;i++){
			x=this.sub(x, 30);
			try {
				Thread.sleep(1);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			System.out.println(Thread.currentThread().getName() + " : 当前的x值= " +x); 
		}
	}
	
	public synchronized int sub(int x,int y){
		return x=x-y;
	}
}

```


### **二、内部锁**
内部锁将类的相关静态方法加上synchronized关键字,比如Bank类有一个静态同步的方法，那么当该方法被调用时，Bank.class将会被调用对象锁住，则其他线程无法再调用该类中的对象和其他同步的静态方法。
内部锁也存在以下局限性：
1.无法中断一个在视图获得锁的线程
2.视图锁得锁事不能设定超时
3.每一个锁仅有一个单一条件

### **三、同步阻塞**
如果线程试图进入同步方法，而其锁已经被占用，则线程在该对象上被阻塞。实质上，线程进入该对象的的一种池中，必须在哪里等待，直到其锁被释放，该线程再次变为可运行或运行为止。
当考虑阻塞时，一定要注意哪个对象正被用于锁定：
1、调用同一个对象中非静态同步方法的线程将彼此阻塞。如果是不同对象，则每个线程有自己的对象的锁，线程间彼此互不干预。 
2、调用同一个类中的静态同步方法的线程将彼此阻塞，它们都是锁定在相同的Class对象上。 
3、静态同步方法和非静态同步方法将永远不会彼此阻塞，因为静态方法锁定在Class对象上，非静态方法锁定在该类的对象上。 
4、对于同步代码块，要看清楚什么对象已经用于锁定（synchronized后面括号的内容）。在同一个对象上进行同步的线程将彼此阻塞，在不同对象上锁定的线程将永远不会彼此阻塞。
**volatile域**
若仅仅为了读写一个或两个实例域就使用同步，这对资源的开销过大。volatile是为实例域的访问提供了一种免锁机制，若声明一个实例域为volatile，则虚拟机就知道该域可能被其他线程并发访问。声明方式：
private volatile boolean flag;
还有一种用于原子整数，浮点数等的包装器类 Atomic可以应用于程序的并发访问，保证域的安全。
总之，在以下三个条件下，域的并发访问是安全的:
1.域是final 并且在在构造器调用完成后访问
2.对域的访问由公有的锁进行保护
3.域是volatile的

### **四、线程安全**
当一个类已经很好的同步以保护它的数据时，这个类就称为“线程安全的”。
即使是线程安全类，也应该特别小心，因为操作的线程是间仍然不一定安全。
举个形象的例子，比如一个集合是线程安全的，有两个线程在操作同一个集合对象，当第一个线程查询集合非空后，删除集合中所有元素的时候。第二个线程也来执行与第一个线程相同的操作，也许在第一个线程查询后，第二个线程也查询出集合非空，但是当第一个执行清除后，第二个再执行删除显然是不对的，因为此时集合已经为空了。程序说明如下:

```
public class Demo4 {
	@SuppressWarnings("unchecked")
	private List resultList=Collections.synchronizedList(new LinkedList());
	
	public synchronized void add(String param){
		resultList.add(param);
	}
	public synchronized String remove(){
		if(resultList.size()>0){
			return (String) resultList.remove(0);
		}else{
			return null;
		}
	}
}
```
**测试调用**

```
public static void main(String[] args) {
	final Demo4 d=new Demo4();
	d.add("test");
	class Test extends Thread{
		@Override
		public void run() {
			String param=d.remove();
			try {
				Thread.sleep(1);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			System.out.println(param);
		}
	}
	Thread t1=new Test();
	Thread t2=new Test();
	t1.start();
	t2.start();
		
	}
```
虽然集合对象  private List nameList = Collections.synchronizedList(new LinkedList());
是同步的，但若remove()方法的synchronized去掉后，会引起线程不安全问题。
### **五、死锁**
锁和条件不能解决多线程中所有问题，当多个线程发生阻塞时，每个线程在等待另一线程释放资源会发生死锁，比如 账户A：200元，账户B:300元，A线程从账户A转移300到账户B,线程B从账户B转移400到账户A，因为A和B账户的余额都不足，无法进行转换，两个线程无法继续执行，而引发死锁状态。下面看一个发生死锁的实例:

```
public class DeadLock implements Runnable {
	int flag=1;
	final Object o1=new Object();
	final Object o2=new Object();
	@Override
	public void run() {
		System.out.println("flag="+flag);
		if(flag==1){
			synchronized (o1) {
				try {
					Thread.sleep(300);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			synchronized (o2) {
				System.out.println("1");
			}
		}else if(flag==0){
			synchronized (o2) {
				try {
					Thread.sleep(300);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			synchronized (o1) {
				System.out.println("2");
			}
		}
	}

}
```
**程序说明:**

 ◆ 一个线程（ThreadA）调用run()。 

  ◆ ThreadA在o1上同步，但允许被抢先执行。 

  ◆ 另一个线程（ThreadB）开始执行。 

  ◆ ThreadB调用run()。 

  ◆ ThreadB获得o2，继续执行，企图获得o1。但ThreadB不能获得o1，因为ThreadA占有o1。 

  ◆ 现在，ThreadB阻塞，因为它在等待ThreadA释放o1。 

  ◆ 现在轮到ThreadA继续执行。ThreadA试图获得o2，但不能成功，因为o2已经被ThreadB占有了。 

  ◆ ThreadA和ThreadB都被阻塞，程序死锁。 