---
layout: post
title: "Doxygen 10 分钟入门教程"
date: 2014-03-21 22:02:07 +0800
comments: true
categories: [教程，Doxygen]
---

## 综述

本文试图在10分钟内，帮助您了解文档生成工具Doxygen的基本概念，并熟悉Doxygen的使用规则，同时给予深入学习的一些方向性建议。

因为本文仅仅是帮助初学者入门，所以选例方面较为简单，缺乏广度和深度。后期会编写一些深度学习Doxygen的文章，敬请关注


## Doxygen是什么东西？

Doxygen是一款文档生成工具，它可以从代码中提取出相应的文档，并组织，输出成各种漂亮的文档（如HTML，PDF，RTF等）

有了Doxygen工具，程序员便可以在写代码的时候，直接内嵌文档，再也不需要为某个功能代码单独写文档，从而最大程度的保持了文档和代码的统一性

另外，Doxygen 1.8.x版本中增加对markdown的支持，也支持内嵌部分HTML标签，从而极大的简化了文档编写难度，甚至，您可以用Doxygen生成一个静态的网站。

目前Doxygen支持C/C++，Objective-C, C#，PHP等语言，支持多平台(Mac OS, Linux, Windows)，更多信息，请参考[Doxygen官方介绍](http://www.stack.nl/~dimitri/doxygen/index.html)


<!-- more -->


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

### 第一次尝试
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

### 最终效果展示

接下来，我们为代码中增加对应的描述信息

Doxygen制定了一套注释规范，在保证正确输出文档的同时，也兼顾了良好的可读性。对编程人员来说，只需在编写注释的时候，稍微注意格式，即可生成非常优秀的文档，额外增加的工作量可忽略不计。

下面就是某个函数的Doxygen注释

{% codeblock lang:c %}

//*****************************************************************************
//
//! \brief Write one byte to special register
//!
//! This function is to write one byte to LIS302DL register,one byte will be
//! writen in appointed address.
//!
//! \param RegAddr specifies the target register address.
//! \param Data is the data written to target register.
//!
//! \return Indicate the status of operation which can be one of the following
//! value \b SUCCESS or  \b FAILURE .
//!
//! \note This function is used by internal, user MUST NOT call it in your 
//!  Application.
//
//*****************************************************************************
static Result _I2CRegWriteByte(uint8_t RegAddr, uint8_t Data)
{
    Result retv = SUCCESS;

    // Begin to I2C Transfer
    // first send START signal to control I2C bus
    // then send 7-bit address and R/W bit to I2C salve
    // at last send target register address
    retv = xI2CMasterWriteS1(LIS302DL_PIN_I2C_PORT, LIS302DL_I2C_ADDR,
            RegAddr, I2C_TRAN_NOT_END);
    if(retv != SUCCESS)
    {
        return (FAILURE);
    }

    // Send the wanted data to I2C bus
    // then Send STOP signal to release I2C bus
    retv = xI2CMasterWriteS2(LIS302DL_PIN_I2C_PORT, Data, I2C_TRAN_END);
    if(retv != SUCCESS)
    {
        return (FAILURE);
    }

    return(SUCCESS);
}

{% endcodeblock %}


我们可以看到，程序代码可读性非常好，即便没有生成单独的文档，任何具有一定英文基础的同学都可以
轻松的了解到函数的用法和入口参数，注意事项等信息。

接下来，我们采用Doxygen语法为main.c dev.c dev.h添加注释信息，完成后的效果如下所示：

{% include_code lang:c 2014/03/21/FinalSourceCode/main.c %}
{% include_code lang:c 2014/03/21/FinalSourceCode/dev.c %}
{% include_code lang:c 2014/03/21/FinalSourceCode/dev.h %}

用Doxygen编译后，生成的HTML文档如下所示：

![最终效果]({{ site.url }}/images/2014/03/21/Doxygen_Final_Demo.jpg)

**注：**请点击[这里]({{ site.url }}/assets/2014/03/21/html_output_final/index.html)查看详细的输出文档。

Doxygen把代码中的文件，例子，函数，宏等信息提取出来，一目了然

这里要说一下，如果没有Doxygen，我们该怎么做呢？

很多人会用SourceInsight这个代码阅读工具来辅助阅读，功能非常强大，但你电脑上必须装SI，否则没法阅读，这就限制了使用范围，从这里可以看到Doxygen的优势所在：提取代码文档，并将所有部分链接起来，形成统一整体

也许还有人会说，SI也可以生成继承图，调用图等图标，Doxygen可以吗？完全可以的，借助于Dot工具，Doxygen可以生成各种关系图表

**注：**Doxygen配合Dot工具生成图表以及Dot工具的用法，后期专门写一篇文章来介绍

### 一步一步跟我学Doxygen 

现在让我们开始从0开始，一步步为代码添加注释信息，最终生成上面所看到的效果

首先为函数添加注释信息，这是必须要做的。这里有个选择性问题，添加到哪里呢？.c文件？.h文件？

一般来说：

*	.h文件代表模块对外的接口最小信息，面向模块使用者
*	.c文件代表模块的实现代码，面向的是开发者

在实际编程中，事先约定各个模块间的接口，然后将不同的模块分配给不同的开发者，于此同时，测试人员根据接口要求，编写测试代码，这就完全保证了并发编程和白盒测试要求。

这里我们可以看到，文档主要是用来描述接口信息的，所以，我对代码的注释规定如下：

*	模块对外接口，仅在.h中提供注释信息
*	模块内部辅助函数，全部用static设为私有函数，同时仅在.c中保留注释信息

当然，您也可以同时为.c .h的接口函数编写两份完全一样的注释信息，但这么做，您会同时维护两份信息，出错的概率会更大些。

确定了注释位置，下一步考虑一个函数需要哪些信息

一般来说，需要函数功能，入口参数，返回值，注意事项，某些时候还需要说明上下文环境，从而保证函数能正确执行

比如这个函数

	extern int Dev_PrintInt(int number); 

它的功能就是打印一个整形数据，传入参数为整数，返回的是成功打印的数据长度（字节为单位），同时呢，我们在调用这个函数之前，必须要先初始化Dev设备

ok，这就是所有接口信息，稍微规范一下，就变成了下面的样子

	// 函数功能：打印整数
	// 入口参数：number为一个整数类型
	// 返回结构：返回的是成功打印的数据长度（字节为单位）
	// 注意事项：
	//          1：在调用本函数前，请确保已经调用Dev_Init初始化设备
	//          2：请注意函数返回值，如果该值为0，则说明函数执行失败

	extern int Dev_PrintInt(int number); 

用英文来书写呢，则变成下面的样子

	//***************************************************************************************
	//
	// brief  : Print Int number to terimal device.
	//
	// param  : number is the data you want to print.
	// retval : the number of print information, in bytes. return zero indicate print error !
	//
	// Note:
	//      * Be sure you have called \ref Dev_Init function before call this fuction.
	//      * Remember to check return value.
	//
	//***************************************************************************************
	extern int Dev_PrintInt(int number);

注释信息写完了，一般来说，函数能达到这种信息程度就ok了，但既然要生成文档，就不得不考虑一个问题

如果你是Doxygen作者，怎么从上面的注释里面提取信息呢，信息那么多，有`*`号，有各种文字信息。

你可以将所有的注释信息都输出出来，但这么做，等于没有分类整理，同时也包含了杂乱信息，比如一排`*`

另外一个解决方法是：设置某些特殊字符，比如`function`表示，一旦检测到这个特殊标记，则认为是接下来
的一行是函数功能描述。但这么做，万一用户的注释里面出现很多个function，你怎么识别哪个是普通文本，
哪个是特殊标记？

也许你会说了，可以采用$FUNCTION$这种形式啊，恩，这么做是可行的，可以确保识别出来特殊标记

接下来，还有一个问题，我们上面的注释中，有很多`*`号，仅仅起到美观和格式化的作用，当然不希望在
输出文档中显示这些东西，问题是你怎么识别这些符号，并不显示呢？也许你会说，可以强制规定注释的
格式，不让用户在代码中写很多`*`，ok，假设用户同意这么做。那接下来呢，如果我希望在代码中写某些话
，但是不希望输出到文档中，比如“祝某党长命百岁，领导是2B”等等，你又该怎么做呢？

正向思考遇到问题时，不妨反向考虑，这是谁的问题：是我设计思路的问题还是用户用法的问题？

困难重重，肯定是设计思路的问题

如果设计一个标记符，将普通注释和要生成的文档注释区分开来，就能解决问题了。

Doxygen的用法，说白了，就是为了解决上面提到的两个问题：
	
	怎么区分普通注释和输出注释  
	怎么在输出注释里面，识别特殊标记和普通文本  

ok，讲到这里，基本把Doxygen的机制给解释清楚了，如果您还不理解，最简单的方法就是把你假设为Doxygen
作者，重新推演一遍。

下面咱们看看Doxygen怎么解决这两个问题的

**区分普通注释和特殊注释**

对于C/C++语言来说，注释形式有两种

	//
	/* */

Doxygen通过在这里增加`*`，`/`，`!`来作为特殊标记，比如

对于`/* */`这种注释来说，正常注释为

	/*
	 * 正常注释
	 */

Doxygen在注释第一个`*`后，设置`*`或`!`作为标志，如果检测到有这些，
就将接下来的注释作为导出文档来解释

	/**
	 * 要输出成文档的注释
	 */

	 或者

	/*!
	 * 要输出成文档的注释
	 */

同时，中间的`*`号可以省略，像这样

	/**
	   要输出成文档的注释
	 */

	 或者

	/*!
	   要输出成文档的注释
	 */

对于`//`这种类型的注释，Doxygen在第二个`/`后，增加`!`或`/`作为区分标志，如果检测到有这些，
就将接下来的注释作为导出文档来解释

	/// 要输出成文档的注释

	或者

	//! 要输出成文档的注释

