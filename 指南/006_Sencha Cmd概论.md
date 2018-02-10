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

### 静默安装Sencha Cmd

有些用户可能希望无GUI安装Sencha Cmd。如果您只想通过命令行来安装，
那么您只需在命令行工具中运行以下命令:
#### Mac OSX

`SenchaCmd-6.x.y.z-osx.app/Contents/MacOS/JavaApplicationStub -q`

#### Linux
`SenchaCmd-6.x.y.z-linux(-i386|-amd64).sh -q`

#### Windows

`SenchaCmd-6.x.y.z-windows-(32|64)bit.exe -q`

这将静默安装Sencha Cmd，而不需要运行GUI安装程序。
#### 更改安装路径
    
如果需要更改安装路径，可以使用“dir”标志指定它。例如:

`sudo SenchaCmd-6.x.y.z-linux(-i386|-amd64).sh -q -dir "/opt"`

注意:您安装路径的选择可能需要修改权限。

## 升级Sencha Cmd

`sencha upgrade`命令可以让你升级sencha Cmd。

查看Sencha Cmd的更新:

`sencha upgrade --check`

如果没有`--check`选项，`sencha upgrade`命令下载并安装最新版本的程序，如果您还没有安装最新的版本 的话:

`sencha upgrade`

安装程序完成后，启动一个新的控制台或终端，查看您的PATH环境变量的更改。

由于Sencha Cmd的多个版本可以并行安装，您可以安全地尝试新版本，
并简单地卸载它们(或调整PATH变量中的路径或符号链接)，
以回到您以前的版本。不过，如果你使用`sencha app upgrade`升级了应用，
并且你把Sencha Cmd降级为老的版本，你可能需要“回滚”应用的版本。

### 静默升级Sencha Cmd

有些用户可能希望无GUI升级Sencha Cmd。如果您只想通过命令行来安装，
那么您只需在命令行工具中运行以下命令:

`sencha upgrade --unattended`

这将静默升级Sencha Cmd，而不需要运行GUI安装程序。

### Beta版本
如果你想查看beta版本，请使用:

`sencha upgrade --check --beta`
 
 安装最新的beta版本：

`sencha upgrade --beta`
 
 注意：当前版本可能是在“测试版”或“稳定通道”中。
也就是说，`sencha upgrade --beta`可能会安装一个测试版，它的发布日期会先于通过`sencha upgrade`安装的当前版本。

## 基本命令
   
Sencha Cmd的命令排序特性是：按类别(或模块)和命令排列:

`sencha [category] [command] [options...] [arguments...]`

使用`help`命令可以获取相关的帮助信息。

`sencha help [module] [action]`

例如，使用如下命令：

`sencha help`

显示当前版本和可用的顶级命令。例如:

    Sencha Cmd v6.0.0.202
    ...
    
    Options
      * --beta, -be - Enable beta package repositories
      * --cwd, -cw - Sets the directory from which commands should execute
      * --debug, -d - Sets log level to higher verbosity
      * --info, -i - Sets log level to default
      * --nologo, -n - Suppress the initial Sencha Cmd version display
      * --plain, -pl - enables plain logging output (no highlighting)
      * --quiet, -q - Sets log level to warnings and errors only
      * --sdk-path, -sd - The location of the SDK to use for non-app commands
      * --strict, -st - Treats warnings as errors, exiting with error if any warnings are present
      * --time, -ti - Display the execution time after executing all commands
    
    Categories
      * app - Perform various application build processes
      * compile - Compile sources to produce concatenated output and metadata
      * cordova - Quick init Support for Cordova
      * diag - Perform diagnostic operations on Sencha Cmd
      * fs - Utility commands to work with files
      * generate - Generates models, controllers, etc. or an entire application
      * manager - Commands for interacting with Sencha Web Application Manager.
      * manifest - Extract class metadata
      * package - Manages local and remote packages
      * phonegap - Quick init support for PhoneGap
      * repository - Manage local repository and remote repository connections
      * template - Commands for working with templates
      * theme - Commands for low-level operations on themes
      * web - Manages a simple HTTP file server
    
    Commands
      * ant - Invoke Ant with helpful properties back to Sencha Cmd
      * audit - Search from the current folder for Ext JS frameworks and report their license
      * build - Builds a project from a legacy JSB3 file.
      * config - Load a properties file or sets a configuration property
      * help - Displays help for commands
      * js - Executes arbitrary JavaScript file(s)
      * upgrade - Upgrades Sencha Cmd
      * which - Displays the path to the current version of Sencha Cmd
**注意：** 输出的具体内容取决于你安装的版本。
### 详细输出

在Sencha Cmd 5中，调试输出的信息量显著降低。您可以通过在命令中包含`-info`标志来重新启用详细输出。
例如:
`sencha -info app watch`
该命令将提供关于在初始化期间和文件系统更新期间后台进程发生的更多信息。

