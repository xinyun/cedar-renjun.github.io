---
layout: post
title: "Doxygen 10 分钟入门教程"
date: 2014-03-21 22:02:07 +0800
comments: true
categories: [教程，Doxygen]
---

【修订版本，随时更改】


## 综述

本文试图在10分钟内，帮助您了解文档生成工具Doxygen的基本概念，并熟悉Doxygen的使用规则，同时给予深入学习的一些方向性建议。

因为本文仅仅是帮助初学者入门，所以选例方面较为简单，缺乏广度和深度。后期会编写一些深度学习Doxygen的文章，敬请关注


## Doxygen是什么东西？

Doxygen是一款文档生成工具，它可以从代码中提取出相应的文档，并组织，输出成各种漂亮的文档（如HTML，PDF，RTF等）

有了Doxygen工具，程序员便可以在写代码的时候，直接内嵌文档，再也不需要为某个功能代码单独写文档，从而最大程度的保持了文档和代码的统一性

另外，Doxygen 1.8.x版本中增加对markdown的支持，也支持内嵌部分HTML标签，从而极大的简化了文档编写难度，甚至，您可以用Doxygen生成一个静态的网站。

目前Doxygen支持C/C++，Objective-C, C#，PHP等语言，支持多平台(Mac OS, Linux, Windows)，更多信息，请参考[Doxygen官方介绍](http://www.stack.nl/~dimitri/doxygen/index.html)


## Doxygen适合什么人？

适合对代码文档有一定要求的程序员

**PS：**能看到这里的程序员，一定是位有追求，有理想的工程狮，哈哈，就你了，go on please


## 开始学习Doxygen

**注：**在继续阅读之前，请确保您

*	知道如何在windows环境下调出命令窗口
*	具有简单的编程基础

### 下载和安装

doxygen最新版为1.8.6版本，下载链接如下

	http://sourceforge.net/projects/doxygen/files/latest/download?source=files

下载完成后，双击安装，采用默认设置就ok，这里不做过多介绍

### 准备源文件

这里准备了三个简单的C语言源代码

	main.c  // 演示如何调用Dev中的设备接口
	dev.c   // Dev设备的实现代码
	dev.h   // Dev设备的操作接口

具体代码如下：  

{% include_code lang:c 2014/03/21/OriginSourceCode/main.c %}
{% include_code lang:c 2014/03/21/OriginSourceCode/dev.c %}
{% include_code lang:c 2014/03/21/OriginSourceCode/dev.h %}


将这3个源文件放在某个文件夹内，这里以GettingStart文件夹为例，其目录组织结构如下所示

	GettingStart
	   |-- dev.c
	   |-- dev.h
	   |-- main.c

### 生成文档
接下来，我们看看，不编写任何注释的情况下，Doxygen会怎么生成文档

打开命令行(powershell)窗口，并CD到GettingStart目录下，输入下面命令

	doxygen -g  

Powershell的返回信息如下，同时，我们的GettingStart目录下增加了一个名叫Doxyfile的文件(注意第21行)


{% codeblock lang:powershell %}

F:\Doxygen_Demo\GettingStart> doxygen -g

Configuration file `Doxyfile' created.

Now edit the configuration file and enter

  doxygen Doxyfile

to generate the documentation for your project

PS F:\Doxygen_Demo\GettingStart> ls


    dir: F:\Doxygen_Demo\GettingStart

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         2014/3/22     15:44        212 dev.c
-a---         2014/3/22     16:14        119 dev.h
-a---         2014/3/22     16:46     103431 Doxyfile
-a---         2014/3/22     15:56        236 main.c

{% endcodeblock %}


这个Doxyfile就是Doxygen工程的配置文件，里面含有一些配置信息

接下来我们做一些修改

在默认情况下，Doxygen会输出HTML和LATEX形式文档，LATEX主要用于生成PDF，这里暂时不需要，所以我们禁用LATEX输出

在Doxyfile中将下面一行

	GENERATE_LATEX         = YES

修改为

	GENERATE_LATEX         = NO

接下来，我们修改HTML的显示方式，将下面两行代码

	DISABLE_INDEX          = NO  
	GENERATE_TREEVIEW      = NO  

修改为

	DISABLE_INDEX          = YES  
	GENERATE_TREEVIEW      = YES  

至于为什么修改，暂时不用深究，我们后期再讨论

现在我们就可以输出文档了，在命令行（powershell)下输入

	doxygen .\Doxyfile

**注：**这个命令称为编译，下文直接用**编译**来代替表示这个指令

这时，Doxygen会从我们的代码中提取相应的文档，并生成HTML文件，进入GettingStart
文件夹内，双击打开HTML文件夹下的index.html文件

点击[这里]({{ site.url }}/assets/2014/03/21/html_output_1/index.html)查看

可以看到，当Doxygen对没有任何注释的代码，也可以生成对应的文档框架，不过，仅仅是框架而已，没有太大作用。Doxygen针对这种情况，专门设置了一个选项`EXTRACT_ALL`，默认情况下为`NO`状态，手工设置为YES后，Doxygen会尽可以的从代码中提取信息，这里我们将Doxyfile中的

	EXTRACT_ALL            = NO

改为

	EXTRACT_ALL            = YES

然后再次编译，前后输出结果，对比如下：

![对比结果]({{ site.url }}/images/2014/03/21/NakeCodeComp.png)

**注：**请点击[这里]({{ site.url }}/assets/2014/03/21/html_output_2/index.html)查看`EXTRACT_ALL = YES`的输出结果


## 总结

## 深入学习

## 参考资料

0.	[Doxygen官网](http://www.stack.nl/~dimitri/doxygen/index.html)

0.  [Doxygen支持的命令](http://www.stack.nl/~dimitri/doxygen/manual/commands.html)

0.	[IBM - 学习用doxygen生成源码文档](http://www.ibm.com/developerworks/cn/aix/library/au-learningdoxygen/)
