title:  Android之Activity分析
date: 2015-10-12 16:24
categories: android开发
tags: [android,Activity]
toc: true
---
>activity是手机应用程序窗口的交互界面，一个应用程序通常会包含多个activity,都会在Mainfest.xml中指定一个入口(主的acitivity)
<!--more-->
**一、什么是Activity**
activity是手机应用程序窗口的交互界面，一个应用程序通常会包含多个activity,都会在Mainfest.xml中指定一个入口(主的acitivity).配置如下
```
<activity
   android:label="@string/app_name"
   android:name=".MainActivity" >
	  <intent-filter >
		  <action android:name="android.intent.action.MAIN" />
		  <category android:name="android.intent.category.LAUNCHER" />
	  </intent-filter>
 </activity>
```

应用第一次运行时就会看到此Activity，并可以通过此Activity去启动其他的Activity.每当新的Activity启动时，当前的Activity就会停止，新的Activity就会压入到
stack(先进后出)中，并获得用户焦点，当回退时，当前的Activity就回销毁，上一个Activity就会恢复


**二、activivty的什么周期**
流程图如下:
	![这里写图片描述](http://img.blog.csdn.net/20151012162332940)
涉及都的方法有onCreate()  onStart() onResume() onPause() onStop() onRestart() onDestory 
Activity启动过程 onCreate()——>onStart()——>onResume()  退出Activity过程 onPause()——>onStop()——>onDestory()
按下home键回到主界面时执行过程:onPause()——>onStop(),再次打开运行过程:onRestart()——>onStart()——>onResume()
以下是各个方法的说明:
onCreate()：当 activity 第一次创建时会被调用。在这个方法中你需要完成所有的正常静态设置 ，比如创建一个视图（ view ）、绑定列表的数据等等。
如果能捕获到 activity 状态的话，这个方法传递进来的 Bundle 对象将存放了 activity 当前的状态。调用该方法后一般会调用 onStart() 方法。

onRestart()：在 activity 被停止后重新启动时会调用该方法。其后续会调用 onStart 方法。

onStart()当 activity 对于用户可见前即调用这个方法。如果 activity回到前台则接着调用 onResume() ，如果 activity 隐藏则调用onStop()

onResume()：在 activity 开始与用户交互前调用该方法。在这时该activity 处于 activity 栈的顶部，并且接受用户的输入。其后续会调用 onPause() 方法。

onPause()：在系统准备开始恢复其它 activity 时会调用该方法。这个方法中通常用来提交一些还没保存的更改到持久数据 中，停止一些动画或其它一些耗 CPU 的操作等等。
无论在该方法里面进行任何操作，都需要较快速完成，因为如果它不返回的话，下一个 activity 将无法恢复出来。如果 activity 返回到前台将会调用 onResume() ，如果 activity 变得对用户不可见了将会调用onStop() 。

onStop()：在 activity 对用户不可见时将调用该方法。可能会因为当前 activity 正在被销毁，或另一个 activity （已经存在的activity 或新的 activity ）已经恢复了正准备覆盖它，而调用该方法。
如果 activity 正准备返回与用户交互时后续会调用onRestart ，如果 activity 正在被释放则会调用 onDestroy 。

onDestroy()：在 activity 被销毁前会调用该方法。这是 activity 能接收到的最后一个调用。可能会因为有人调用了 finish 方法使得当前activity 正在关闭，
系统为了保护内存临时释放这个 activity的实例，而调用该方法。你可以用 isFinishing 方法来区分这两种不同的情况。

**三、如何启动一个新的Activity**
1.启动一个新的Activity，可以通过Context的startActivity()方法
 如Intent intent=new Intent(this,TestActivity.class)(其中TestActivity是需要被启动的Activity)
   startActivity(intent)
2.若启动一个新的Activity，并需要传递数据到此Activity
	Intent intent=new Intent(this,TestActivity.class)(其中TestActivity是需要被启动的Activity)
	Bundle bundle=new Bundle();
	bundle.putString("name","张三")
	intent.putExtra(bundle)
	startActivity(intent)
3.若启动新的带返回值的Activity,并将返回值传给启动它的Activity
实现如下
Intent intent=new Intent(ActvityDemo.this,TestActivity.class)
startActivityForResults(intent,0x1000)
在ActvityDemo中需要获取到TestActivity的返回值，则TestActivity需要实现如下
Intent intent =new Intent();
intent.putExtra("backkey","test");
setResult(0x1001, intent);
那么backkey值在哪里获取呢？必须重写onActivityResult方法，通过判断requestCode
if(backCode==0x1001){
String str = data.getStringExtra("backkey");
	Log.i(TAG, "返回的值为："+str);
	}
	
**四、保持Activity的运行状态**

**五、完成退出Activity**
当进行back时，Activity回调用onDestory()方法，但进程没有被杀死，甚至调用finish()方法，进程还在。则需要完成退出，实现如下:
Intent intent=new Intent();
intent.setClass(context,MainActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_UP);
intent.putExtra("flag",EXIT_APPLICATION);
context.startActivity(intent);