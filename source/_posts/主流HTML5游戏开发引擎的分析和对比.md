title: 主流HTML5游戏开发引擎的分析和对比
date: 2015-10-22 15:47:17
categories: HTML5开发
tags: [HTML5,游戏引擎]
toc: true
---
HTML 5是近十年来Web开发标准最巨大的飞跃。和以前的版本不同，HTML 5并非仅仅用来表示Web内容，它的新使命是将Web带入一个成熟的应用平台，在HTML 5平台上，视频，音频，图象，动画，以及同电脑的交互都被标准化
<!--more-->
## 一、引擎说明
本文主要选取了Construct2、ImactJS、LimeJS、GameMaker、CreateJS、lycheeJS、Crafty、three.js、melonJS、Turbulenz、Quintus、Cocos2d-html5等进行了简要介绍和对比，主要是根据网上的资料整理而成。
主流框架对比

### Construct2
Construct 2是一个运行于Windows平台的游戏制作工具，它可以让没有任何编程基础的用户在短时间内不写一行代码快速开发出一款可运行于所有平台（Windows、Mac、Linux、Android、iOS等）的游戏。免费版可以将游戏导出成HTML5。收费版本分为个人版（79英镑）和企业版（259英镑），可以导出所有平台的版本，同时提供了更多的特效和音乐。如果使用该工具盈利超过5000美元，需要升级到企业版。

优点：
1. 简单易用，可实时运行游戏
2. 强大的事件系统，可以不通过写代码来控制游戏逻辑
3. 提供了可编程扩展的接口
4. 提供了大量特效，支持物理效果
5. 支持所有平台
6. 完整的文档以及社区支持

缺点：
不如直接写代码灵活

参考资料：
1. 官方网站
2. Construct 2 vs. Javascript

### ImpactJS
ImpactJS是一个基于JavaScript的HTML5游戏引擎，同时支持PC和移动平台浏览器。它是目前除了Construct2之外最受欢迎的HTML5游戏引擎，使用需要支付99美元。

优点：
1. 提供了灵活的关卡编辑器，可以快速构建游戏地图
2. 提供了强大的调试工具
3. 提供了Ejecta可以将JavaScript的执行结果通过OpenGL渲染出来，可以在iOS平台上获得与原生应用相近的效率
4. 文档齐全，有两本专门介绍ImpactJS开发的书
5. 支持物理效果
6. 支持自己编写插件来扩展

App Store游戏
1. Biolab Disaster
2. Drop JS

### LimeJS
LimeJS是一个基于Google Closure Library开发的HTML5游戏框架，继承了Closure代码易读易懂、架构清楚的特性。同时提供了游戏中各种通用实体的封装，如Director、Scene、Layer、Event和Animation等，与Cocos2d的API类似。它是由Digital Fruit公司创建。

优点：
1. 基于Apache协议的开源框架
2. 功能强大，文档齐全，与ImactJS类似
3. 支持物理效果
4. 与Cocos2d的API类似，容易上手

缺点：
依赖于Google Closure

### GameMaker
GameMaker与Construct 2类似，都是一个游戏制作工具，可以导出到各个平台运行，分为免费版、标准版（49.99美元）、专业版（99.99美元）和大师版（799.99美元）。其中免费版只能导出Mac和Windows版本，导出HTML5需要大师版或者专业版（再额外支付99.99美元）。

优点和缺点：
优势与Construct2类似，但性价比不如Construct2高

### CreateJS(EaselJS)
CreateJS是Adobe官方赞助的开源开发框架，它大部分API都是基于Flash原有的API来模仿实现的，并且官方提供了直接把Flash动画转成JS数据包的工具，调用起来很方便。CreateJS提供了若干开发套件及工具，分别是：EaselJS（负责图形、事件、触控、滤镜等功能）、TweenJS（补间动画）、SoundJS（音频控制）、PreloadJS（文件加载）和Zoë（生成图片精灵及动画数据）。

优点：
1. Flash开发者很容易上手
2. 提供了Flash转html5的工具，可以将部分Flash代码进行转换再修改
3. 基于MIT协议的开源框架
4. 类库设计非常独立，包含不同的模块，可选择性使用

### lycheeJS
lycheeJS是一个环境独立的JavaScript游戏引擎，可以在任何支持JavaScript的环境中运行。它的理念是做最快的JavaScript游戏引擎。

优点：
1. 同时支持PC（Firefox、Chrome、Opera、Safari、IE）和移动平台（WebKit、Chrome、Firefox、Safari）的浏览器
2. 提供了CDN、WebSockets、SPDY、HTTP2.0以及游戏截图的支持
3. 提供了可以直接导出第三方（Facebook、AppStore、Google Play Store）资源包来发布
4. 基于MIT协议的开源框架

### Crafty
Crafty是一个体积小、简单、轻量级的2D的HTML5游戏引擎，它提供了通过Canvas或DOM来绘制实体，提供了精灵Map以及SAT高级碰撞监测支持。它是由个人（Louis Stowasser）创建，同时由Github上的一些开发者共同开发。

