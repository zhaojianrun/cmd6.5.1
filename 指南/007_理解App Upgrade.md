# 理解App Upgrade

本指南解释了如何在您的应用程序中管理Sencha Cmd和Sencha框架的升级。

## 先决条件

在继续之前，推荐先阅读以下指南:

- [Introduction to Sencha Cmd](http://docs.sencha.com/cmd/6.5.1/guides/intro_to_cmd.html).
- [Using Sencha Cmd](http://docs.sencha.com/cmd/6.5.1/guides/extjs/cmd_app.html).
## 升级你的应用程序
  
  生成的应用程序包括与Sencha Cmd相关的两种基本内容:构建脚本(或脚手架)和目标Sencha SDK的重要内容。
  因此，您偶尔需要对这些部分进行升级。
  您可以使用以下命令来完成此操作:
  
  `sencha app upgrade [ path-to-new-framework ]`

“path-to-new-framework”（新框架的路径）是可选的，用于升级Sencha Cmd脚手架和应用程序所使用的框架。

### 准备升级

在您的应用程序源代码中执行任何批量操作时，最好从版本控制的“干净”状态开始。
也就是说，最好在执行升级之前没有未提交的更改。
通过这种方式，您可以轻松地查看并可以放弃`sencha app upgrade`所带来的更改，从而使由此带来的麻烦最少。

## 升级Sencha Cmd脚手架
   
   要想从以前版本的应用程序生成Sencha Cmd的新版本，可以从应用程序内部运行这个命令:
   
   `sencha app upgrade`  
   
   这将取代“.sencha“里的内容，但也将升级”app.js”以及其他一些文件。
## 升级框架
   
   由于生成的应用程序包含了它们最初生成的SDK的副本，因此需要对应用程序进行更新，以使用SDK的新版本。
   `sencha app upgrade`命令将用新的SDK取代旧的SDK副本:
   
   `sencha app upgrade ../path/to/framework`
   
   上面的命令指向已经下载并提取的SDK的路径。

重要提示：
不要使用像使用`sencha generate app`那样使用`-sdk`命令开关。而是使用上面所示的命令。

如果您从Ext JS 4.1升级到4.2+，此命令还可能还会对生成的源代码进行一些更改。

## 处理合并冲突

在Sencha Cmd中，代码生成器合并了一个三路合并，以最好地协调它生成的代码与上次生成的代码，以及您可能已经编辑过的当前状态的代码。
这种方法允许你用许多方法编辑文件(比如“app.js”)，只要你的改变跟Sencha Cmd想要做的改变不重叠。

合并过程遵循了如下“app.js”的伪代码(作为示例):  

    mv app.js app.js.$old
    regenerate last version to app.js.$base
    generate new version to app.js
    diff3 app.js.$base app.js.$old app.js 

在理想的情况下，您在工作中不会注意到这种机制。
然而，在有些情况下，您可能会收到一条错误消息，告诉您存在“合并冲突”，您需要手动解决这个问题。

在无法重新创建基本版本的情况下，".$old"文件会留在磁盘上，您可以将它与当前版本进行比较。
或者您可以使用您的源代码控制系统来比较当前文件和最后提交的文件。

当基本版本可以生成(多数情况下)时，“.$old“文件被删除，因为冲突在目标文件中以标准方式注释:

    <<<<<<< Generated
        // stuff that Sencha Cmd thinks belongs here
    =======
        // stuff that you have changed which conflicts
    >>>>>>> Custom
### 使用可视化合并工具
    
这一过程与您在源控制系统中所期望的文件相匹配，这些文件被您和别的用户(在本例中，是指Sencha Cmd)都修改过了。
与版本控制一样，解决这些问题的理想方法是使用可视化的合并工具。
为了配置Sencha Cmd在遇到合并冲突时调用一个合并工具，你需要设置以下两个属性:
  
    cmd.merge.tool
    cmd.merge.tool.args
如果合并工具在PATH环境变量中，那么设置cmd.merge.tool属性可以仅设置程序名，但是，如果它不在环境变量中，
可能需要设置该应用的可执行文件的完整路径。

相应的cmd.merge.args属性应该根据需要的合并工具所需的命令行参数设置。
该属性是一个模板，可以包含以下可替换令牌:   

    cmd.merge.tool.args={base} {user} {generated} {out} 
该模板首先以空格分割，然后用实际的文件名替换令牌。
如果合并工具有更多的定制需求，那么可能需要设置cmd.merge.tool到一个可以封装合并工具调用的shell脚本。 
#### 合并工具辅助属性
     
 Sencha Cmd提供了一些属性来帮助配置几个流行的合并工具:
 
 对于[p4merge](http://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools)，你可以设置如下属性:
 