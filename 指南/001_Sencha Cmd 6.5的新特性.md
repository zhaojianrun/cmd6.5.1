#Sencha Cmd 6.5.1的新特性
## 动态加载包的最小体积的构建
在Ext JS 6.5.0中，使用新的动态包加载器的应用程序的构建输出包含了所有的框架类。
这是因为动态加载的包是单独构建的，应用程序不知道它们的框架依赖关系。

对于Ext JS 6.5.1，动态包的类的引用可以传递给应用程序构建，并允许它只包含需要的类，如下所示:


"output": {
    "js": {
        "filter": "minimum"   
    }
}
备选的过滤器选项是“all”，其中包括所有来自“used”的包的代码，以及“使用”(6.5.0中的默认行为)，其中包括应用程序所需要的所有代码和它所使用的包的所有代码。

## 手动从构建中排除类
现在您可以手动地从构建中排除类名。这有助于删除Sencha Cmd自动依赖扫描器检测到的代码，但实际上并不是应用程序所使用的。例如:
"js": {
    "exclude": [
        "Ext.data.BufferedStore",
        ...
    ]
}

# 在Sencha Cmd 6.5中的新特性

## 简介

Sencha Cmd 6.5有几个主要的和次要的特性和增强，它们可以帮助您简化开发过程，使用最新的web技术优势，充分利用Ext JS 6.5的功能。

## ECMAScript 2015(或ES6)

使用Sencha Cmd 6.5，您可以使用箭头函数、let关键字、对象去结构化以及几乎所有的ES6中的酷炫新特性来编写代码。
Sencha Cmd将编译你的代码让它到处都可以运行。
这种翻译被称为“转换机（transpiler）”，在内部，Sencha Cmd使用Google（Google's Closure Compiler）的闭包编译器来转换你的代码。

Cmd还利用了闭包所提供的所有填充，因此您也可以使用那些花哨的新数组方法，而不用担心哪些浏览器支持它们。

有些情况下，你不需要所有的这些。
也许你的目标是电子，或者你只支持拥有所有这些特性的现代浏览器。
您可以禁用转换机，并且仍然使用Sencha Cmd代码压缩器来对您的原生ES6代码进行编译。
只需对app.json文件进行一个调整，就可以对transpiler和它的polyfills说再见:
"output": {
    "js": {
        "version": "ES6"
    }
}

## 动态包加载

Sencha Cmd多年来一直支持包的概念，大型应用程序经常利用包来封装类、样式和资源。
然后，Sencha Cmd将这些部分构建到你的应用程序中。
现在，您可以以一种全新的方式使用这些包——动态加载。

如果您现在正在使用包，那么您将在您的app.json中的“requires”数组中看到:
requires: [
    'dashboard',
    'settings',
    'users'
]
要切换到动态加载，只需将其中的一些或全部移动到“uses”数组中，并添加一个新的包来“引用”这些uses数组中的包，这个新的包名为‘package-loader’:
requires: [
    'package-loader'
],
uses: [
    'dashboard',
    'settings',
    'users'
]

在这些变化之后，当Sencha Cmd构建应用程序时，它将为应用程序和每个使用的包分别生成单独的包。
当您的应用程序加载时，它只包含它的代码和'requires'数据中的包的代码，而不是uses中的包。
相反，这些‘uses’数组中的包的JavaScript、CSS和资源将会出现在应用程序的构建（build）文件夹中，就像图像或其他资源一样。

当包资源准备好后，Ext.Package.load()方法使加载包变得很简单。
包加载程序处理包的JavaScript和CSS资产，并递归地加载任何包内可能引用的其他包。

如果您正在使用Ext JS路由，您可能会做一些类似如下的事情来加载一个包:
routes: {
    ':type': {
        before: 'loadPackage',
        action: 'showView'
    }
},

