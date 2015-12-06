title:  Android之事件处理
date: 2015-11-09 23:49:00
categories: android开发
tags: [android,事件处理]
toc: true
---
>事件处理是一种在应用程序上为用户动作提供响应的机制，android提供了强大的事件处理机制，包括两套事件处理机制。
<!--more-->
**一、android事件处理概述**
	  事件处理是一种在应用程序上为用户动作提供响应的机制，android提供了强大的事件处理机制，包括两套事件处理机制:
	A.基于监听的事件处理:主要实现为Android的界面组件提供特定的事件监听器
	B.基于回调的事件处理:主要实现为重写Android组件绑定的回调方法或重写Activity的回调方法
	一般而言，基于回调的事件处理会常用于处理一些通用性的事件
**二、android的事件处理机制**
	**1.基于监听事件的处理**
	1.1监听的处理模型
		在事件监听的处理模型中，主要有如下三类对象:
		事件源(EventSource):通常是各个组件，例如按钮，菜单等。
		事件(Event):封装在界面上操作的的特定动作。
		事件监听器(EventListener):赋值监听事件所发生的事情，并对各种事件作出响应。
    Anroid的事件处理机制是一种委派式事件处理方式:普通组件将整个事件处理委托给事件监听器，当事件源发生指定的事件时，就通知所委托的事件监听器，并由它处理此事件。
	事件监听器与事件是一对多的关系，一个监听器可以监听一个或多个事件源，委派式的处理方式可以把事件源上所有可能发生的事件分别授权给不同的事件监听器来处理，同时也可以让同一类事件都使用一个相同的事件监听器。
事件处理的示意图如下:
	
**1.1.1程序实例**
	

```
public class EventQs extends Activity
{
	@Override
	public void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		// 获取应用程序中的bn按钮
		Button bn = (Button) findViewById(R.id.bn);
		// 为按钮绑定事件监听器。
		bn.setOnClickListener(new MyClickListener()); // ①
	}

	// 定义一个单击事件的监听器
	class MyClickListener implements View.OnClickListener
	{
		// 实现监听器类必须实现的方法，该方法将会作为事件处理器
		@Override
		public void onClick(View v)
		{
			EditText txt = (EditText) findViewById(R.id.txt);
			txt.setText("bn按钮被单击了！");
		}
	}
}
```
从上面的程序中可看出，基于监听的事件处理模型编程步骤有:
A.获取普通界面组件(事件源)
B.实现事件监听器类，需要实现XxxListener接口
C.调用事件源的setXxxListener方法将事件监听器注册给事件源

**1.2 事件和事件监听器**
	事件监听的处理模型涉及到三个元素:事件，事件监听器，事件源等，其中事件源是最容易创建，任何界面组件都可以作为事件源；事件的产生也是由系统自动产生，无须程序员关系.
	所以事件监听器是整个事件处理的核心。
	在基于事件监听的处理模型中，事件监听器必须实现事件监听器接口，Android为不同的界面组件提供了不同的事件监听器接口，这些接口通常以内部类的方式存在。
	以View来说明,包含如下几个内部接口
	view.onClickListener():单击的事件需要实现的接口。
	view.onCreateContextMenuListener():创建上下文菜单的事件需要实现的接口。
	view.onFousChangeListener()焦点改变的事件监听器需要实现的接口。
	view.onKeyListener():按键事件的事件监听器需要实现的接口。
	view.onLongClickListener():长按单击的事件监听器需要实现的接口。
	view.onTouchListener():触摸屏事件监听器需要实现的接口。
	
总之事件监听器，其实就是实现了特定接口的java类，在程序中通常有以下几种实现方式:
	1.内部类形式:将事件监听器类定义为当前内部类。
	2.外部类形式:将事件监听器类定义为一个外部类。
	3.Activity本身作为事件监听器类:Activity实现监听器接口，并实现事件处理方法。
	4.匿名内部类形式。
	以下为各种形式的代码实现:
	A.外部类的实现
	
```
public class SendSmsListener implements OnLongClickListener
{
	private Activity act;
	private EditText address;
	private EditText content;

	public SendSmsListener(Activity act, EditText address
		, EditText content)
	{
		this.act = act;
		this.address = address;
		this.content = content;
	}

	@Override
	public boolean onLongClick(View source)
	{
		String addressStr = address.getText().toString();
		String contentStr = content.getText().toString();
		// 获取短信管理器
		SmsManager smsManager = SmsManager.getDefault();
		// 创建发送短信的PendingIntent
		PendingIntent sentIntent = PendingIntent.getBroadcast(act
			, 0, new Intent(), 0);
		// 发送文本短信
		smsManager.sendTextMessage(addressStr, null, contentStr
			, sentIntent, null);
		Toast.makeText(act, "短信发送完成", Toast.LENGTH_LONG).show();
		return false;
	}
}
```
创建此监听器需要传入Activity 和EditText两个参数
B.Activity本身作为事件监听器类

```
// 实现事件监听器接口
public class ActivityListener extends Activity
	implements OnClickListener
{
	EditText show;
	Button bn;

	@Override
	public void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		show = (EditText) findViewById(R.id.show);
		bn = (Button) findViewById(R.id.bn);
		// 直接使用Activity作为事件监听器
		bn.setOnClickListener(this);
	}

	// 实现事件处理方法
	@Override
	public void onClick(View v)
	{
		show.setText("bn按钮被单击了！");
	}
}
```
此形式十分简洁，直接实现OnClickListener监听器接口，但有如下缺点:
程序结构混乱，Activity主要职责是完成界面的初始化工作，若包含处理监听器的实现，则会产生混乱
C.匿名内部类形式

```
public class AnonymousListener extends Activity
{
	EditText show;
	Button bn;

	@Override
	public void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		show = (EditText) findViewById(R.id.show);
		bn = (Button) findViewById(R.id.bn);
		// 直接使用Activity作为事件监听器
		bn.setOnClickListener(new OnClickListener()
		{
			// 实现事件处理方法
			@Override
			public void onClick(View v)
			{
				show.setText("bn按钮被单击了！");
			}
		});
	}
}
```
此形式是使用比较广泛的方法，只需要new 监听器或new 适配器 方式就可以实现。
D.直接绑定到元素的标签

**2.基于回调的事件处理**
	Android除了基于事件监听处理模型外，还有基于回调的事件处理模型 (详情请期待更新)。