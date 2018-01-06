# Cmd 6.2的新特性

Cmd 6.2包含一些新的实用工具，使您可以更轻松地在Ext JS应用程序中使用Cmd。
大多数这些新工具都集中在工作区定制和非Cmd应用程序和工作空间上。

让我们来看看其中的一些新特性。

## 应用程序初始化

`app init`初始化当前目录为Sencha Cmd应用目录。
您可以从一个完全空的目录开始，发出以下命令之一，最后生成一个完全结构化的Ext js/cmd“hello world”应用程序。

`sencha app init --ext=/path/to/extjs/ MyApp --modern`

`sencha app init --ext=/path/to/extjs/ MyApp --classic`

`sencha app init --ext=/path/to/extjs/ MyApp --universal`

注意:您的应用程序在构建之前不能使用：

`sencha app build`

## app install

`app install`确保了一个不完整的Sencha Cmd应用程序在一个可运行的状态。
例如，您可以使用一个非常简单的应用程序文件夹，将它放到您的机器上，然后运行这个命令来创建一个完整的Cmd应用基础架构。

您可以查看这个github仓库的例子（https://github.com/sencha-extjs-examples/QuickStart/），以便查看该命令的示例：

`sencha app install --frameworks=/path/to/extjs`
“框架”是Cmd 6.1中引入的特性(见下文)。
它允许您指向包含多个Ext JS版本的文件夹。
正如您在上面的“代码仓库”中看到的，Ext JS版本是在workspace.json中指定的。
框架标记将尝试在您所指向的文件夹中找到与所需版本最接近的匹配。


## workspace init

`workspace init`确保当前目录是一个Sencha Cmd工作空间或是工作空间的一部分。

`sencha workspace init`

## workspace install
   
   `workspace install`确保了一个不完整的Sencha Cmd工作区处于可运行状态。
   例如，您可以使用一个非常简单的工作空间文件夹，将它放到您的机器上，然后运行这个命令来创建一个完整的Cmd基础架构。
   
   `sencha workspace install --frameworks=/path/to/extjs`
   
## 工作区清理命令
  
  `workspace cleanup`用于将不再出现在工作区中的应用程序入口从`workspace.json`中删除 。
  
 `sencha workspace cleanup`  
 
# Sencha Cmd 6.1中的新特性
  
## 框架和工作区管理
  
  当项目扩展到在单个工作空间中对多个应用程序进行操作的时候，不同的框架版本可能会存在于同一个工作空间下。
  在Sencha Cmd 6.1之前，工作区只能配置一个Ext JS版本(以及Sencha Touch的另一个版本)，这将迫使所有应用程序同时进行升级。
  
  从Sencha Cmd 6.1开始，“workspace.json”文件可以配置多个框架。
  
   "frameworks": {
    
        "ext62": "ext",
        
        "ext602": "ext-6.0.2"
        
    }
“frameworks”属性下的每个条目对应于当前工作空间中的一个目录，属性名是在“app.json”中使用的框架的键值。

 `app.json`中的“framework”属性映射到其中一个条目:
 
 "framework": "ext62"
 
 要向工作空间添加一个新的框架，可以使用“framework add”命令:
 
 `sencha framework add /path/to/ext`
 
 这个命令将根据它的版本号(例如:Ext JS 6.2.0的“ext62”或Ext JS 6.0.2的“ext602”等)来决定所提供的框架的适当键。
 
 “framework add”命令还提供了一些额外的开关配置，允许您控制框架条目的所有方面。
 
 例如:

`sencha framework add -key extFiveO -source /path/to/ext-5.0.0 -path ext50` 

此命令会在“workspace.json”文件的“frameworks”属性中添加一个类似如下的入口：

"frameworks": {
    "extFiveO": "ext50"
}

Cmd还将把重要的框架文件从`/path/to/ext-5.0.0`复制到工作区中的子文件夹ext50(用-source和-path配置项来配置)。注意，框架的键要以“ext”或“touch”开始。

在添加了一个框架之后，您可以通过使用它的框架键作为“generate app”命令的配置项来生成一个应用程序。
例如，要使用我们刚刚添加的“extSixTwo”框架来生成一个应用程序，您需要像这样调用`generate apps`命令：

`sencha generate app -extFiveO AppName path/to/app`

在任何时候，您都可以使用“framework list”命令在当前工作空间中列出框架，该命令将显示与下面类似的输出:

C:\Workspace> sencha framework list

Workspace located at: C:\Workspace

Available frameworks

- ext602: (Commercial)
  - Framework: Ext JS
  - Version: 6.0.2
  - Source: ext
  - Used by 1 application:
     - MyApp (./MyApp)

- ext62: (Commercial)
  - Framework: Ext JS
  - Version: 6.2.0.589
  - Source: ext62
  - Not in use by any of the applications listed on workspace.json.

