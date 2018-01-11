#Fashion

##简介
   
   在Sencha Cmd 6.0中，我们为Ext JS 6.0的主题开发而开发了一个新的、快速的工具，叫做“Fashion”。
   结合Fashion和sencha app watch，为我们的主题开发创造了一种新的模式，我们称之为“实时更新”。
   
   Live Update使用Fashion来编译，然后在你的运行页面中注入最新的CSS。
   这意味着您不必重新加载页面以查看主题更改，而是在浏览器中几乎实时地看到这些更新。
   
   Sencha Cmd 6在为Ext JS 6应用程序编译主题时也使用了Fashion。
   因为时尚是用JavaScript实现的，所以不再需要Ruby了。
   
  ## Fashion是什么?
   
  Fashion是一个编译器，用于将 .scss文件生成CSS。
   时尚还添加了一些在Sass中不能用的新功能，允许像Sencha Inspector等工具一样对主题(或您的应用程序)定义的变量进行视觉检查和编辑。
   
  ### JavaScript扩展
   
   用户可以通过编写JavaScript模块来扩展Fashion的功能。   一般来说，Ext JS用户可能最熟悉JavaScript，因此扩展Fashion应该比扩展Compass简单得多。
   下面我们将进一步讨论如何扩展Fashion。
   
   
  ### 兼容性
   
   Fashion与CSS3语法以及大多数sass-spec（https://github.com/sass/sass-spec）验证套件兼容。
   因为Fashion确实实现了Sass的大部分代码，所以才使得使用Fashion来编译scss代码并不难。
   
   然而，由于稍后讨论的附加功能，说Fashion“是Sass的实现”或Fashion编译的语言“是Sass”是不对的。
   在很多地方，“sass”这个词被用来作为配置选项或磁盘上的文件夹的名称。
   出于兼容性的考虑，这些配置选项仍然被命名为“sass”，尽管配置项下的语言并不是严格意义上的“sass”。
    
   ## 使用实时更新
   您可以在(现代)浏览器中打开应用程序，而Sass文件将被加载，而不是生成的CSS。
   然后，Fashion可以对文件修改做出反应，重新编译Sass，并在不重新加载页面的情况下更新CSS。 
   在sencha app watch命令中，有两种方式可启用Fashion。
     
   您可以通过编辑在app.json中“development”对象来启用Fashion。
   
   ...
   
   "development": {
   
       "tags": [
           "fashion"
       ]
   },
   ...
   
   或者，当你加载页面时你可以在页面URL后添加添加“?platformTags=fashion:1”来启用Fashion。
   
   现在我们准备好了:
   
  * 在应用程序的根目录下执行“sencha app watch”。
  * 在浏览器中导航到您的应用程序(例如,http://localhost:1841/app)。
   现在，当您修改您的主题变量时，应该能在浏览器中立即看到更改效果。
   
   注意:实时更新只会在Cmd启用的web服务器中查看页面时才起作用。
   在Ext JS的经典工具包中，一些Sass的更改可能需要布局或重新加载页面。
   而在现代工具包中，这将不再是一个问题，因为它更依赖于CSS3，并将重新设计以适应更具挑战性的变化。
   