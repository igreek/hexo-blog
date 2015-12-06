title:  Android之Intent分析
date: 2015-10-25 23:54
categories: android开发
tags: [android,Intent]
toc: true
---
>android的Intent是目的，意图之意，是android提供的一种协助应用程序间交互和通讯的机制。intent不仅可以用于应用程序间，也可以应用于应用程序内部间的Activity/service的交互，在Intent的使用中不会表现出函数的调用。相对于函数的调用而言，Intent更为抽象，调用Intent的基本元素是Activity/Service。
<!--more-->
**一、Intent的简介**

android的Intent是目的，意图之意，是android提供的一种协助应用程序间交互和通讯的机制。intent不仅可以用于应用程序间，也可以应用于应用程序内部间的Activity/service的交互，在Intent的使用中不会表现出函数的调用。相对于函数的调用而言，Intent更为抽象，调用Intent的基本元素是Activity/Service。

**二、Intent的基本用法**
	Android中与Intent相关的还有Action/Category及IntentFilter等，以及用于广播的Intent，那下面来说明Intent的一些用法:
	Intent的2种基本用法为1.显示的Intent,即在构造Intent对象就指定接受者；2.隐士的Intent，即Intent的发送者在构造Intent对象时，并不知道也不关心接受者是谁。
**(1)显示的Intent**
1.同一个应用程序间Activity的切换
通常一个应用程序中需要多个UI 屏幕，也就需要多个Activity 类，并且在这些 Activity 之间进行切换，这种切换就是通过 Intent 机制来实现的
在同一个应用程序中切换 Activity时，我们通常都知道要启动的 Activity 具体是哪一个，因此常用显式的 Intent 来实现。下面的例子用来实现一个非常简单的应用程序 SimpleIntentTest ，它包括两个UI 屏幕也就是两个 Activity——SimpleIntentTest类和 TestActivity 类， SimpleIntentTest类有一个按钮用来启动 TestActivity。
程序的代码非常简单， SimpleIntentTest类的源代码如下：

```
public   class  SimpleIntentTest  extends  Activity  implements  View.OnClickListener{

     @Override

     public   void  onCreate(Bundle savedInstanceState) {

         super .onCreate(savedInstanceState);

        setContentView(R.layout. main );

        Button startBtn = (Button)findViewById(R.id. start_activity );

        startBtn.setOnClickListener( this );

    }
     public   void  onClick(View v) {

         switch  (v.getId()) {

         case  R.id. start_activity :

            Intent intent =  new  Intent( this , TestActivity. class );

            startActivity(intent);

             break ;

         default :

             break ;

         }

    }   

}
```

Intent intent =  new  Intent( this , TestActivity. class );
startActivity(intent);
这里定义 Intent 对象时所用到的是 Intent 的构造函数之一：
Intent ( Context  packageContext,  Class <?> cls)
两个参数分别指定 Context 和 Class ，由于将Class 设置为 TestActivity.class，这样便显式的指定了TestActivity 类作为 该Intent 的 接收者，
通过后面的startActivity() 方法便可启动 TestActivity 。
定义好TestActivity后，则需要 在AndroidManifest.xml 中增加TestActivity 的声明:

```
< activity   android:name = ".TestActivity" />
```

2.不同应用程序间Actvity的切换
```
Intent intent =  new  Intent();
intent.setClassName( "com.tope.samples.intent.simple" , 
		 "com.tope.samples.intent.simple.TestActivity" );
startActivity(intent);
```

**(2).隐式 Intent(Implicit Intent)**
		如果 Intent 机制仅仅提供上面的显式 Intent 用法的话，这种相对复杂的机制似乎意义并不是很大。确 实，Intent 机制更重要的作用在于下面这种隐式的 Intent ，即 Intent 的发送者不指定接收者，很可能不知道也不关心接收者是谁，而由 Android 框架去寻找最匹配的接收者
1.最简单的隐式Intent
下面定义一个用来启动 Android 自带的打电话功能的 Dialer 程序。

```
public   class  ImplicitIntentTest  extends  Activity     

     implements  View.OnClickListener{

     /**   Called   when   the   activity   is   first   created.   */

     @Override

     public   void  onCreate(Bundle savedInstanceState) {

         super .onCreate(savedInstanceState);

        setContentView(R.layout. main );

        Button startBtn = (Button)findViewById(R.id. dial );

        startBtn.setOnClickListener( this );

    }
     public   void  onClick(View v) {

         switch  (v.getId()) {

         case  R.id. dial :

            Intent intent =  new  Intent(Intent. ACTION_DIAL );

            startActivity(intent);

             break ;

         default :

             break ;

        }

    }   

}
```

与显示Intent的方式不同，此方式指定接受者，初始化Intent时，只是传入Intent. ACTION_DIAL参数，没有显示的支持哪个接收者。
2.增加一个接收者
若接收者希望能接收某些Intent，则需要在AndroidMainfest.xml中增加Activity的声明，并设置对应的IntentFilter,Action，如:

```
< activity   android:name = ".TestActivity" >

         < intent-filter >

             < action   android:name = "android.intent.action.DEFAULT"   />

             < action   android:name = "android.intent.action.DIAL"   />

             < category   android:name = "android.intent.category.DEFAULT"  />

         </ intent-filter >

     </ activity >
```

Intent Filter 及 Action, Category 等概念—— Intent 发送者设定 Action 来说明将要进行的动作，而 Intent 的接收者在 AndroidManifest.xml 文件中通过设定 Intent Filter来声明自己能接收哪些Intent