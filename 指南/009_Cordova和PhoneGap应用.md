# 与Cordova或PhoneGap整合
## Cordova  
  [Apache Cordova](https://cordova.apache.org/)为Android、iOS、黑莓和Windows Phone设备开发应用而提供了多个api和打包工具。
  Sencha Cmd 5+提供一个支持多个构建配置文件的构建系统。
  这是理想的原生打包工具。
  Cordova Native Packager组件已经被更新，以充分利用这一特性。
  
## PhoneGap
  PhoneGap是在Cordova之上构建的，并且都可以访问Cordova API。
  PhoneGap和Cordova的不同在于他们的打包工具的实现方式。
  PhoneGap为[Adobe PhoneGap Build](https://build.phonegap.com/)提供了一个远程构建接口，让您可以打包并模拟云中的单平台应用程序。
  PhoneGap Native Packager组件已被更新，以充分利用这一特性。

## 选择一个打包工具
   在Cordova和PhoneGap之间选择时，有重要的几点需要注意：
   
- Cordova打包工具可以让您同时为您的机器上的多个平台同时构建。
- Cordova是由开源社区不断更新的，而PhoneGap更新是由Adobe协调的。
- PhoneGap提供了一种远程构建应用程序，可以消除对本地构建的要求。

## 使用带有Cordova和PhoneGap的Sencha Cmd
每一个通过Sencha Cmd生成的应用都可以通过Sencha Cmd提供的这些服务来切换原生构建。
Sencha Cmd将负责所有重复的任务，例如构建应用程序，将其放到Cordova或PhoneGap的适当位置，并运行适当的命令来构建、仿真或运行应用程序。
   
要了解更多关于原生开发的信息，请参阅[Apache Cordova平台指南](https://cordova.apache.org/docs/en/3.0.0/guide_platforms_index.md.html#Platform%20Guides)。
这些指南提供了获取系统启动和运行所需的信息和先决条件。  

### 重要的事项
1.packager.json已经被完全删除。 如果您的项目仍然包含这个文件，您可以安全地删除它。
    这个文件只被Sencha的旧版本的Native Packager（原生打包器）使用。
    
2.对于Cordova和PhoneGap，Sencha Cmd只能创建应用程序的调试版本，而不是发布（对于app store）版本。要创建发布版本，你必须使用一个打包器，比如打包Android的Eclipse或IntelliJ，以及打包iOS的Xcode。
    
3.为了Sencha Cmd Cordova命令能正确执行，您必须安装[Java JRE 1.7](http://www.oracle.com/technetwork/java/javase/downloads/java-se-jre-7-download-432155.html)。
    当Sencha Cmd 6在Windows和Mac OS X上的安装程序内包含最新版本的JRE时，Cordova工具将无法找到并使用Sencha Cmd的内部JRE。

## 环境设置
   在开发Cordova或PhoneGap应用程序之前，您需要使用以下软件来创建您的环境：

1.安装[Java JRE](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html)

2.安装[Node.js](http://nodejs.org/download/)

3.如果您想要用Cordova打包并仿真应用，请使用以下命令安装它：

`npm install -g cordova`

如果你在Mac上，你可能需要添加“sudo"关键字来完成安装。
那么命令将变成：

`sudo npm install -g cordova`

**注意**：无论你是否安装Cordova来打包并模拟你的应用程序，Cordova API对你的应用程序都是可用的。在Cordova或PhoneGap应用中，Cordova的打包和模拟不会影响的Cordova API的使用。

4.如果您想要使用PhoneGap打包并模拟应用，请安使用以下命令来安装它：

`npm install -g phonegap`

如果你在Mac上，你可能需要使用“sudo”关键字来完成安装。
那么命令将变成：
`sudo npm install -g phonegap`

注意：您必须在[Adobe PhoneGap Build](https://build.phonegap.com/)站点上免费注册获得用户名和密码才能访问PhoneGap远程构建站点。

5.下载并安装[Sencha Cmd](http://www.sencha.com/products/sencha-cmd/)。
有关此过程的更多信息，请参阅[Sencha Cmd概论](http://docs.sencha.com/cmd/6.5.1/guides/intro_to_cmd.html)。

6.检查满足目标设备平台特定需求：
  
**Android**:下载并安装[Android SDK管理器](http://developer.android.com/sdk/index.html)。

**黑莓**：查看[黑莓原生SDK](http://developer.blackberry.com/native/)，并使用 BlackBerry Keys Order Form注册你的app。

**iOS**:在[苹果iOS准备金配置文件门户](https://developer.apple.com/account/ios/profile/profileLanding.action)上完成iOS备用金提取的所有步骤（需要一个苹果ID、密码和一个购买的开发者许可）。
使用此站点获得证书、识别设备并获得一个AppID。

另外，下载并安装免费的[Xcode](https://itunes.apple.com/us/app/xcode/id497799835)软件。
在设备上安装应用前，你可以使用Xcode模拟器来调试你的iOS应用。
Xcode只在安装有Linon，Mountain Lion或者Mavericks OS X上的Mac上能正常运行。
  
### config.xml
当你创建PhoneGap或Cordova应用程序时，它们都会为您的项目创建一个`config.xml`文件。

Cordova的`config.xml`文件存储在您的“app_root/cordova”文件夹中，而PhoneGap的`config.xml`文件存储在"app_root/cordova/www"文件夹中。
这是一个问题，因为`www`文件夹是编译构建的目标文件夹。
很有可能这个www文件夹会被删除并进而删除`config.xml`文件。

为了在Sencha Cmd中解决这个问题，当创建PhoneGap应用程序时,我们复config.xml文件到PhoneGap项目的根目录。
每次进行构建时，我们都会将配置文件和编译后的应用程序复制到www文件夹。
这意味着不管您使用哪个框架，用户都可以预期config.xml文件位于Cordova或PhoneGap文件夹的根目录。

有关配置PhoneGap和Cordova的更多信息，请查看以下参考资料：

- [PhoneGap example config.xml file](https://github.com/phonegap/phonegap-start/blob/master/www/config.xml)
- [Configuration Reference](http://docs.phonegap.com/en/3.0.0/config_ref_index.md.html#Configuration%20Reference)
## 开发一个Cordova应用程序
  使用如下Cmd的generate命令创建您的应用程序：
  
	`sencha -sdk /path/to/Framework generate app MyApp /path/to/MyApp`
	
在开发一个Ext 6+通用应用程序时，生成的应用程序的`app.json`文件中包含一个`builds`块。
当您的应用程序已经包含一个`builds`块时，`sencha phonegap/cordova init`命令将不能添加修改到您的app.json文件中。
要添加Cordova支持，请修改您的app.json中的builds块如下：	

  "builds": {
    "classic": {
      "toolkit": "classic",
      "theme": "theme-triton"
    },
  
    "modern": {
      "toolkit": "modern",
      "theme": "theme-cupertino",
      "packager": "cordova",
      "cordova": {
        "config": {
          "platforms": "ios",
          "id": "com.mydomain.MyApp"
        }
      }
    }
  }  
  
导航到新生成的项目文件夹，并运行下列命令之一（APP_ID和APP_NAME参数可选）。

`sencha phonegap init com.mycompany.MyApp MyApp`

或者

`sencha cordova init com.mycompany.MyApp MyApp`

在上述命令完成之后，您现在应该在项目文件夹中看到phonegap或cordova目录。

您还会在项目的app.json文件中看到PhoneGap或Cordova的一个块。
`app.json`位于项目的根目录中。
在这里，您可以设置您要构建的平台（ios或者安卓）。

类似于下面的构建对象现在应该出现在您的app.json文件中。

    "builds": {
      "native": {
        "packager": "cordova",
        "cordova" : {
          "config": {
              "platforms": "ios"
              "id": "com.mydomain.MyApp"
          }
        }
      }
    }
    
让我们来讨论一下上面的代码片段。

### 构建名称
值得注意的是，“native”这个词仅仅是你的构建的名字，它可以是你喜欢的任何单词。
构建名称必须是单个字符串，只能包含没有空格的字母数字字符。
例如，你可以使用类似“ios”、“android”、“iphone”、“ipad”等的构建名称。

注意：虽然您可以为构建名称选择任何东西，但是名称“production”、“testing”和“development”都是为Sencha Cmd保留的，不应该在这个上下文中使用。

### （platforms)平台
然后，您可以将平台对象设置为任何平台或平台的组合。
对于Cordova，你可以指定一个空格分隔的列表，例如“ios android”。

### ID
当第一次生成Cordova应用程序时，将使用“id”属性。
这是应用程序的标识符。
也就是说，你要确保你正确地选择了这个属性。
如果您需要更改它，您将需要从您的项目中删除Cordova文件夹，并在更改了ID属性之后让Cmd重新生成它。
这对于iOS应用程序尤其重要，因为它必须匹配您的Bundle标识符。

从终端或命令行运行以下命令：

`sencha app build {build-name}` 

在这里{build-name}是你的app.json中build对象里定义的构建名称之一。
例如，如果你的构建名称为“android”，命令将是：      

`sencha app build android`

就是这样!
Sencha Cmd现在将创建一个Cordova应用程序，并通过app.json中的platforms属性来构建您指定的平台。

## Sencha Cordova命令
   您可以使用Cordova和这个新的构建对象来使用4个命令。
### Build
`sencha app build {build-name}`

“build”将构建您的Sencha应用程序，然后构建一个原生应用程序。  
### Run
`sencha app run {build-name}`
“run”将构建您的Sencha应用程序和您的原生应用程序。
然后它会尝试在一个连接的设备上运行该应用。
### Emulate
`sencha app emulate {build-name}`
“emulate”将构建您的Sencha应用程序和您的原生应用程序。
然后，它将尝试在模拟器中运行它。
### Prepare
`sencha app prepare {build-name}`
“prepare”将构建您的Sencha应用程序，然后将构建的应用程序准备到原生应用程序中，然而，它不会构建原生部分。
如果您需要在sencha编译后，原生构建之前向应用程序注入一些东西，这是很好的选择。

## 开发一个PhoneGap应用
PhoneGap与Cordova非常相似。
事实上，如果您不是通过PhoneGap云服务来构建的话，这个过程几乎是一样的。

下面是app.json的一个片段，它将提供与上面的Cordova项目相同的PhoneGap项目。

    "builds": {
      "native": {
        "packager": "phonegap",
        "phonegap" : {
          "config": {
              "platform": "ios",
              "id": "com.mydomain.MyApp"
          }
        }
      }
    }
正如您所看到的，从本地转换到远程构建非常简单。
您所需要做的就是将remote标志添加到您的app.json中的build块中。

下一步是提供您的用户名和密码，以便Sencha Cmd能够自动将您登录到PhoneGap远程服务器并上传您的项目。

为此，我们建议添加一个`local.properties`文件到应用程序的根目录。
出于安全考虑，该属性文件应该被所有形式的版本控制软件所忽略。

向该文件添加以下行：

    phonegap.username=myname@domain.com
    phonegap.password=s3nch@isgr3@t

一旦配置好用户名和密码，发送一个构建任务就很简单了。
只要运行`sencha app build native`，你的应用程序就会被编译并发送到云端，让Adobe生成你的原生应用。

构建完成之后，您可以通过PhoneGap门户访问最终包文件，以便在设备上进行测试，或者将其上传至应用程序商店。    