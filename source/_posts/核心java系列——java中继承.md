title:  核心java系列——java的继承
date: 2015-09-20 15:47:17
categories: 核心java
tags: [java,继承]
toc: true
---
继承是面向对象的一个重要特性，子类继承超类的方法和属性，可以使代码通用性强，封装性好。java中继承不支持多继承。下面分析继承的使用:
<!--more-->
**一、类，超类和子类**
	继承是子类继承父类的属性和方法，比如在同一公司中，经理和普通职员存在待遇存在差异，比如他们都每月领取自己的薪水，而经理又会领到奖金。
	则这可以用继承的概念去构造，子类也能去定义有差异的属性和方法。继承的格式是由关键字extends表示
	如 class Employee extends Manager{
		//属性和方法
	}
	通过extends 在已存在的类上派生出一个新类。已存在的类被称为超类，基类或父类，新类被称为派生类，子类。超类并不比子类存在更多的功能
	1.子类可扩展超类没有的方法。
	2.通用的方法常封装在父类中定义。
	3.子类可以增加域，增加方法或覆盖父类的方法，但不能删除继承父类的域和方法。
	super关键字:是指向父类的，可以拥有调用父类的属性和方法,如super(name)调用父类具有相同参数的构造器。
	下面用如下几个方面继承中的几个概念：
	**1.继承层次**	
	继承不只局限于一个层次，也可以多层次继承，有一个公共类派生出所有类的集合被称为继承的层次。
	java不支持多继承。
	**2.多态**
	多态是指一个类可以有多种对象的引用，子类的每个对象也可是超类的对象，比如
	Employee e=new Employee();
	Employee e=new Manager(); 说明e即可父类Employee的引用，也是子类Manager的引用。
	**3.动态绑定**
	动态绑定是一种很重要的特性，可以在无须修改现存代码的基础上对程序进行扩展，比如生成一个新类B,变量e持有类B的引用，则对类中的非final,private,static修饰的方法，变量e都可调用。
	**4.阻止继承**
	 父类中若有些方法不想被子类去继承，则可用final修饰，被称为不可扩展类。定义形式如下:
	 final class Manager extends Employee{
	 }
	 类中的方法也可被修饰成final类.且此方法无法被继承。如
	 
```
class Employee{
	 public final String getName(){
		return name;
	  }
	 
```
**5.抽象类**
java中抽象是一个很重要的概念，抽象类和抽象方法的具体实现在其子类中，扩展抽象的方法有如下方式
1.在子类中定义部分抽象方法或不定义抽象方法，则子类必须定义为抽象类。
2.在子类定义全部的抽象方法，则子类不需要定义为抽象类。
```
public abstract class Person {
	private String name;
	public String getName() {
		return name;
	}
	public Person(String name){
		this.name=name;
	}
	public abstract String getDescription();
}

```
```
public class Employee extends Person {

	public Employee(String name,double s,int year,int month,int day) {
		super(name);
		salary=s;
		GregorianCalendar calendar=new GregorianCalendar(year, month-1, day);
		hday=calendar.getTime();
	}
	@Override
	public String getDescription() {
		return String.format(" an employee salry is $%.2f", salary);
	}
	private  double salary;
	private Date hday;
	public double getSalary() {
		return salary;
	}
	public Date getHday() {
		return hday;
	}
	
	
}
```

**6.受保护访问**
在类中常将属性修饰为private，方法修饰为public 可让子类不能访问超类中的私有属性。然而有时候超类的中的某个属性能被子类访问。
则可将此属性修饰为protected.如Employee的name属性被protected修饰，子类可访问该属性，而不能访问其他属性。
java中常用于控制可见性的4个修饰符:
对本类可见-private
对所有类可见-public
对本包和所有子类可见-protected
对本包可见-默认修饰符
	
**二、Object-所有类的超类**
Object类所有java类的祖先，java中每个类都是由它扩展而来。如除基本类型不是对象，字符，布尔类型等都不是对象，所有数组类型，包括基本类型与对象数组都扩展于Object.
Object中几个比较重要的方法，如equals,HashCode()和toString()等方法
1.equals()是检测一个对象是否与另一个对象相同，在Object中，用于判断两个对象的引用是否相同，若引用相同，则认为两个对象是相等的。
2.hashCode()方法是基于散列码，散列码是对象导出的一个整型值，若x,y是两个不同的对象，则x.hashCode()与y.hashCode()基本不会相同。
  Equals()与hashCode()的定义必须一致，如x.equals(y)返回true,则x.hashCode()与y.hashCode()就必须有相同的值。

3.toString()返回对象值的字符串
如 public string toString(){
	return "Employee[name="+name+",salary="+salary+"]"
	}
	或
public string toString(){
	return getClass().getName()+"[name="+name+",salary="+salary+"]"
	}