优点
1. 体积小
2. 轻量级引擎，不会受到框架的太多束缚
3. 同时支持PC和移动平台浏览器

### three.js
Three.js是一个轻量级的JavaScript库，用于在浏览器上创建和显示3D图形。它可以同时使用Canvas、SVG或WebGL进行绘制。

优点和缺点：
支持3D，但是不适合做2D游戏

### melonJS
melonJS是melonJS团队对Javascript热情以及开发经验的结晶，是一个简单、免费、而且独立的类库。

优点
1. 轻量级的2D引擎
2. 支持所有主流的PC和移动平台浏览器
3. 支持使用Tiled map editor来创建和编辑地图
4. 支持多声道音频
5. 基于MIT协议的开源框架

### Turbulenz
Turbulenz是一个开源的HTML5游戏引擎，提供了可以运行在Windows、MacOS、Linux上的SDK，允许开发人员创建高质量和硬件加速的2D、3D游戏。包括以下功能：异步资源加载、进行特效和粒子渲染、支持物理效果、碰撞检测以及动画、3D音效支持、支持网络交互以及社交网络分享、场景和资源的管理。

优点：
1. 功能强大，同时支持2D和3D
2. 基于MIT协议的开源引擎

### Quintus
Quintus是一个容易上手、轻量级、且模块化的HTML5游戏引擎。它引用面向对象的思想来进行HTML5游戏开发，同时依赖于jQuery来提供事件处理机制和元素选取操作。

缺点
1. 依赖于jQuery
2. 目前引擎仍处于初级阶段，还很不成熟

### Cocos2d-html5
Cocos2d-html5是一款基于Cocos2d-x API的2D开源免费HTML5游戏引擎。它目前通过canvas进行渲染，将来会支持WebGL。它由国内Cocos2d-x核心团队主导开发和维护，行业领袖、HTML5大力推动者Google为这个项目提供支持。同时，Zynga、Google等大公司的工程师也参与到它的设计工作中。

优点：
1. 与Cocos2d的API类似，容易上手
2. 中文文档齐全，资料丰富
3. 基于MIT协议的开源引擎

## 二、各框架具体参数对比
1. 各HTML5游戏框架对比HTML5 Game Engines
2. List of JS Game Engines
3. 对于Crafty、Lime、Frozen、Melon、Impact、Quintus框架，可以在Breakouts上查看用这些引擎开发同一个游戏的效果以及代码风格。Breakouts中使用到的特性包括碰撞检测、精灵动画、音效、地图、场景切换、交互、文字渲染、移动平台支持。
4. 以上各引擎中，除了Construct2、ImpactJS、GameMaker是收费的之外，其他引擎都是免费并且开源的。对于开源引擎，我们可以从Github上面的关注度了解到该引擎的流行程度，关注的人越多，遇到问题越容易解决。同时一般来说，项目开发者越多，版本更新越快；项目的进行时间越长则越成熟。下面将对各开源引擎的开发者人数、项目启动时间、关注度进行对比。
Game Engine	Github commits	Github contributors	Start time	Github Star	Github Fork
LimeJS	532	22	2011.1.19	1091	187
EaselJS	784	15	2011.1.23	2758	650
lycheeJS	4	1	2012.9.5	110	20
Crafty	1182	67	2010.11.5	993	225
three.js	6409	198	2010.3.23	12691	2816
melonJS	1287	15	2011.4.11	643	137
Turbulenz	736	12	2013.4.26（最近才开源）	1522	207
Quintus	118	11	2012.8.4	450	89
Cocos2d-html5	2706	39	2012.1.28	735	303

## 三、总结
以上各引擎中，Construct2、ImpactJS、GameMaker三个是收费的，其中Construct2与GameMaker更像一个游戏开发工具，可以实现不用写一行代码来制作游戏，更适合于没有编程基础的人使用。而ImpactJS作为一个高质量的框架，且易于扩展，虽然是收费的，但是物有所值。

开源引擎中，three.js是最火的，但是仅限于开发3D游戏。其次是CreateJS，由Adobe官方赞助且采用Flash类似的API以及模块化开发，是Flash开发者以及将Flash游戏转换成html5不可多得的选择。Turbulenz虽然开源时间比较晚，但颇有后来者居上的趋势，由于其对2D和3D的同时支持，是同时开发2D和3D游戏的最佳选择。LimeJS与Crafty相比的优势在于有一个公司进行维护，相比个人要更稳定，但是需要依赖于Google Closure，也使之成为一个重量级的框架。Crafty体积小、轻量级，更适合于小游戏的开发。Cocos2d-html5作为国产框架的一个优势在于中文文档和教程多，且得到了Google的支持，但相比ImpactJS、CreateJS仍不够成熟。melonJS、Quintus、lycheeJS的开发者和使用者都较少，相关文档和教程也相对少，还有待观察。

参考资料
1. JavaScript Game Engine Comparison

转自:http://blog.csdn.net/zhaoxy_thu/article/details/11867123