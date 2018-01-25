#Sencha Cmd 5升级指南

本指南旨在帮助开发人员使用Sencha Cmd从ExtJS 4.1.1 a+升级到ExtJS 5.0.x。
尽管在这个版本中有一些重要的变化，但是我们已经尝试使升级过程尽可能的没有痛苦。
在深入讨论之前，值得一提的是，使用此指南需要一些前提。

* 您的应用程序是使用Ext JS 4.1.1 a+构建的
* 您的应用程序是使用我们推荐的MVC模式布局的
* 你的应用目前正在与Sencha Cmd一起构建
我们认识到，并不是所有的客户都有能力运行最新版本的Ext JS和Sencha Cmd。
时间限制、最后期限和支持许可通常决定了在企业中使用Ext JS的版本。

花时间升级到Ext JS 4.x和Cmd 4.x的最新版本将会使升级到我们的5.x分支更为顺畅。
使用最新的版本可以确保您可以使用修复过bug的版本并避免大量的API更改。

## 迁移过程

### 在开始之前
#### 清除状态
迁移过程的第一步是确保在您的源代码控制系统中没有未提交的更改。
不建议在未提交更改时启动升级。
这将让你更容易地回顾Sencha Cmd的变化，这样你就可以确定你所做的任何更改未受影响。
安装合并工具(推荐)	
在升级过程中，Sencha Cmd可能需要对一些您可能已经编辑过的文件进行更改。
就像任何这样的场景一样，Sencha Cmd需要更新和您可能已经编辑了的代码相同，由此产生了合并冲突。好消息是，就像版本控制一样，有一些工具可以帮助解决这些合并冲突。
Sencha Cmd可以使用任何可以从命令行运行的可视化合并工具(这几乎是所有的)。
这一步是可选的，但高度推荐，因为它将使处理升级变得更加简单。

