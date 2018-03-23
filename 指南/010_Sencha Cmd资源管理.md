# 资源管理
##  简介
  Web应用程序通常除了JavaScript、HTML和CSS内容外还包括图像和其他非代码资源（例如视频片段、数据文件等）。
  Sencha Cmd将这些额外的资产归类为“resources”，并为他们的管理提供了几种选择。
  理解这些选项的最好方法是开始时简单一些，每次扩展解决方案时只生成一层。
## 设置
   首先，让我们生成一个工作区，其中包含了我们将在下面讨论的所有应用程序和包。
   您可以向CLI发出以下命令：  

    sencha -sdk /path/to/extjs6 generate workspace ./workspace
    
    cd workspace
    
    sencha generate app --ext --classic MyApp ./myapp
    
    sencha generate app --ext UniversalApp ./myuniversalapp
    
    sencha generate package arrow-button

下面是我们工作空间执行结果的文件结构： 
![工作空间构建后的结构](http://docs.sencha.com/cmd/6.5.1/guides/images/workspace_build.png)  

学习本指南的基本概念，工作区并不是必需的。
但是，在处理多个应用程序和/或包时，建议使用一个工作区来协调框架和构建。
关于工作空间的更多信息，请查看我们的[工作空间指南](http://docs.sencha.com/cmd/6.5.1/guides/workspaces.html)。

注意：在应用程序目录中生成一个包，默认情况下将自动添加软件包作为应用程序依赖项。
如果您需要手动将软件包添加到您的应用程序中，只需将包名添加到您的`app.json'中的`requires`块中，就像这样：

    "requires": [
      "font-awesome",
      "arrow-button"
    ],
## 应用程序资源
 让我们从“myapp”文件夹下的经典工具包应用程序开始。
 一个单一工具箱应用程序的标准结构如下：  

    app/
      view/
      ...
    resources/   # home for app resources
    sass/
    app.json  
### app.json
应用程序描述文件，“app.json”包含许多配置选项。
自然，“resources”数组描述了应用程序资源的位置：

    "resources" [{
      "path": "resources",
      "output": "shared"
    ...
    }]   

在诸如此类的单工具包应用程序中，只有第一个条目（如上所示）对应于一个实际的资源文件夹。
剩下的条目（以及第一个条目的“output”属性）在本例中被忽略，但是通用应用程序中这些配置将会发挥作用（更多配置看下面）。

在“app.json”中最高级的“output”对象通过指定应用程序的构建的输出位置来完成这构建结构幅图。这里的开始部分是输出基础地址：

    "output": {
      "base": "${workspace.build.dir}/${build.environment}/${app.name}",
      ...
    },
    
输出基础目录（output base）是放置所有构建产品的根目录。
在上面，输出基础目录是一个使用几个配置变量的公式，当它装载“app.json”文件时，它会被Sencha Cmd解释。
在这种情况下，解释操作将产生以下路径：
    
    ./workspace/build/production/MyApp
默认情况下，资源被复制到这个目录中的“resources”子文件夹中。
这种安排意味着开发模式中的相对路径将与构建文件夹中的相对路径相匹配。

### 主题
应用程序所需要的许多资源都是由其主题提供的。
您可以通过“app.json”指定一个主题，像如下这样：

    "theme": "theme-triton", 

主题通常扩展了其他主题，这使得资源可以根据需要继承和重写。
Triton的父主题是Neptune，因此这两个主题的资源将被应用程序继承并复制到“resources”文件夹中。

为了说明这个过程，让我们考虑一个特定图像资源的传播：

    theme-neptune/
      resources/
        images/
          loadmask/
           loading.gif

现在，像这样构建您的应用程序：

`sencha app build --production`

“loading.gif“是从Neptune的“resources”文件夹复制到应用程序的构建输出目录中。
得到的输出结果如下： 

    build/
      production/
        MyApp/
          index.html    # the output page
          resources/
            images/
              loadmask/
                loading.gif  # from theme-neptune
                
正如您所看到的，构建输出现在包含一个从其基（base）主题继承的图像资源。

#### 资源组合
在上面的例子中，“images/loadmask/loading.gif”资源是从它的基主题——Triton继承而来的。
如果应用程序要在相同的相对路径的“resources”文件夹中创建自己的GIF文件，那么GIF将会覆盖主题中的图像。

您可以使用下面的加载gif来查看应用程序资源的覆盖情况，如果您遵循以下操作：
![loading.gif](http://docs.sencha.com/cmd/6.5.1/guides/images/loading.gif)

要覆盖主题所提供的资源，将替换图像放置到对应于主题资源文件夹的resources文件夹中。
在本例中，我们将重写主题的加载蒙板“loading.gif”文件。
所以请把你的图像放在这里：

    app/
      view/
      ...
    resources/
      images/
        loadmask/
          loading.gif   # override theme image
    sass/
    app.json

现在，您可以构建您的应用程序的生产版本，您应该看到应用程序的“loading.gif“在生成的构建resource文件夹中。

类似地，Triton主题可能覆盖了Neptune的图像，但这对应用程序来说是透明的。
应用程序唯一关心的是配置的主题使用这张图像;
这张图像是来自应用程序本身还是来自主题（或者继承的主题）是完全灵活的。

### 代码包
与主题类似，代码包也可以包含资源。
这些资源也被复制到“resources”构建输出文件夹中。
但是，为了保护多个独立开发的包使它们避免冲突，这些资源被放置在一个以包名命名的文件夹中。

例如，让我们访问上面生成的“arrow-button”包。
这个包应该在其资源中包含一个图像，这将有助于说明包的资源管理。

注意：如果您遵循本指南，您可以方便地使用下面的箭头：

![green arrow](http://docs.sencha.com/cmd/6.5.1/guides/images/arrow.png)
![green arrow](http://docs.sencha.com/cmd/6.5.1/guides/images/arrow2.png)
这个包将包含一个这样的结构：

    resources/
      images/
        arrow.png   # the arrow image used by this package
      ...
    sass/
    package.json   

构建应用程序的生产版本时，软件包的资源会被复制到应用程序构建目录中，像正面这样：

    build/
      production/
        MyApp/
          index.html    # the output page
          resources/
            arrow-button/   # the package's resource "sandbox"
              images/
               arrow.png

与主题一样，应用程序也可以覆盖这些资源：                                              

    app/
      view/
      ...
    resources/
      arrow-button/
        images/
         arrow.png   # override package's image
    sass/
    app.json        

从视觉上看，这个过程是这样的：
![application override resources](http://docs.sencha.com/cmd/6.5.1/guides/images/app_override.png)
## 包资源
如上所示，应用程序使用软件包资源并将它们集成到它们的构建中。
这个过程是由包描述符（“package.json”）引导的，它描述了构建一个单独的包的过程。
   
### package.json
包描述符类似于应用程序描述符（“app.json”）。
该文件是包作者用来配置资源位置和构建输出位置的地方。
例如:

`"output": "${package.dir}/build",`

与“app.json”的“output”配置类似，上面的内容决定了软件包构建过程中包资源被复制到的位置。
“resources”对象在“package.json”中默认不存在。但是默认情况下相当于如下的配置：

    "resources": [{
      "path": "resources"
    }]

这跟`app.json`中的配置是一样的。
### 包构建
为了让包能够在非cmd应用程序很容易使用，必须在包目录中运行以下内容：  

`sencha package build`

与应用程序构建不同，包构建没有“production”或“testing”这样的环境概念。
相反，一个包的构建会产生压缩的、优化的代码和未压缩的、可调试的代码。 

 build/
   arrow-button-debug.js
   arrow-button.js
   resources/
     images/
       arrow.png   # the arrow image used by this package
 ...
 package.json

然后，“build”文件夹中的内容可以直接在不使用Sencha Cmd的应用程序中作为`script`和`link`链接元素引用。

### 资源路径
软件包构建和应用程序构建之间的不同之处在于，它与资源有关，是应用程序构建为包的资源创建一个资源沙箱。
这意味着从CSS文件到包的资源的相对路径在包构建和应用程序构建之间是不同的。

在.scss和.js代码中，都有一些API决定了给定资源的正确路径。
例如，箭头按钮可能有这样的CSS规则：

    .arrow-button {
      background-image: url(get-resource-path('images/arrow.png'));
    }

在包构建中，get-resource-path不会将资源放入沙箱。
然而，当在应用程序构建中使用时，get-resource-path将对应的包的资源放入沙箱。

在JavaScript中，Ext.getResourcePath API执行相同的任务。

    image.setSrc(
      Ext.getResourcePath('images/arrow.png', null, 'arrow-button')
    );

与“get-resource-path”不同，JavaScript等效方法Ext.getResourcePath不能在缺省情况下确定软件包名称，因此必须提供它。
然而，这个参数只适用于代码包。
主题包不需要这个参数，因为它们的资源不是沙箱化的。

注意：第二个参数null是资源池名称，在本例中没有用到。
它通常被通用应用程序使用。

## 通用应用程序和构建配置文件
到目前为止所讨论的应用程序和软件包已经成为了Sencha 5版本以后应用程序的标准示例。
在Ext JS 6中引入通用应用程序，以及它们对构建配置文件的使用，为平衡资源的配置添加了另一个变量。

虽然在[Microloader指南](http://docs.sencha.com/cmd/6.5.1/guides/microloader.html)中详细讨论了构建配置文件，但是它们可以被总结为一种指导Sencha Cmd来构建多个独立优化的应用程序风格的方法。
应用程序在页面加载时决定构建何种概要文件，这取决于适用于应用程序的策略。

例如，构建概要文件可以用于支持多个主题或场所。
在Ext JS 6通用应用程序中，构建概要文件（build profiles）于针对现代和经典的工具包（通常是分别针对移动和桌面创建优化的构建）。

让我们继续，打开我们上面生成的通用应用程序，这样我们就可以解决其中的一些差异。
### app.json
让我们打开“myuniversalapp”应用根目录下的"app.json"文件。
#### builds
`builds`配置是在"app.json"文件中的”builds"对象属性。Ext JS 6入门应用程序有如下的"builds"对象配置：

    "builds": {
    
      "classic": {
        "toolkit": "classic",
        "theme": "theme-triton"
      },
    
      "modern": {
        "toolkit": "modern",
        "theme": "theme-triton"
      }
    
    },  
    
“classic”和“modern”属性来匹配它们各自的“toolkit”选项，但可以是任何名称。
为了避免toolkit名称和构建概要文件名带来的混淆，本例的名称将是“foo”和“bar”： 

    "builds": {
    
      "bar": {
        "toolkit": "classic",
        "theme": "theme-triton"
      },
    
      "foo": {
        "toolkit": "modern",
        "theme": "theme-triton"
      }
    
    },   

因为构建配置文件会调整诸如“toolkit”(工具箱)和“theme”之类的基本内容（因此产生不同的CSS文件），所以它们的资源必须彼此隔离。
换句话说，“foo”构建概要文件将拥有它自己的CSS文件，因此必须将其资源与“bar”构建概要文件分开。

对于某些资源来说，这种隔离是必要的，但是也有一些资源是两个构建概要文件可能想要共享的（比如一个视频片段）。  
#### output(输出)
 可以调整“app.json”文件的“output”对象来处理这两种情况。
 这是默认情况下的通用启动程序所做的处理： 

    "output": {
    
      "base": "${workspace.build.dir}/${build.environment}/${app.name}",
      ...
      "resources": {
        "path": "${build.id}/resources",
        "shared": "resources"
      }
    
    }, 

“resources”对象属性（“path”和“shared”）被称为“资源池(resource pools)”。
在Sencha Cmd 6.0.1之前，只有一个资源输出位置（“output.resources.path”）。
在Sencha Cmd 6.0.1中，这是默认的资源池。
还有一个新的“shared”资源池，但是可以有任意数量的额外输出位置添加到对象中。

在上面的配置中，资源将被放置在与CSS文件相同的子目录中。
它是基于“build.id”的，并扩展构建概要文件的名称（“foo”或“bar”）。
需要注意的是，“output”对象中的所有相对路径都是相对于“output.base”的。

让我们继续构建通用应用程序，这样我们就可以查看生成的build文件夹了。
执行“cd”进入“myuniversalapp”，并从您的CLI中发出以下命令：
 
`sencha app build --production`

注意：这将为两个构建目标创建一个构建:foo和bar。

查看生成的构建文件夹：
 
build/
  production/
    UniversalApp/
      index.html    # the output page
      bar/          # the build profile using "classic"
        resources/
          UniversalApp-all.css
          images/
            loadmask/
              loading.gif  # from theme-neptune
      foo/          # the build profile using "modern"
        resources/
          UniversalApp-all.css
          ... 
### 资源
与上面的资源池对应的是顶级“resources”数组中的每个条目的可选“output”属性。

    "resources": [{
      "path": "resources",
      "output": "shared"   # targets the "shared" resource pool
    }, {
      "path": "${toolkit.name}/resources"   # uses the default pool
    }],  

给定这些资源位置，通用应用程序仍然可以覆盖“loading.gif“。但为了做到这一点，它就会把图像移动到”classic/resources”文件夹中。

    app/
      view/
      ...
    resources/
      ...
    classic/       # from the ${toolkit.name} in the resources array
      resources/
        images/
          loadmask/
            loading.gif   # override theme image
    sass/
    app.json      
### 共享资源（shared resources）
当资源可以被共享时，它应该被放在顶级的“resources”文件夹中，而不是“classic/resources”或者“modern/resources”文件夹中。
例如:

    app/
        view/
        ...
    resources/
        video.mp4
    classic/
        resources/
    sass/
    app.json 
这个文件夹与“shared”资源池相关联，该资源池指向构建输出目录根部的“resources”文件夹：

    build/
      production/
        UniversalApp/
          index.html    # the output page
          resources/    # shared resource pool
            video.mp4
          foo/          # build profile
            ...
          bar/          # build profile
            ...     
默认情况下,包资源不共享,会构建这样的输出结果:
![unshared package resources](http://docs.sencha.com/cmd/6.5.1/guides/images/unshared_package_resources.png)

在主题包的情况下，这种隔离是必要的，并且这种逻辑在缺省情况下也适用于所有的包。
然而，在Sencha Cmd 6.0.1中，软件包资源也是可以共享的。

#### 共享包资源
像应用程序一样，包可以通过在资源(resource)条目上使用“output”属性指定资源池名称来描述特定资源池的资源。
例如，“font-awesome”包包含将近800 KB的字体资源，这些资源不需要为每个构建配置文件复制。
为了支持这种优化，”font-awesome“包的"app.json"文件看起来包含这样的配置：

    "resources": [{
      "path": "${package.dir}/resources",
      "output": "shared"
    }]
    
对于没有定义资源池的应用程序，这些资源将被放置到默认资源池中。
虽然这在某些情况下可能是低效的，但这确保了资源将是相互可用的。
因此，总是建议在使用构建概要文件的应用程序中声明一个“shared”资源池。

修改“arrow-button”资源以遵循“font-awesome”使用的模式，产生以下构建输出：    
![shared package resources](http://docs.sencha.com/cmd/6.5.1/guides/images/shared_package_resources.png) 
#### 定位共享资源
为了在.scss中定位一个共享资源，您可以向`get-resource-path`API提供资源池名称：
`background: url(get-resource-path('images/foo.png', $pool: 'shared'));`

在JavaScript可以这样调用：

`image.setSrc(Ext.getResourcePath('images/foo.png', 'shared'));`

某些组件configs将接受URL类型的资源池标签（例如Ext.Img#src）：

    items: [{
        xtype: 'img',
        src: '<shared>images/foo.png'
    }]
### 包和构建配置文件
软件包构建还提供了对构建概要文件的支持。
软件包的构建概要文件与应用程序的非常相似，但是包构建概要文件用于创建供非Cmd应用程序使用的软件包构建构件。
因此，软件包可以使用构建概要文件来创建多种版本的构建版本，或为'toolkit'或为'theme'，这取决于这些变量如何影响其代码或样式的可用性。
    
## 结论
Sencha Cmd的资源管理选项允许您描述您的输入资源并灵活地控制构建输出的形式。
结合使用API来确定最终的资源位置，所有这些灵活性都不需要转换成代码复杂性。
    