loadPackage: function (type, action) {
    var view = this.getView(),
        pkg = this.getPackageForType(type);

    if (!pkg || Ext.Package.isLoaded(pkg)) {
        action.resume();
    }
    else {
        view.setMasked({
            message: 'Loading Package...'
        });

        Ext.Package.load(pkg).then(function () {
            view.setMasked(null);

            action.resume();
        });
    }
},
对于用户来说，使用动态包加载可以节省时间。
他们不再需要等待应用程序的每一个字节来加载，实际上他们只需要大约20%的时间。
它还可以节省开发人员的时间，因为Sencha Cmd不再需要加载所有代码来进行“dev”模式构建，或者同时监视所有代码。

在“app build”和“app watch”命令中有许多新的命令行开关，让你可以控制哪些外部包(如果有的话)来构建或监视。
这允许您通过将构建限制在当前工作的包(s)上，从而大大减少构建时间。

## 查看它的实际案例

为了让您入门，我们编写了一个演示应用程序，它在一些实际场景中使用了一些包。
查看GitHub的仓库（https://github.com/sencha-extjs-examples/MultiPackageDemo）。


## Progressive Web Apps

Progressive Web Apps(PWAs)提供了一种使用现代Web技术的近乎原生的应用体验。
有了PWA，你可以显示一个横幅，邀请Android用户在他们的主屏幕上安装你的应用。
通过服务线程的魔力和它的缓存(目前在Chrome和Firefox中支持)，你的应用甚至可以离线运行。

Sencha Cmd简化构建过程通过提供一个预构建服务工人(基于谷歌https://github.com/GoogleChrome/sw-toolbox)。
服务线程可以在app.json中配置，它的缓存清单可以由Sencha Cmd在源代码中使用@sw-cache 的注释来增强。
这些注释告诉Cmd，您需要缓存特定的资源，还可以配置如何管理每项资源。

### PWA例子

我们已经整合了一个progressive web 应用示例来展示它是如何工作的。
查看GitHub上的“仓库”，并按照README的指示开始。
GitHub的仓库（https://github.com/sencha-extjs-examples/PWA）包含了Ext JS前端应用程序和基于Node.js的后端服务。

## 项目结构

使用Sencha Cmd 6.5，生成的应用程序不再包含“Sencha app build”命令所使用的构建脚本。
这些脚本现在已经从sencha Cmd安装目录加载，而不是把它们放在本地的.sencha文件夹。

“sass”目录也不再为应用程序生成。
你可以把你的*.scss文件放在与JavaScript文件相同的目录中。
换句话说，对于Foo视图文件夹，您可能拥有所有这些文件:
foo/
    Foo.js
    Foo.scss
    FooController.js
    FooModel.js
建议在app/Application.scss文件或从那里导入的文件的文件中添加通用或全局样式。

### 框架管理

为了简化新项目的设置，如果你有多个应用的话,你可以利用新的“sencha  app init”和“sencha app install”命令以及它们的“工作空间”对应的“sencha workspace init”和“sencha workspace install”命令来管理框架。

所有这些命令都接受了您已经提取Ext JS的路径。
如果您下载并将Sencha SDK的所有内容都提取到一个单独的文件夹中，您可以像这样简化这些命令:	
$ sencha config --prop sencha.sdk.path=~/sencha-sdks --save

在Windows系统中，“~”的部分将被“C:\Uses\Me\”类似的路径所取代。

现在，“sencha-sdks”包含了您下载和提取的所有SDK zips，并且您已经使用“sencha config --ave”保存了这条路径，您不需要将--frameworks参数传递给任何init或install命令。

## 已知的问题

在升级到Cmd 6.5.0之后，一些用户可能很难构建他们的原生Cordova应用程序。
如果您收到与Cordova平台配置有关的任何错误，请手工创建一个名为cordova.local.properties的文件，如果该文件不存在的话。然后将下面的代码放在文件中并再次尝试构建:
cordova.platforms=${app.cordova.config.platforms}
注意:cordova.localproperties 文件应该放置在项目的根目录中。
