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
  