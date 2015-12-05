title:  git教程详解（一）
date: 2015-11-27 15:47:17
categories: git教程
tags: [git,版本管理]
toc: true
---
git是一个很好的分布式版本管理工具，在git中，版本库又叫仓库，英文名为repository，可以简单的理解为本地的一个目录，用git进行管理，目录内的文件git进行删除，修改等操作时，git都可以跟踪，从而记录仓库的操作历史。
<!--more-->

#### **一、git简介**
git中，版本库又叫仓库，英文名为repository，可以简单的理解为本地的一个目录，用git进行管理，目录内的文件git进行删除，修改等操作时，git都可以跟踪，从而记录仓库的操作历史。
### **二、创建仓库**
在本地创建一个目录，如/user/temp/test/，然后在此目录下执行git init，就可将目录变成git管理的仓库。
![这里写图片描述](http://img.blog.csdn.net/20151127202612430)
执行完后，是一个空仓库，包含一个隐藏的.git目录，若没看到该目录，linux上可以用 ls -ah 查看.
**(1).将文件添加到版本库**
所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，
只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道	。
1.现在在test目录或其子目录下添加一个readme.txt ,将该文件放到git仓库方式如下:
```
git add readme.text
git commit -m "add new file"
```

上述命令中 -m 为本次提交的注释，是为了方便他人查看，所以需要加上。
2.若git想批量添加文件，可以将多个文件用逗号分隔，然后commit。
```
git add test1.txt test2.txt
git commit -m "add new file"
```

**(2).查看仓库的状态**
git status命令可以让我们时刻掌握仓库当前的状态。如图:

上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
git diff可以告诉我们修改了什么内容。
```
git diff readme.txt 
```

**(3).版本回退**
git会将修改的文件保存一个快照，一旦你把文件修改乱了或误删除文件，可以从最近的快照恢复，而不影响工作。


 - git log 查看版本控制系统的修改记录
	![这里写图片描述](http://img.blog.csdn.net/20151127203423789)
	git log命令显示从最近到最远的提交日志，上述操作我们可以看到有2次提交，每次提交后，都会有一个commit Id(版本号).

 - git reset 回退版本
	
	Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
git reset -- hard HEAD^ 比如要将当前版本回退到上一个版本 ,HEAD^也可换成版本号.

 - git reflog 恢复版本
	
	当你用 $ git reset --hard HEAD^
	
	回退到上一个版本时，再想恢复回来，就必须找到恢复版本的commit id。Git提供了一个命令git reflog用来记录你的每一次命令：git reflog
	![这里写图片描述](http://img.blog.csdn.net/20151127203653884)
**(4)删除文件**
1.一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了  $ rm test.txt
2.git rm file 从版本库删除该文件，并且git commit
3.git checkout -- test.txt 用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

### **三、远程仓库**

远程仓库可以是在一台运行git的服务器上创建一个的仓库。类似网站如gitcafe ,github等都可进行仓库的托管。只要注册一个账号，可以获得免费的远程仓库。
由于在github上，你本地仓库和远程仓库的传输是通过ssh加密的。所以需要进行如下设置:
1.创建SSH key
```
ssh-keygen -t rsa -C "***********@163.com"
```

创建成功后，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
2.登录github，配置ssh key ，如图所示:

![这里写图片描述](http://img.blog.csdn.net/20151127203849320)

图中的Title任意填上，在Key文本框里粘贴id_rsa.pub文件的内容。
github 支持添加多个不同的key,假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了.
**(1)、添加仓库**
在上面操作配置好ssh key 后，就可以创建远程git仓库了，在github上，在右上角找到“Create a new repo”按钮，创建一个新的仓库。如图所示
![这里写图片描述](http://img.blog.csdn.net/20151127204002038)

创建好仓库后，需要和你本地的仓库进行关联，然后把本地的内容push到远程仓库中，则你需要在的本地库目录下，执行如下操作:
```
git remote add origin git@github.com:igreek/Timber.git
```

执行后，你的远程库为origin。但此时还是没有内容，若把关联的本地库的内容push到origin上，则需要执行如下操作:

```
git push -u origin master  
```

参数-u:第一次push时需要，以后可以省略。
推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样。


**(2)、从远程库克隆**
假如没有在本地创建仓库，在远程创建了一个仓库，想把远程仓库的内容克隆到本地的一个目录下。可以执行如下操作:
```
git clone origin git@github.com:igreek/Timber.git
```

从上面的操作都是使用ssh协议，其实也可以其他的https协议，但它速度有些慢，端口有限制。

总结:本篇主要介绍了git的作用，git 与其他版本工具的一些差异，git的创建仓库以及如何管理仓库等功能，涉及到一些git命令需要熟悉，git也是一个很实用的分布式系统，下篇将介绍git的分支管理。