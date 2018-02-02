# Sencha Cmd概论

[Sencha Cmd](http://www.sencha.com/products/sencha-cmd/)是一个跨平台的命令行工具，
它在应用程序的整个生命周期中提供了许多自动化的任务，从生成一个新项目到将应用程序部署到生产环境。

## 认识Sencha Cmd

Sencha Cmd提供了一组强大的、高效的功能模块，这些功能模块可以协同工作，并可以与Sencha Ext JS和Sencha Touch框架交互工作。
Sencha Cmd提供以下功能:

- 代码生成工具:使用代码生成工具能够生成整个应用程序，并使用新的MVC组件扩展这些应用程序。
- JS编译器:一种框架感知的JavaScript编译器，它能够理解Sencha框架的语义，
并且可以从源代码生成最小内存占用的应用。编译器可以优化由Sencha框架提供的许多高级语义，
以减少应用程序的加载时间。
- Web服务器:提供一个轻量级Web服务器，用于从本地主机提供Web服务。
- 包管理系统:分布式包管理系统，可以方便地集成由他人创建或来自Sencha包存储库的包(比如Ext JS主题)。
- Sencha Web应用程序管理器集成——轻松地将新的应用程序版本发布到Sencha Web Application Manager服务器。
- 工作空间管理:协助在多个应用程序之间共享框架、包和自定义代码。
- 构建脚本:使用“before”和“after”扩展点为应用程序和包生成构建脚本，
这样您就可以定制构建过程以满足您的特定需求。
- Cordova/PhoneGap整合:原生打包将web应用程序转化为一种一流的移动应用程序，可以访问设备功能，
并可以在应用程序商店（App Stores）中分发。
- 图像捕捉:将CSS3特性(如border-rasius和线性渐变(linear-gradient))转换为遗留浏览器的精灵。
- 调优工具:强大的代码选择工具，用于根据页面间的通用代码和分区共享代码对应用程序的最终构建中包含的内容进行调优，
使用高级设置操作来根据您的需要准确地获得构建。
- 灵活的配置系统:为应用程序或工作区级或机器上的所有工作空间指定默认命令选项值。
- 日志记录:健壮的日志记录，以帮助您了解命令的内部工作原理，并有助于进行故障排除。
- 第三方软件:Sencha Touch和Ext JS 5以及更老的版本，Sencha Cmd包含了一个兼容的Compass和Sass的版本。
- 代码生成钩子:可以特定于一个页面，或者在工作空间中的所有页面共享，例如，在生成新模型时检查编码约定或规范。
## 兼容性

Sencha Cmd支持Sencha Ext JS版本4.1.1或更高版本与Sencha Touch版本2.1或更高。
Sencha Cmd的许多特性都需要框架支持，只有在当前版本或更新版本才可用。
一般来说，一些低级命令可以用于较老版本的Sencha框架或JavaScript。

如果你使用的是旧版本的Ext JS，你可以使用Sencha Cmd的构建命令来构建你的JSB文件。
换句话说，Sencha Cmd可以代替JSBuilder，生成JSB文件中描述的压缩构建。
Sencha Cmd不会更新您的JSB文件，就像被弃用的SDK工具v2所做的那样。

Sencha Touch 2.0和Sencha Ext JS 4.0要求已弃用的SDK工具v2，它不能用于以后的触摸或Ext JS版本。

## 系统设置

Sencha Cmd 6安装程序包含了构建Ext JS 6应用程序所需的所有软件，所以只需下载Sencha Cmd即可:

- [Sencha Cmd](http://www.sencha.com/products/sencha-cmd/)
### 使用旧框架

如果你使用的是较老版本的Ext JS或Sencha Touch，你需要在安装程序中选中“Compass扩展”选项:

![image](http://vcl-pictures.qiniudn.com/o_1c57u4hm1ali241fm91nsd5bnc.png)

你还需要安装Ruby来编译使用Sass的主题和应用程序。
Ruby根据不同操作系统安装不同版本:

- Windows:从rubyinstaller.org下载Ruby。获取".exe"文件并安装它。
- Mac OS:Ruby是预先安装的。您可以使用Ruby-v命令来测试是否安装了Ruby。
- Ubuntu:使用sudo-get install ruby2.0.0来下载Ruby。
如果你将使用Cordova或PhoneGap，可能会对这些工具有其他要求。
参见[与Cordova或PhoneGap的集成](#)。

安装程序将在PATH环境变量中添加一条路径指向运行目录。

### 验证安装
为了验证Sencha Cmd是否正常工作，打开一个命令行窗口，将目录更改为您的应用程序所在目录，并输入`sencha`命令。

你应该看到这样的输出:

	Sencha Cmd v6.0.0.202
	...

所显示的版本号应该与刚刚安装的版本号匹配。

## 静默安装Sencha Cmd

有些用户可能希望无GUI安装Sencha Cmd。
如果您需要一个只有clia的安装过程，只需从您的命令行工具中运行以下命令: