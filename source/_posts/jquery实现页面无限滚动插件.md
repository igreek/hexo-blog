title:  jquery实现页面无限滚动插件
date: 2015-11-07 19:28:00
categories: 前端插件
tags: [无限滚动插件,Autobrowse]
toc: true
---
>实现网页底部自动加载内容的插件很多，如jQuery ScrollPagination，jQuery Screw,Autobrowse;下面主要介绍Autobrowse的使用。
<!--more-->
一、插件概述
------

实现网页底部自动加载内容的插件很多，如，
1、jQuery ScrollPagination
jQuery ScrollPagination plugin 是一个jQuery 实现的支持无限滚动加载数据的插件。
地址：http://andersonferminiano.com/jqueryscrollpagination/
他的demo下载：http://andersonferminiano.com/jqueryscrollpagination/jqueryscrollpagination.zip
2.jQuery Screw
Screw (scroll + view) 是一个 jQuery 插件当用户滚动页面的时候加载内容，是一个无限滚动翻页的插件。
官方地址：https://github.com/jasonlau/jQuery-Screw
Autobrowse jQuery Plugin 插件在用户滚动页面的时候自动通过 Ajax 加载更多内容，使用浏览器内置缓存。
3. AutoBrowse jQuery Plugin
Autobrowse jQuery Plugin 插件在用户滚动页面的时候自动通过 Ajax 加载更多内容，使用浏览器内置缓存。
官方地址：https://github.com/msjolund/jquery-esn-autobrowse

二、插件使用说明
--------

下面介绍一下Autobrowse的使用：
1.引入js依赖文件
```
<script  type="text/javascript" src="../plugin/jquery.js"></script> 
<script type="text/javascript" src="../plugin/jquery.esn.autobrowse.js"></script>
```
2.定义HTML元素
```
<div class="full-dom">
	<ul class="list-y2" id="activityList">
	</ul>
</div>
```
3.js定义

```
$("#activityList").autobrowse({
		url:function (offset) {
			//请求服务器端地址
		}, template:function (data) {
			//异步组装服务器端返回的数据
		},
		 itemsReturned:function (data) {
			//返回服务端数据的长度
		}, 
		offset:1,
		max:10000, 
		loader:'' //加载的图标,
		useCache:false, //使用缓存
		expiration:1,//过期时间
		sensitivity: 2000 //触发下一页的差值
		finished: function () { $(this).append('<p style="text-align:center">加载完成，没   有更多活动了</p>') }//没有数据时的提示
	});
```
 从上可看出，autobrowse可以自定义参数，来触发页面底部自动加载数据的时间和内容，能带来好的用户体验。

三、依赖文件下载
--------
http://download.csdn.net/detail/xhwwc110/9244831