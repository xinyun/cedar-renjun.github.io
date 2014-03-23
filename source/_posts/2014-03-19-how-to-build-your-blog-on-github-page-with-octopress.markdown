---
layout: post
title: "用Octopress在github上搭建自己的博客系统"
date: 2014-03-19 20:04:41 +0800
comments: true
categories: [github, octopress]
---

【文档修订中，随时更改】


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
windows7 32位


<!-- more -->


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

Octopress是用Ruby语言编写的，所以我们在本机上需要安装Ruby环境和DevKit

**特别注意:** Ruby和DevKit的安装目录不能含有空格

### 下载Ruby for windows
rubyinstaller号称是windows上最简单的ruby安装方式，官方网址如下所示：

	http://rubyinstaller.org/

我们首先安装Ruby 1.9.3版本，这里要特别注意，必须是1.9.3版本，其它版本的不能很好的和octopress协同工作  
下载链接如下所示：

	http://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-1.9.3-p545.exe

占位符，图示安装过程

在安装快结束时，请确保勾选这3个选项

### 安装Dev Kit
因为后面会大量用到gem命令，所以需要Dev Kit

**特别注意：**
无论您的系统是32位还是64位，这里的DevKit必须是32位版本，下载链接如下所示：

	https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe

下载完成后，您会发现DevKit是一个自解压文件包，需要手工输入一些命令才能正确安装，这里我们先把DevKit解压到任何一个固定目录，假设为DevKit_Dir，接下来打开一个CMD窗口并CD到DevKit_Dir目录下

**Tips：**如果你用的是win7，可以先选中DevKit_Dir文件夹，按住shift，然后鼠标右键，这时菜单项中会出现`在此处打开命令窗口`，选择该命令，就可以直接开启一个终端，并自动CD到选中的文件夹目录下，非常好用

在CMD终端里输入以下两条指令

	ruby dk.rb init
	ruby dk.rb install

如果输完命令后，终端出现以下提示文字，则说明DevKit安装成功

占位符：官方Devkit提示文字


另官方提供的验证方法如下，供参考：

>	Confirm your Ruby environment is correctly using the DevKit by running gem install json --platform=ruby. JSON should install correctly and you should see with native extensions in the screen messages. Next run ruby -rubygems -e "require 'json'; puts JSON.load('[42]').inspect" to confirm that the json gem is working.


关于Devkit的更多详细信息，请参考：  
https://github.com/oneclick/rubyinstaller/wiki/Development-Kit


##	部署Python
octopress官方没有说要安装python，但如果您需要代码高亮功能，则必须安装python，为了避免后期麻烦，我们建议安装ActivePython-2.7.6.9-win32-x86.msi，下载链接如下：

	http://downloads.activestate.com/ActivePython/releases/2.7.6.9/ActivePython-2.7.6.9-win32-x86.msi

在安装后，手工将python可执行路径添加到系统环境变量

至此，我们完成了Python的部署


##	部署Octopress

### 下载和安装octopress
CD到MyBlog目录下，然后输入下面的指令

	git clone git://github.com/imathis/octopress.git octopress
	cd octopress

octopress下载完毕，下面安装依赖项

	gem install bundler	
	bundle install

安装过程中需要从网上下载一些更新包，第一次时间稍微有些长，请耐心等待

### 配置slash主题

slash主题是专为octopress设计的极简风格主题，不仅具有默认主题所有功能，还能自适应各种终端设备，自动调整图片和视频大小，同时具有丰富的插件


在终端中输入如下命令来下载并安装slash主题
	
	git clone git://github.com/tommy351/Octopress-Theme-Slash.git .themes/slash
	rake install ['slash'] 
	rake generate

**特别注意**  如果使用上面命令后，无法正确显示主题，请用下面命令

	git clone git://github.com/tommy351/Octopress-Theme-Slash.git .themes/slash
	rake install \['slash'\]
	rake generate

来代替

至此，octopress和对应主题安装完毕


##	发布博客系统

接下来让我们发布我们的第一个博客文章

	gem new_post["Hello world"]

进入`\source\_post`文件夹下，就可以看到`2014-03-19-Hello-world.markdown`文件，这时，您可以使用任何顺手的文本编辑工具来编辑该文件，同时需要记得将该文件的编码格式调整为`UTF-8 无BOM`

编辑完成后，接下来，我们输入下面的指令还生成相应的静态网站

	rake generate  # 生成静态网站

我们可以通过下面的指令来在本地预览网站

	rake preview # 预览网站

不用关闭窗口，直接在浏览器中打开`http://localhost:4000/`
这时，当我们修改*.markdown文件后，octopress会自动重新生成静态网站，这时，我们只需要在浏览器中手工刷新界面，就可以看到修改后的效果

当我们编写成满意的网站之后，便可以用下面指令，把新作的修改推送到github服务器上

	rake deploy # 推送更新

这仅仅是将master分支推送到了github服务器，我们可以通过下面指令来手工将source分支也推送到github服务器上去
	
	git add .                      # 添加所有更新
	git commit -m "First Commit"   # 编写commit记录	 
	git push origin source         # 推送source分支到github source分支


##  注意事项

**1 正确显示中文文档的方法**

* md文件采用英文命名方式  
* md中title项可以为中文，同时用双引号包括中文字符串  
* md正文可以采用中文  
* md文件编码格式为UTF-8，无BOM


**2 输入`rake preview`命令后，在浏览器中打开`localhost:4000`，打开速度非常慢或者完全打不开**

* 请检测浏览器是否设置代理模式，如果有，则取消


**3 代码无高亮或者markdown中含有代码高亮部分时，博客系统无法显示，或者输出一片空白**  
请考虑

* python的版本是不是2.7.X  
* python路径是不是添加环境变量到系统中

**4 出现Error:invalid byte sequence in GB2312**

这是中文编码的错误，如果是写英文博客就不会出错，这似乎是 Jekyll 的一个 bug，解决方法是将 Ruby 安装文件路径下的 .\lib\ruby\gems\1.9.1\gems\jekyll-1.1.2\lib\jekyll\convertible.rb 文件第 31 行：

	self.content = File.read(File.join(base, name))

修改为：

	self.content = File.read(File.join(base, name), :encoding => "utf-8")

这样便可成功启动本地服务器进行调试。


**5 代码高亮中的linenos和mark无效**

这个暂时无解




##	参考资料

0.	[github主页](https://github.com/)
0.	[github pages](http://pages.github.com/)
0.	[markdown语法](http://github.github.com/github-flavored-markdown/)
0.	[markdown在线编辑器，stackio](https://stackedit.io/)
0.	[jekyll项目主页](http://jekyllrb.com)
0.	[w3cschool网络技术培训](http://www.w3cschool.cn/)
0.	[octopress项目主页](http://octopress.org)
0.	[slash主题](http://zespia.tw/Octopress-Theme-Slash/index.html)

