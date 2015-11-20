title: 核心java系列-抽象类和接口
date: 2015-11-20 19:17:27
categories: 核心java
tags: [java]
toc: true
---
##简介
>java中，抽象类和接口是支持抽象概念的两种机制，因为它们的存在，赋予了java强大的面向对象编程能 力。接口抽象类在抽象定义上有很多相似地方，甚至可以相互替换。但它们还是有区别的，下面从接口，抽象类以及两者的区别等方面来分析。
<!--more-->
##一、抽象类
**在面向对象领域，一切都可以理解为对象，对象可以用类来描述。但不是所有的类都是来描述对象的。这些类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。**
抽象类往往用来表征我们在对问题领域进行分析、 设计中得出的抽象概念，是对一些类的本质的抽象。比如在车子的类别中，有自行车，桥车，它们的本质属性都可以归类为车，但这个车不能描述出自行车和桥车的区别，需要通过自行车的定义来具体描述车的性质，所以车可以理解为一个抽象类的概念。
(1).在面向对象中，抽象类是不能是实例化的，在抽象类中可以定义一组抽象方法，即没有具体实现的方法，在方法前加上abstract关键字来修饰。这些抽象方法的实现需要具体的派生类去实现。
(2)抽象类也可以继承，其目的也是为了体现继承，抽取出类的相同性质，体现OOP原则。所以在使用抽象类时需要注意以下几点:
 1. 抽象类不能被实例化，若一个类包含了一个以上的抽象方法，那么这个类也必须定义为抽象类（即必须也用abstract来修饰这个类）。
 2. 抽象类的抽象方法必须由其其子类实现。
 3. 抽象类中可以保护具体的实现方法，也可以不包含抽象方法
 4. abstract不能与final并列修饰同一个类。
 5. abstract 不能与private、static、final或native并列修饰同一个方法。

下面通过一个实例来说明

```
public abstract class Bird {
	public abstract void fly();
	public void eat(){
		System.out.println("鸟会吃虫子...");
	}
}

public class Eagle extends Bird {
	@Override
	public void fly() {
	}
	public void crash(){
		System.out.println("Eagle...会抓老鼠");
	}
}
public class Parrot extends Bird {
	@Override
	public void fly() {
		
	}
	public void sign(){
		System.out.println("Parrot ");
	}
}
public class AbtractTest {
	public static void main(String[] args) {
		Bird b1=new Parrot();
		Bird b2=new Eagle();
		b1.fly();
		b2.fly();
	}
}
```

##二、接口
接口不是类，是类的功能的描述，不能实例化一个接口，只能new接口的实现类。它比抽象类更加抽象。接口是类所依赖的一种规范化形式，没有具体的实现方法，若实现该接口类，则必须实现该接口的所有方法，用implements是实现，接口的定义由interface声明。
接口是对抽象类的一种可扩展，因为抽象类是不能实现多继承的，但接口可以实现多继承，各被继承的接口由逗号分隔，当然接口和抽象类可以联合使用，如 A extends B implements C,D{};
在接口使用中需要注意以下几点：
1.接口中的所有方法必须要被实现。
2.接口只可以定义不可变的成员变量。如 static final String  A_PARAM="";
3.接口不能用new去实例化,但可以声明一个接口变量，该变量必须引用一个实现该接口的类的对象，可以使用 instanceof 检查一个对象是否实现了某个特定的接口。例如：if(anObject instanceof Comparable){}。
实例介绍如下:

```
	#接口
	public interface Common {
		public void run() throws Exception;
	}
	#实现类
	public class CommonImpl implements Common {
		@Override
		public void run() throws Exception {
		}
	}
	
	public class InterfaceTest {
	public static void main(String[] args) throws Exception {
		CommonImpl cl=new CommonImpl();
		cl.run();
	}
}
```

##三、接口和抽象类的区别
尽管接口和和抽象类有很多相似处，都是用来被继承或被实现，但也有区别，下面从方式的实现，构造器，访问修饰符，多继承，效率等方面进行比较:
| 参数     |    抽象类 | 接口  |
| :-------- | --------:| :--: |
| 默认的方法实现  | 它可以有默认的方法实现 | 接口完全是抽象的,它根本不存在方法的实现 |
| 实现    |   子类使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。|子类使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现 |
|构造器      |   抽象类可以有构造器| 接口不能有构造器  |
|修饰符     |    抽象方法可以有public、protected和default这些修饰符 | 接口方法默认修饰符是public。你不可以使用其它修饰符。  |
|main方法      |   抽象方法可以有main方法并且我们可以运行它| 接口没有main方法，因此我们不能运行它。  |
|多继承      |    抽象方法可以继承一个类和实现多个接口 | 接口只可以继承一个或多个其它接口|
|效率      |    它比接口速度要快 | 接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。 |
|添加新方法    |    如果你往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。 | 如果你往接口中添加方法，那么你必须改变实现该接口的类。  |

##四、总结
1.如果你拥有一些方法并且想让它们中的一些有默认实现，那么使用抽象类吧。
2.如果你想实现多重继承，那么你必须使用接口。由于Java不支持多继承，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。
3.如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就需要改变所有实现了该接口的类。