`-info`标志可以用于任何生成输出信息的Sencha命令。

## 当前目录
   
在许多情况下，Sencha Cmd要求你设置一个特定的当前目录。
或者，它可能只需要了解相关的Sencha SDK的详细信息。
SDK(或“框架”)可以由Sencha Cmd在一个已生成的应用程序文件夹中运行时自动确定，
或者从一个已提取的SDK文件夹中运行一些命令得到。

重点注意：对于以下命令，Sencha Cmd需要从生成的应用程序的根文件夹中运行。
如果不从应用程序的根文件夹运行，命令就会运行失败。

    * `sencha generate ...` (for commands other than `app`, `package` and `workspace`)
    * `sencha app ...`
这也适用于包。当运行诸如`sencha package build`这样的命令时，当前目录必须是所要打包的包文件夹。
## Sencha Cmd文档
   
   许多指导Sencha Cmd的指南是按照便于帮助您理解的顺序编排，并且建议您遵循这一顺序。
   提前进行跳转可能会导致混淆，因为高级向导通常是建立在对早期指南的内容进行理解的基础上。
   
   在每个指南的开头，都是关于该指南的预备知识的链接。
   此外，大多数指南以一组链接结束，以供进一步阅读。
## 高级知识技能
   
除了基础知识，Sencha Cmd还有很多其他的细节对我们很有帮助。
`help`命令是一个很好的参考，但是如果您想要遍历所有的亮点，
请参考[高级Sencha Cmd](http://docs.sencha.com/cmd/6.5.1/guides/advanced_cmd/cmd_advanced.html)。

## 故障排除
   
   下面是一些解决使用Sencha Cmd时遇到的常见问题的技巧。
   
### Java堆空间
   
   如果在执行Cmd命令后收到一个“java heap space”错误，您可能需要增加java分配的内存使用。
#### Mac & Linux
将正面的语句添加到`~/.bash_profile`文件中：

`export _JAVA_OPTIONS="-Xms1024m -Xmx2048m"`
#### Windows
你可以将这个变量添加到环境变量，或者你可以将以下内容添加到`startup:bat`:

    set _JAVA_OPTIONS="-Xms1024m -Xmx2048m"
### 命令未找到
如果运行`sencha`的结果,在OSX/linux是错误消息`sencha:command not found`或在Windows上是
`'sencha' is not recognized as an internal or external command, operable program or batch file`，
请遵循以下步骤:
- 关闭终端/命令提示窗口并打开一个新窗口。
- 确保正确安装Sencha Cmd:
    - 安装目录是存在的。默认情况下，安装路径是:
        - Windows:C:\Users\Me\bin\Sencha\Cmd\ { version }
        - Mac OS X:~/bin/Sencha/Cmd/ { version }
        - Linux:~/bin/Sencha/Cmd/{ version }
    - Sencha Cmd的安装路径被预先设置到PATH环境变量。
在终端上，在Windows上运行`echo %PATH%`，或者在Mac或Linux上运行`echo $PATH`。
Sencha Cmd目录应该在输出内容的一部分中显示。如果Sencha Cmd运行目录没有在输出中显示，
那么将它添加手动到您的PATH环境变量中。

### 无法找到Ruby
    
如果您看到一个与没有识别或发现“ruby”相关的错误，
那么很可能是因为ruby没有安装或者不在您的`PATH`环境变量中。
请参阅前面的[系统设置](http://docs.sencha.com/cmd/6.5.1/guides/intro_to_cmd.html#System_Setup)部分。

您在Ext JS 6中应该看不到此类消息，因为Ext JS 6使用了Fashion而不是Compass，而Fashion不需要Ruby。

### 错误的当前目录（Wrong Current Directory）
一个常见的错误是执行一个命令，要求当前目录是一个提取的SDK目录或一个应用程序目录，
但当前目录并不是。如果没有达到这个要求，Sencha Cmd将显示一个错误并退出。
注意：一个有效的应用程序目录是由Sencha Cmd生成的。
### 处理依赖关系时发生错误

`sencha app build`命令通过读取index.html和app.json文件并扫描所需的类来工作。
如果您的应用程序没有正确地声明它所需要的类，构建通常会完成，但是不会包含应用程序所需要的所有类。

为了确保您拥有所有指定的类，请始终使用启用调试器控制台(IE/Chrome中的“开发工具”、FireFox中的FireBug和Safari中的Web Inspector)，
并解决所有出现的警告和错误消息。

每当你看到这样的警告:

    [Ext.Loader] Synchronously loading 'Ext.foo.Bar'; consider adding 'Ext.foo.Bar'
    explicitly as a require of the corresponding class
你应该添加“Ext.foo.Bar"到名为`requires`的对应的依赖源的类属性中。
如果它是一个应用程序范围的依赖项，那么将它添加到`app.js`中的`Ext.application(...)语`句中的
`requires`数组属性。