对于这种呢，有一个潜在的问题，很多时候，我们需要在把注释放到后面，比如下面这种

	#define DEV_ON      ((int)(1))      //! Simple device is power on.
	#define DEV_OFF     ((int)(0))      //! Simple device is power off.

如果真要这么写的话，Doxygen会把`//! Simple device is power on.`当做`DEV_OFF`的注释，这
当然不是我们所希望的! 怎么办呢，只好再加一个特殊标记了，Doxygen针对这种情况，需要在`!`后
再增加一个`<`标志符，如果检测到这个，则认为这个注释是为前面代码准备的，所以，上面的注释应该
这么写

	#define DEV_ON      ((int)(1))      //!< Simple device is power on.
	#define DEV_OFF     ((int)(0))      //!< Simple device is power off.

做到这里，Doxygen就可以正确区分普通注释和特殊注释了，^_^

更详细的信息，请参考[Doxygen注释规范](http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html)


**区分特殊标记符和普通文本**

ok，现在可以识别出了普通注释和特殊注释，接下来，Doxygen是怎么从特殊注释里面提取信息的呢

比如

![最终效果]({{ site.url }}/images/2014/03/21/Doxygen_Final_Demo.jpg)

注意左边导航栏，Doxygen怎么识别出这是一个函数/宏呢？答案还是采用特殊标记

**注：**提到特殊标记，其实吧，编程语言非常常用，比如HTML就是典型的markup语言，一堆一堆的括号，看着就头疼

Doxygen采用`\`和`@`作为特殊标记符，当在特殊注释里面检测到了特殊标记符，则接下来检测紧跟单词是不是Doxygen
事先规定好的，如果是，则将按照特定的规则来解释紧跟着的注释；如果不是呢，则将`\`和`@`解释为普通文本，聪明吧

可能有点拗口，下面给你个例子

	//***************************************************************************************
	//
	//! \brief  Print Int number to terimal device.
	//!
	//! \param  [in] number is the data you want to print.
	//! \retval the number of print information, in bytes. return zero indicate print error !.
	//!
	//! \note
	//! * Be sure you have called \ref Dev_Init function before call this fuction.
	//! * Remember to check return value.
	//
	//***************************************************************************************
	extern int Dev_PrintInt(int number);

看到了吧，这里的`\brief`和`\param`都是特殊符号，表示简要描述和参数。万一你小手一抖，把`\param`
写成了`\parame`，那就悲剧了，因为Doxygen不认识`parame`，所以它会把这句话当做是普通文本来处理

其实，上面的`\`换成`@`也是ok的，如下所示

	//***************************************************************************************
	//
	//! @brief  Print Int number to terimal device.
	//!
	//! @param  [in] number is the data you want to print.
	//! @retval the number of print information, in bytes. return zero indicate print error !.
	//!
	//! @note
	//! * Be sure you have called \ref Dev_Init function before call this fuction.
	//! * Remember to check return value.
	//
	//***************************************************************************************
	extern int Dev_PrintInt(int number);

相信某些玩过ARM芯片的，对这类注释非常熟悉，官方库都是采用Doxygen语法规则注释的，我也写过一套LPC17xx
的库 https://github.com/cedar-renjun/Cox_LPC17xx

那么，我们怎么知道Doxygen认识哪些符号呢，参考Doxygen自带手册啊，[Special Commands](http://www.stack.nl/~dimitri/doxygen/manual/commands.html)章节

授之于鱼，不如授之于渔，该说的多介绍完了，至于具体的指令含义和用法，自己慢慢看手册呗

注：点击[这里]({{ site.url }}/assets/2014/03/21/DoxygenGettingStart.zip)下载GettingStart的压缩包，包含源码，HTML输出文档，配置脚本
如果在看脚本的时候遇到问题，请参考[这里](http://www.stack.nl/~dimitri/doxygen/manual/config.html)


### 深入研究

不好意思，文章有点长，估计过了10分钟，so bad，o(╯□╰)o

根据20/80原则，一个软件，常用的仅仅是20%的功能，其余80%功能是极少用到的

上面仅仅介绍了Doxygen的设计思想和学习范例，抓住了纲领，相信您通过研究代码和帮助手册，很快就能学会使用Doxygen

如果您想深入研究Doxygen，比如markdown和代码无缝交互，插入HTML代码，定制HTML网页，CSS等功能，
请参考Doxygen帮助手册，里面有详细说明，如果学习过程中，遇到问题，欢迎和我交流，互相学习

------

{% blockquote Cedar, QQ:819280802 %}
Do one thing and do it well
{% endblockquote %}

------

## 参考资料

0.	[Doxygen官网](http://www.stack.nl/~dimitri/doxygen/index.html)

0.  [Doxygen支持的命令](http://www.stack.nl/~dimitri/doxygen/manual/commands.html)

0.	[IBM - 学习用doxygen生成源码文档](http://www.ibm.com/developerworks/cn/aix/library/au-learningdoxygen/)


