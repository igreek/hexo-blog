title:  Android之数据存储（一）
date: 2015-12-06 00:11:00
categories: android开发
tags: [android,数据存储]
toc: true
---
>应用程序都具有数据的读取和写入功能，android应用也不例外，应用程序的设置参数，程序运行状态数据等需要保存的外部存储上，以防数据丢失。若应用程序只有少量数据需要保存，则用普通文件就可以，若应用程序有大量数据需要存储，访问，则需要借助数据库。因此本篇将介绍android的普通文件存储，android内置的数据SQLite的应用。
<!--more-->
### 一、数据存储简介
>应用程序都具有数据的读取和写入功能，android应用也不例外，应用程序的设置参数，程序运行状态数据等需要保存的外部存储上，以防数据丢失。若应用程序只有少量数据需要保存，则用普通文件就可以，若应用程序有大量数据需要存储，访问，则需要借助数据库。因此本篇将介绍android的普通文件存储，android内置的数据SQLite的应用。
### 二、Android读写SharedPrefernce
对于少量的数据，如何是否打开音效，是否提供振动效果等数据都可以用Android读写SharedPrefernce进行保存。
**(1).SharedPrefernce简介**
SharedPrefernce保存数据主要是类似配置信息格式的数据，保存的数据类型是key-value数值对。SharedPrefernce接口负责读取用于程序的数据，提供了如下等方法来访问SharedPrefernce的数据。

 -  boolean contains(String key):判断SharedPrefernce是否包含该key;
 
 -   Map  getAll(): 获取SharedPrefernce中所有的数据；
 
 - boolean getXxx(String key,Type value) :获取SharedPrefernce中指定的key对应的value.

SharedPrefernce本身是一个接口，若需要创建该实例则需要通过Context提供的
 getSharedPrefernces(String name,int model )方法类获取。其中model有如下两个参数值：
Context.MODEL_WORLD_READABLE 指定该数据只能被其他应用读，不能写；
Context.MODEL_WORLD_WRITEABLE:指定该数据只能被其他应用读写。

SharedPrefernce本身没有提供写入的方法，需要调用edit()方法获取对应的Editor对象，Editor提供了如下方法向SharedPrefernce写入数据:
 1.clear():清空SharedPrefernce中所有数据
 2.PutXxx( String key ,Type value):向SharedPrefernce指定的key存入value数据。
 3.remove(String key):删除SharedPrefernce指定key的数据
 4.Commit():提交修改的内容。
 
**（2）、程序实现**
下面通过一个例子介绍如何向SharedPrefernce写入数据。

```
public class SharedPreferencesTest extends Activity {
	Button write,read;
	SharedPreferences preference;
	SharedPreferences.Editor editor;
	@SuppressWarnings("deprecation")
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		//写入和读取数据
		write=(Button) findViewById(R.id.write);
		read=(Button) findViewById(R.id.read);
		//获取
		preference=getSharedPreferences("greekw", Context.MODE_WORLD_READABLE);
		editor=preference.edit();
		
		read.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				int flag=preference.getInt("flag",0);
				String time=preference.getString("time", null);
				String result = time == null ? "您暂时还未写入数据" : "写入时间为：" + time
						+ "\n上次生成的随机数为：" + flag;
				Toast.makeText(SharedPreferencesTest.this, result,5000).show();
			}
		});
		
		write.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View v) {
				SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 "
						+ "hh:mm:ss");
				editor.putString("time",sdf.format(new Date()));
				editor.putInt("flag", 1);
				editor.commit();
			}
		});
	}
```

上述例子说明读取和写入SharedPrefernce的数据，SharedPrefernce的数据保存在data/data/packageName的目录下，以xml格式保存。
**（3）、读写其他应用的SharedPrefernce**
读写其他拥有的SharedPrefernce，则需要获取该SharedPrefernce的应用指定的相应的权限。如该应用指定为Context.MODEL_WORLD_READABLE，表明该应用可被其他重新读取。写也类似。下面是读写其他应用的SharedPrefernce的步骤：
1.创建其他应用程序对应的Context. context=createPackageContext("包结构"); anroid的包名是应用程序的唯一标识，因此可根据包名获取相应的Context.Context是Android的全局信息接口
2.调用其他应用的Context的SharedPrefernce获取相应的SharedPrefernce对象
3.调用其它应用的SharedPrefernce的editor()获取相应的Editor。