下面是一些流行的选择(一些免费的，一些商业的):

 - [p4merge](http://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools)
 - [SourceGear](http://www.sourcegear.com/diffmerge/index.html)
 - [kdiff3](http://sourceforge.net/projects/kdiff3/files/kdiff3/)
 - [Syntevo SmartSynchronize 3](http://www.syntevo.com/smartsynchronize/)
 - [TortoiseMerge](http://tortoisesvn.net/) - (part of TortoiseSVN)
 - [AraxisMerge](http://www.araxis.com/merge/)
在下一步中，我们将配置Sencha Cmd使用您的首选合并工具。

### 升级Sencha Cmd
接下来，您需要获取Sencha Cmd 5的最新版本。

你可以下载Sencha Cmd 5或者查看下面链接中的已知问题的发行说明:

[Release Notes](http://www.sencha.com/products/sencha-cmd/)

如果你已经在使用Sencha Cmd 5，你可以从我们的下载页面获得最新的测试版或者运行如下命令:

`sencha upgrade --beta`

安装Sencha Cmd并重新启动您的终端。

注意:如果你安装了以前的Cmd，它不会取代它，但是在运行Cmd软件时它会优先运行现行版本。
有关详细信息,请参阅上面。

#### 配置合并工具

如果你选择了一个合并工具，我们需要配置Sencha Cmd来使用这个合并工具。
要这样做，您需要向配置文件中添加两个属性:

`cmd.merge.tool`

`cmd.merge.tool.args`

另外，这两个属性可以以独立于版本的方式设置，这样您的配置参数将适用于Sencha Cmd的所有版本。
详情请见“sencha.cfg” 文件的尾部。
### App Upgrade
我们已经准备好开始升级了。
只需在应用程序的根文件夹中运行该命令，就可以完成升级操作:

`sencha app upgrade -ext`

您应该会看到少量的绿色文本，让您知道您的应用程序已经成功地升级了。

在大多数情况下，您将需要在应用程序的基础上执行构建，然后应用才能像预期的那样运行。
可以通过执行以下命令来完成:

`sencha app build`

### Microloader（微加载器）

在Sencha Cmd之前的发行版中，Ext JS应用程序与Sencha Touch应用程序的执行非常不同。
这主要是由于Sencha Touch“微加载器”造成的。
在Sencha Cmd 4中，microloader从Sencha Touch移动到Sencha Cmd，但是在那个时候，
微加载器不能在所有Ext JS支持的浏览器上运行。

使用Sencha Cmd 5和Ext JS 5，这种限制不再会有。
这意味着Ext JS 5应用程序现在可以使用与Sencha Touch相同的处理过程。
这为一些非常有用的特性打开了大门。

#### app.json

为了利用这些特性，我们首先需要将标记页面从“x-compile”注释表单转换为微加载器脚本标记并关联“app.json”文件。
在大多数情况下，这将是一个简单的问题，将脚本和CSS引用移到您的“app.json”文件中即可。
以前版本的Sencha Cmd生成的默认标记如下:

    <!-- <x-compile> -->
         <!-- <x-bootstrap> -->
             <link rel="stylesheet" href="bootstrap.css">
             <script src="../ext/ext-dev.js"></script>
             <script src="bootstrap.js"></script>
         <!-- </x-bootstrap> -->
         <script src="app.js"></script>
    <!-- </x-compile> -->
   
上面的标记应该更改为:

    <script id="microloader" type="text/javascript" src="bootstrap.js"></script>
    
"bootstrap.js"文件是由Sencha Cmd生成的，用于加载微加载器。这个文件只在开发模式中使用。
上面的脚本标记被替换为构建的一部分，就像前面所替换的“x-compile”块。

等同的“app.json”配置是这样的:    

    {
        "framework": "ext",
        "css": [
            {
                "path": "bootstrap.css",
                "bootstrap": true
            }
        ],
        "js": [
            {
                "path": "app.js",
                "bundle": true
            }
        ]
    }
 
 您可能会注意到，在“ext-dev”文件中没有js入口。
 这是因为框架包(“ext”)是Sencha Cmd 5内部识别的东西，它会自动地在您的工作空间中找到它。
 
 您可以在“app.json”文件中探索到许多其他选项。
  这个文件使您可以轻松地访问包(使用“requires”)，并且允许您将选项传递到运行时。
 这是因为您在“app.json”文件中的配置内容，在运行时中将被加载为Ext.manifest。
 译者注：在运行环境里，您可以通过Ext.Manifest来访问app.json中配置的内容。例如：您可以为测试环境和生产环境
 配置不同的API域名。
 
 ### 构建属性
 
 在以前的Sencha Cmd的版本中，必须在 ".sencha /app”中的多个文件里指定许多属性。
 在Sencha Cmd 5中，可以在许多情况下使用app.json来代替。
 通过添加app.并将属性名与“.”连接在一起，可以将app.json的内容转换到构建属性中。
 例如，在app.json中的这两个属性:
 
     {
         "theme": "ext-theme-crisp",
         "sass": {
             "namespace": "MyApp"
         }
     }
 在“……sencha/app/sencha.cfg”中替换这些属性：
 
     app.theme=ext-theme-crisp
     app.sass.namespace=MyApp
     
还有许多其他的构建属性不能在app.json中指定(通常是因为它们不是以app.作为前缀)，
但是建议您只保留那些在“app.json”中可以控制的属性。
有关您可以在app.json中控制的内容的详细信息，请参考该文件的注释。
对于其他属性,参看“.sencha /app/defaults.properties”文件。 

### 监视

最后一步是在你的应用程序根文件夹中构建应用：`sencha app build`，或者监视应用：`sencha app watch`。
这两种方法都将更新您的应用程序并准备使用。
使用watch，您现在可以在自己本地主机上的web服务器的或在http://localhost:1841上查看应用程序，
这是我们自动为您定制的服务器。这个web服务器是app watch默认的服务器。
在以前的版本中，您必须分别运行sencha web start来使用Sencha Cmd web服务器。
您可以使用这些属性调整web服务器配置(显示它们的默认值):

    build.web.port=1841
    build.web.root=${workspace.dir}

在这个版本中，我们优化watch功能，使它有了更快的启动时间，
更好地与Compass共享工作，同时避免了不必要的Sass编译。

## Cordova / PhoneGap

如果你正在使用Sencha Cmd对Cordova或PhoneGap的进行集成，那么升级过程将包括一些额外的步骤。

### 构建配置文件

Sencha Cmd 5增加了在您的app.json文件中定义多个构建的支持。
这种支持对于原生打包来说是比较理想的。
对于Cordova或PhoneGap支持的应用程序，“sencha app upgrade”命令将在你的app.json文件的顶部
添加一些默认的构建配置文件。
这些构建配置文件提供了来自Sencha Cmd以前版本的一致的命令接口，并展示了你想实现的更多功能。

`app upgrade`命令为Cordova应用程序添加的构建文件看起来是这样的:

    "builds": {
        "web": {
            "default": true
        },
    
        "native": {
            "packager": "cordova",
            "cordova" : {
                "config": {
                    "platforms": "ios",
                    ...
                }
            }
        }
    },
    
这些构建配置文件确保所有的“sencha app build”命令的变化都与以前的版本相同。
但是，您会注意到，“native”现在是一个构建配置文件，而不是一个环境(比如“测试”或“生产”)。
这意味着你现在可以执行“sencha app build native testing”，这在以前是不可能的。

### 本地属性文件

Sencha Cmd所产生的新项目不再使用“cordova.local.properties”或“phonegap.local.properties”。
如果您的应用程序还有这些文件，它们也会继续正常运行。
新的项目应该向标准的“local.properties”文件添加个人属性(例如PhoneGap构建凭证)。

### 总结

就这样!您的应用程序升级到最新的Ext JS和Sencha Cmd。
在测试完之后，您可以提交更改(其中很多都在“.sencha”文件夹)。

## 其他注意事项

### 日志输出

如果你对Sencha Cmd很熟悉，你可能会注意到，Sencha Cmd的控制台输出已经显著减少了。
如果您更喜欢原始的日志记录级别，您可以通过向您的命令添加-info来重新启用它。
例如:

`sencha -info app watch`

如果你想要更少的日志输出，你可以用-quiet参数来运行命令:

`sencha -quiet app watch`

## 更多的信息

有关升级过程的更多信息，请查看以下指南:
- [Charts - Upgrade Guide](file:///E:/Software/cmd-651-docs/cmd/extjs/5.0/whats_new/5.0/charts_upgrade_guide.html)
- [Ext JS 5 - Upgrade Guide](file:///E:/Software/cmd-651-docs/cmd/extjs/5.0/whats_new/5.0/extjs_upgrade_guide.html)