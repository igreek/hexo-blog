title:  Android之Fragment分析
date: 2015-11-11 23:53:00
categories: android开发
tags: [android,Fragment]
toc: true
---
>Fragment是非常实用的组件，是Android3.0引入的新API,Fragment代表了Activity的子模块，可以将其理解为Activity的片段。
Fragment也拥有自己的生命周期，但它的生命周期会受到它所在Activity的生命周期的控制。
<!--more-->
一、Fragment概述
------------

Fragment是非常实用的组件，是Android3.0引入的新API,Fragment代表了Activity的子模块，可以将其理解为Activity的片段。
Fragment也拥有自己的生命周期，但它的生命周期会受到它所在Activity的生命周期的控制。例如,当Activity暂停时，该Activity内所有的Fragment都会暂停；当Activity被销毁时，该Activity内所有的Fragment都会被销毁。Android3.0引入Fragment的目的是为了适应大屏幕的平板电脑。Fragment简化了大屏幕UI的设计，不需要开发者管理组件复杂的包含关系。

二、Fragment的作用
-------------

开发者使用Fragment对UI组件进行分组，模块化管理，可以更方便的在运行过程中更新Activity的用户界面。

三、Fragment的特征
-------------

1.Fragment总是作为Activty的界面组成部分，可以调用getActivity()获取它所在的Activity.而Activity则可调用FragmentManager中方法来获取Fragment。
2.一个Activity可以包含多个Fragment,反之，一个Fragment也可被多个Activity复用。

四、Fragment的创建
-------------

与创建Activity类似，创建的Fragment必须继承Fragment基类，android提供了如下图的继承体系:
![这里写图片描述](http://img.blog.csdn.net/20151111233854343)

此外，创建Fragment通常需要实现如下几个方法:
	1.onCreate():系统创建Fragment时调用此方法，在方法中初始化在Fragment中的必要组件。
	2.onCreateView()当Fragment绘制界面组件会回调此方法，此方法返回一个View对象，也就是Fragment所显示的View
	3.onPause() 当用户离开Fragment时调用此方法
下面介绍一个Fragment显示加载一份简单的界面布局文件，并根据传入的参数更新界面组件的程序实现。
代码如下：
```
public class BookDetailFragment extends Fragment
{
	public static final String ITEM_ID = "item_id";
	// 保存该Fragment显示的Book对象
	BookContent.Book book;
	@Override
	public void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		// 如果启动该Fragment时包含了ITEM_ID参数
		if (getArguments().containsKey(ITEM_ID))
		{
			book = BookContent.ITEM_MAP.get(getArguments()
				.getInt(ITEM_ID)); //①
		}
	}

	// 重写该方法，该方法返回的View将作为Fragment显示的组件
	@Override
	public View onCreateView(LayoutInflater inflater
		, ViewGroup container, Bundle savedInstanceState)
	{
		// 加载/res/layout/目录下的fragment_book_detail.xml布局文件
		View rootView = inflater.inflate(R.layout.fragment_book_detail,
				container, false);
		if (book != null)
		{
			// 让book_title文本框显示book对象的title属性
			((TextView) rootView.findViewById(R.id.book_title))
					.setText(book.title);
			// 让book_desc文本框显示book对象的desc属性
			((TextView) rootView.findViewById(R.id.book_desc))
				.setText(book.desc);	
		}
		return rootView;
	}
}
```
上述Fragment用来显示一个简单的布局文件，界面内容由布局文件定义。
	

五、Fragment与Activity的交互
----------------------

创建完Fragment后，为了让它在Activity中显示，则必须将Fragment加载到Activity中，将Fragment加载到Activity中有如下几种方式：
(1).在布局文件中添加Fragment ,元素的android:name属性指定为Fragment的实现类。
(2).在类中通过FragmentTransaction对象的add()方法添加Fragment。如getFragmentManager()可返回FragmentManager,而FragmentManager对象的beginTransaction()可开启并返回FragmentTransaction对象。
实例如下:
1.Activity会通过布局文件使用定义好的BookDetailFragment ，此Activity的左边显示一个ListFragment,右边会显示一个Fragment容器，该容器会动态更新Fragment显示的内容。

```
<?xml version="1.0" encoding="utf-8"?>
<!-- 定义一个水平排列的LinearLayout，并指定使用中等分隔条 -->
<LinearLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	android:orientation="horizontal"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	android:layout_marginLeft="16dp"
	android:layout_marginRight="16dp"
	android:divider="?android:attr/dividerHorizontal"
	android:showDividers="middle">
	<!-- 添加一个Fragment -->
	<fragment
		android:name="org.crazyit.app.BookListFragment"
		android:id="@+id/book_list"
		android:layout_width="0dp"
		android:layout_height="match_parent"
		android:layout_weight="1" />
	<!-- 添加一个FrameLayout容器 -->
	<FrameLayout
		android:id="@+id/book_detail_container"
		android:layout_width="0dp"
		android:layout_height="match_parent"
		android:layout_weight="3" />
</LinearLayout>
```
2.Activity代码:

```
public class SelectBookActivity extends Activity implements
		BookListFragment.Callbacks
{
	@Override
	public void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		// 加载/res/layout目录下的activity_book_twopane.xml布局文件
		setContentView(R.layout.activity_book_twopane);
	}
	// 实现Callbacks接口必须实现的方法
	@Override
	public void onItemSelected(Integer id)
	{
		// 创建Bundle，准备向Fragment传入参数
		Bundle arguments = new Bundle();
		arguments.putInt(BookDetailFragment.ITEM_ID, id);
		// 创建BookDetailFragment对象
		BookDetailFragment fragment = new BookDetailFragment();
		// 向Fragment传入参数
		fragment.setArguments(arguments);
		// 使用fragment替换book_detail_container容器当前显示的Fragment
		getFragmentManager().beginTransaction()
			.replace(R.id.book_detail_container, fragment)
			.commit();  //①
	}
}
```

由此可见，将Fragment添加到Activity后，Fragment必须与Activiity交互信息，则可按如下方法进行
1.调用Fragment下的getActivity()获取它所在的Activity。
2.调用Activity关联的FragmentManager的findFragmentById(int id)获取指定的Fragment。
此外,Fragment还可以与Activity进行数据传递，比如:
1.Fragment向Activity传递数据:则在Fragment中定义一个内部回调接口。
2.Activity向Fragment传递数据:则在Activity中创建数据包Bundle 后调用Fragment的setArguments(Bundle bundle)方法可将Bundle中的数据传给Fragment。
