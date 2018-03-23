# Sencha Cmd中的工作空间
本指南介绍了[Sencha Cmd](http://www.sencha.com/products/sencha-cmd/)的工作区。工作区被设计用来支持在多个应用程序间共享框架、代码、样式和资源。
## 先决条件
在继续深入之前，推荐阅读以下指南：

- [Sencha Cmd概论](http://docs.sencha.com/cmd/6.5.1/guides/intro_to_cmd.html)
- [使用Sencha Cmd](http://docs.sencha.com/cmd/6.5.1/guides/extjs/cmd_app.html)
## 工作空间是什么?
构建大型应用程序的过程与构建单页面应用程序的过程相同。
一旦应用程序扩展到需要多个页面，就会出现一些常见问题：

- 使用Sencha框架的通用副本（Ext JS和/或Sencha Touch）。
- 使用跨页面通用的代码。
- 共享或第三方包。
为了支持这些，Sencha Cmd定义了“工作区(Workspace)”的概念。
工作空间就是一个文件夹，它最终包含一个或多个页面、框架、包和其他共享代码或文件。
工作空间根文件夹的位置，应该选择在满足这些需求，以及您的源代码控制需求的地方。
任何在工作空间文件夹的子文件夹中创建的应用程序/页面，不管它们的深度如何，都被认为是工作空间的成员。

虽然不是必需的，但通常情况下，工作区文件夹是源码控制存储库中的根文件夹。

在您的工作空间中，您的页面的具体组织形式对于Sencha Cmd来说并不重要。
但是，为了简单起见，本指南中的示例将所有页面创建为工作空间的直接子文件夹。

注意术语。
“应用程序（application）”这个术语可能会让人感到困惑，因为它在不同的组织结构中是重载的，并且具有不同的规模。
对于Sencha框架，“应用程序”是一个web页面。
通常，这些应用程序调用Ext.application()在页面加载中启动适当的代码。
如果一个项目需要多个页面，那么通常情况下，整个页面集合被称为“应用程序”。
为了这些指南，我们将使用Sencha框架里的概念来理解一个应用程序，将它理解为一个web页面。

## 生成一个工作区
要生成工作空间，请使用以下命令：
`sencha generate workspace /path/to/workspace`
这将在指定的文件夹中创建下列结构。

    workspace.json          # The JSON descriptor for the workspace.
    .sencha/                # Sencha-specific files (e.g. configuration)
      workspace/          # Workspace-specific content (see below)
        sencha.cfg      # Configuration file for Sencha Cmd
        plugin.xml      # Plugin for Sencha Cmd
## 配置
配置类似于应用程序的配置。
“app.json”文件保存了单个应用程序的配置，而“workspace.json“包含工作空间中所有应用程序的配置属性。
### 框架的位置
Sencha Ext JS或Sencha Touch的位置（例如“SDK”或“framework”）作为工作空间的配置属性存储在配置文件中。
这允许多个页面共享这个配置。
不同的团队会对这些位置有不同的偏好，以及Sencha SDK是否存储在它们的源代码控制系统中。
下面讨论的设置使您能够控制Sencha SDK在您的工作空间中的位置。

默认情况下，工作区包含Sencha Ext JS和Sencha Touch两种SDK。
这些属性的设置在“.sencha/workspace/sencha.cfg"文件中的如下配置项中：        

    ext.dir=${workspace.dir}/ext
    touch.dir=${workspace.dir}/touch
workspace.dir属性的值由Sencha Cmd确定，并根据需要扩展。
换句话说，默认情况下，工作区内含有它所包含的应用程序所使用的SDK的副本。

应用程序在他们的“app.json”文件中通过framework属性间接引用了他们的框架：

    "framework": "ext"   
## 生成页面
 一旦您拥有了一个工作区，生成页面（“应用程序(apps)”）就可以像以前那样做了，但是使用工作空间中的“ext”文件夹而非sdk目录：
 
    cd apps
    sencha -sdk ../ext generate app NewExtApp new-app
另外，--ext开关可用于从工作空间中选择“ext”框架，而不必担心它的路径：

    cd apps
    sencha generate app --ext NewExtApp new-app
因为这些生成的页面的目标在工作区中，所以会创建下面的结构（这与单页应用程序稍有不同）：

    workspace.json              # The JSON descriptor for the workspace
    .sencha/                    # Sencha-specific files (e.g. configuration)
        workspace/              # Workspace-specific content (see below)
            sencha.cfg          # Workspace's configuration file for Sencha Cmd
            plugin.xml          # Workspace plugin for Sencha Cmd
    
    ext/                        # A copy of the Ext JS SDK
        ...
    
    touch/                      # A copy of the Sencha Touch SDK
        ...
    
    extApp/
        .sencha/                # Sencha-specific files (e.g. configuration)
            app/                # Application-specific content
                sencha.cfg      # Application's configuration file for Sencha Cmd
        app.json                # The JSON descriptor for the application
    
    touchApp/
        .sencha/                # Sencha-specific files (e.g. configuration)
            app/                # Application-specific content
                sencha.cfg      # Configuration file for Sencha Cmd
        app.json                # The JSON descriptor for the application
    
    build/                      # The folder where build output is placed.
        production/             # Build output for production
            touchApp/
            extApp/
        testing/                # Build output for testing
            touchApp/
            extApp/
要生成更多的页面，请重复上述命令。

## 构建应用程序
构建应用程序的过程与一个或多个应用程序没有什么不同。
将工作目录切换到应用程序的根目录并运行：  

`sencha app build`    
为了提高效率，您可以为这个过程创建一个脚本。

## 包
Sencha Cmd提供了一个强大的包管理子系统，可以用来下载和集成JavaScript、样式和资源包到您的应用程序中。

    workspace.json      # The JSON descriptor for the workspace
      packages/       # Container folder for shared Cmd packages
        local/      # Folder for packages generated in this workspace
        remote/     # Folder for packages downloaded into the workspace     

通常，“packages/remote”文件夹会被源代码控制系统所忽略，因为这些包总是可以从包存储库中再次下载和提取。

注意：在以前的版本中，所有的包都被放置在“packages”文件夹中。
这种分离虽然在大多数情况下很有帮助，但也可以在“workspace.json”中进行更改。

## 下一个步骤
在继续深入之前，推荐阅读以下指南：

- [Sencha Cmd包](http://docs.sencha.com/cmd/6.5.1/6.x/cmd_packages/cmd_packages.html)
- [创建Sencha Cmd包](http://docs.sencha.com/cmd/6.5.1/guides/cmd_packages/cmd_creating_packages.html)                             