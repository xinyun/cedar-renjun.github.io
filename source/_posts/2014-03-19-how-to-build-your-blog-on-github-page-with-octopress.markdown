---
layout: post
title: "用Octopress在github上搭建自己的博客系统"
date: 2014-03-19 20:04:41 +0800
comments: true
categories: [github, octopress]
---

##	写在前面的话
	
搞软件的，一般都晓得[github](https://github.com)和[stackoverflow](http://stackoverflow.com/)这两个著名网站，前者是分布式代码仓库，后者是技术问答社区，这里不做过多介绍

github在提供分布式代码管理功能的同时，也提供pages功能。借助于pages，程序员可以创建一个属于自己的博客

接下来，我会图文并茂的展示如何在github上搭建自己的博客系统


进行正式介绍之前，先界定适用人群和技术基础，免得浪费您的时间


**适用人群**   
喜欢折腾的IT攻城狮

**技术基础**   
了解git常用操作，熟悉markdown语法，熟悉HTML，CSS更好

**操作环境**   
windows7


##	1.	部署git

git是所有操作的基石，让我们来在本地部署git

注：  
*	如果您是git新手，请严格按照操作步骤，一步一步进行；  
*	如果您非常熟悉git，请直接阅读1.4章节(配置)部分

### 1.1  创建git账户和启用page空间

首先我们需要有一个github的账户，在浏览器中敲入下面网址：  

	https://github.com/

您会看到github的主页，

![Git signup](/images/2014/03/20/git_signup.jpg "Git Signup")

这里我们注册一个新的账号，账户信息如下：

	账号名称：Git-Octopress-Demo
	账号密码：Git-Octopress-123
	邮箱地址：git_octopress_demo@163.com

点击`sigin Up for GitHub`后，出现下面界面

![Git Signup Conform](/images/2014/03/20/git_signup_conform.jpg "Git Signup Conform")

首先确认`Choose your personal plan`一栏中选择的是`Free`，然后直接点击`Finsh sign up`即可  
在接下来的界面中，点击`New repository`按钮来创建一个新仓库  

![Create new repository 1](/images/2014/03/20/Create_new_repository_1.jpg "Create new repository 1")

点击后，出现下面github仓库信息界面

![Create new repository 2](/images/2014/03/20/Create_new_repository_2.jpg "Create new repository 2")

这里，仓库名称为必选项，且名字格式是固定的，为

	XXX.github.io

形式，XXX是用户的github账户名，所以，我们在仓库名一栏填写如下信息

	Git-Octopress-Demo.github.io

接下来是仓库描述和README信息，为可选项，可以不填  

填写完成后，点击`Create Repository`完成创建工作，接下来到邮箱里面去验证信息，这里不做过多介绍

至此，我们已经成功的创建了一个github账户，并创建了一个page空间。其实，git上的博客系统分为两类  

**用户page空间**  
该空间是以用户名创建的page空间，挂载在master主分支上，每个用户最多只能有一个该空间

**仓库page空间**  
该空间是github为每个仓库创建的page空间，必须挂载在gh-pages分支上，用户创建任意数量的仓库空间

更多信息，请参考[github page帮助页](https://help.github.com/articles/user-organization-and-project-pages)


### 1.2  下载和安装git客户端
注册github账户后，我们需要在本地PC上安装git客户端  
您可以通过下面网址来下载最新windows版本git，并获得相应帮助

	https://help.github.com/articles/set-up-git

这个是在线安装的，过程很简单，不过因为git软件部署在亚马逊的AWS上，所以下载时间稍微有点长，请耐心等待

安装过程中，可能会输入用户名和用户密码

接下来，我们再配置全局用户名

	git config --global user.name "Git-Octopress-Demo"
	# Git-Octopress-Demo 表示用户名，这里填写您自己的名字

配置全局邮箱地址

	git config --global user.email "git_octopress_demo@163.com"
	# git_octopress_demo@163.com 表示用户邮箱地址，这里替换为您自己的邮箱地址

**注：命令中的两个双引号是可选的**

### 1.3  配置

##	部署Ruby
### 下载Ruby for windows
### 安装Dev Kit

##	部署Octopress
### 下载和安装octopress
### 安装slash主题


##	部署Python
### 安装
### 代码高亮

##	发布博客系统
### 创建第一个博文
### 发布到github上


##	注意事项

0.	经测试，`rake preview`无法生效，待研究
0.	markdown原始文本需保证为UTF-8编码格式
0.	经测试，octopress在Ruby 2.0.0环境下无法正常工作
0.	如果您发现采用代码高亮后，输出一片空白，请考虑1）是不是python2.7.X，是不是添加环境变量到系统中
0.	如果无法预览，请检查浏览器设置中，是否使用了代理模式
0.	插入文档和图片资源


##	参考资料

0.	[github主页](https://github.com/)
0.	[github pages](http://pages.github.com/)
0.	[markdown语法](http://github.github.com/github-flavored-markdown/)
0.	[markdown在线编辑器，stackio](https://stackedit.io/)
0.	[jekyll项目主页](http://jekyllrb.com)
0.	[w3cschool网络技术培训](http://www.w3cschool.cn/)
0.	[octopress项目主页](http://octopress.org)
0.	[slash主题](http://zespia.tw/Octopress-Theme-Slash/index.html)
