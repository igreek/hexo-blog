title:  swiper图片轮播插件
date: 2015-11-05 16:28:00
categories: 前端插件
tags: [图片轮播,swiper]
toc: true
---
>实现图片轮播的幻灯片的效果的插件有很多，如touchsiler,Swiper，silerbox等等，各有独自的API和自定义效果。下面要说的是关于swiper的使用。
<!--more-->
一、swiper简介
----------

	实现图片轮播的幻灯片的效果的插件有很多，如touchsiler,Swiper，silerbox等等，各有独自的API和自定义效果。下面要说的是关于swiper的使用。
	Swiper 是一款免费以及轻量级的移动设备触控滑块的js框架，使用硬件加速过渡（如果该设备支持的话）。主要使用于移动端的网站、移动web apps，native apps和hybrid apps。
	主要是为IOS而设计的，同时，在Android、WP8系统也有着良好的用户体验，Swiper从3.0开始不再全面支持PC端。
	因此，如需在PC上兼容更多的浏览器，可以选择Swiper2.x（甚至支持IE7）。
	中文官网：[http://www.swiper.com.cn/]
	

二、默认参数说明
--------

如:var swiper = new Swiper('.swipe3', {
			    spaceBetween: 10,
			    centeredSlides: false,
			    slidesPerView : 3,//设置slider容器能够同时显示的slides数量(carousel模式)
				initialSlide : 0,//设定初始化时slide的索引。
			    autoplay: 5000,//播放的间隔时间
				loop: true,//是否循环播放
			    autoplayDisableOnInteraction: false,//用户操作swiper之后，是否禁止autoplay。默认为true：停止
			});

三、使用教程
------

1.首先加载插件，需要用到的文件有swiper.min.js和swiper.min.css文件。	

```
<!DOCTYPE html>
<html>
<head>
    ...
    <link rel="stylesheet" href="path/to/swiper.min.css">
</head>
<body>
    ...
    <script src="path/to/swiper.min.js"></script>
</body>
</html>
```
如果你的页面加载了jQuery.js或者zepto.js，你可以选择使用更轻便的swiper.jquery.min.js。

```
<!DOCTYPE html>
<html>
<head>
    ...
    <link rel="stylesheet" href="path/to/swiper.min.css">
</head>
<body>
    ...
    <script src="path/to/jquery.js"></script>
    <script src="path/to/swiper.jquery.min.js"></script>
</body>
</html>
```
2.HTML内容。

```
<div class="swiper-container">
    <div class="swiper-wrapper">
        <div class="swiper-slide">Slide 1</div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
    </div>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>
    
    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
    
    <!-- 如果需要滚动条 -->
    <div class="swiper-scrollbar"></div>
</div>
```
导航等组件可以放在container之外
3.你可能想要给Swiper定义一个大小，当然不要也行。	
.swiper-container {
    width: 600px;
    height: 300px;
} 	
4.初始化Swiper

```
<script>        
  var mySwiper = new Swiper ('.swiper-container', {
    direction: 'vertical',
    loop: true,
    
    // 如果需要分页器
    pagination: '.swiper-pagination',
    
    // 如果需要前进后退按钮
    nextButton: '.swiper-button-next',
    prevButton: '.swiper-button-prev',
    
    // 如果需要滚动条
    scrollbar: '.swiper-scrollbar',
  })        
  </script>
```
如果不能写在HTML内容的后面，则需要在页面加载完成后再初始化。

```
<script type="text/javascript">
window.onload = function() {
  ...
}
</script>
或者这样（Jquery和Zepto）

<script type="text/javascript">
$(document).ready(function () {
 ...
})
</script>
```
5.完成。恭喜你，现在你的Swiper应该已经能正常切换了，如果没有，你可以参考下这个测试包。现在开始添加各种选项和参数丰富你的Swiper，开启华丽移动前端创作之旅。

插件下载地址:
1.http://www.swiper.com.cn/download/index.html
2.http://download.csdn.net/detail/xhwwc110/9244457