- extFiveO: (Commercial)
  - Framework: Ext JS
  - Version: 5.0.0
  - Source: ext50
  - Used by 1 application:
     - SampleApp (path/to/app)
注意，这个命令会让你知道这个框架是否被“workspace.json”的“apps”属性所列出的任何应用程序所使用。

可以使用“framework remove”命令来删除一个框架。
例如:

`sencha framework remove extFiveO`

只有当框架没有被任何应用程序使用时，上面的命令才能正常进行。
如果您想删除一个框架条目，不管它的使用情况如何，您可以使用`--force`开关:

`sencha framework remove --force extFiveO`

要使用特定的框架升级所有应用程序，您可以使用“framework upgrade”命令。
例如，要升级一个具有“ext”键到最新框架版本(位于/path/to/ext)的框架，您需要执行以下命令:

`sencha framework upgrade ext /path/to/ext`

这将升级框架文件，并为使用它的每个应用程序触发一个“sencha app upgrade”命令。

当然，你也可以使用“sencha app upgrade /path/to/ext”命令升级单个应用程序。
您甚至可以升级应用程序的框架到任何已经在工作空间中的框架:

`sencha app upgrade ../ext62`

“workspace upgrade”命令提供了一种方法，可以在您的应用程序和包中升级整个工作空间的与Cmd相关的文件。

`sencha workspace upgrade`

这个命令不会更新框架版本或任何其他信息，只升级在每个应用程序和包中使用的与Cmd相关的文件。

#Sencha Cmd 6的新特性
 
 ## 新的安装程序
 
 使用Sencha Cmd 6，我们已经更改了安装程序，以包含运行Sencha Cmd所需的Java运行时环境(JRE)。
 使用这些安装程序意味着您不需要另外下载和安装Java。
 结合了Ruby的依赖(见下文)的移除，Sencha Cmd除了操作系统之外没有任何先决条件（软件依赖）。
 我们仍会提供不包含JRE的安装程序。
 
 ## 非管理员权限
 
 新的安装程序不需要UAC权限(对于Windows)或root特权(对于Mac和Linux)。
 
 ## Ruby非依赖
 
 随着Fashion的引入(见下)，对于Ext JS 6应用程序和主题来说，Sencha Cmd不再依赖于Ruby。
 虽然这提供了许多优点(比如实时更新)，但是任何依赖于Ruby代码的Compass功能都将不可用。
 然而，Compass中的Sass代码包含在Fashion中，因此这不会影响到所有的Compass功能。
 
 ##Compass(旧的框架)
 
 如果您将使用Sencha Cmd 6和旧的框架(Ext JS 4和5或Sencha Touch 2)，您仍然需要Ruby来编译Sass代码。
 安装程序可以被要求安装所需版本的Compass(和Sass)来支持这些框架。
 默认情况下禁用此设置，但是可以在Sencha的安装程序引导界面的Compass extension选择界面中启用。

 ## Fashion
   
   正如上面所提到的，Sencha Cmd 6包括“Fashion”模块，Sencha的新Sass派生语言编译器。
   Sencha Cmd使用Fashion来为Ext JS 6应用程序和主题构建样式规则( .scss文件)。
   在使用sencha app watch时，你还可以使用实时更新来保持你的应用程序的CSS与SCSS同步，而无需重新加载页面。
   想了解更多关于Fashion的知识，请参阅Fashion指南。
   
 ## 工作空间的改进
   
  ###  包目录
   
   为了方便使用包来进行代码共享，Sencha Cmd 6为包引入了一个额外的文件夹。
   这允许您生成的包在一个地方，而引用包(需要下载并提取)却在另一个地方。

/                   # The workspace root folder

    packages/       # The former location for all packages
    
        local/      # New location for generated packages
        
        remote/     # New location for downloaded/extracted packages
        
包的位置现在可以在workspace.json中配置。
“/packages/remote”文件夹可以通过源代码控制(例如在.gitignore文件中）被配置为“忽略”。因为该文件夹中的所有包都是从包存储库中下载的，并且可以在需要时再重新下载。

### workspace.json

使用Sencha Cmd 6，workspace.json文件将被自动添加到工作区的根文件夹中。
这个文件应该在源代码控制中被跟踪，因为它将在以后的开发工具使用中起作用。
不过，对于初学者来说，它可以让您配置上面提到的包路径。

## Microloader

Sencha Cmd 6为微加载器提供了一个主要版本的更新。
Microloader是由Sencha Touch首先引入的应用程序加载器，随后为Ext JS 5应用程序对应的Sencha Cmd 5中引入了此功能模块。

使用Sencha Cmd 6，最新的微加载器现在可以用于Ext JS 6应用程序，并提供对localStorage缓存的支持。
这类似于Sencha Touch的微加载器中的操作，但是有一些改进。

- 可以在app.json中禁用缓存。

- 微加载器的内容只会被微加载器删除。

- 只有当前版本的应用程序才能保存在本地存储中。       