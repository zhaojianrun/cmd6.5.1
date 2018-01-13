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
   
   ##语言扩展
   ###  动态变量
     
   动态变量在Fashion中扮演着非常重要的角色。
     动态变量与普通变量相似，但是它们的值被包裹在“dynamic()”中。
     动态变量与普通变量不同的是它们如何相互作用。思考如下示例：
     
     
     $bar: dynamic(blue);
     
     $foo: dynamic($bar);  // Notice $foo depends on $bar
     
     $bar: dynamic(red);
     
     @debug $foo;  // red
     
   注意，$foo是根据$bar计算出来的的。
    这种依赖是通过Fashion来处理的，而$foo的计算被延迟到$bar的最终值被确定的时候。
    
   换句话说，动态变量有两种处理方式:赋值和计算。
    
   #### 赋值
    
   动态变量，就像普通的变量一样，被分配到正常的“级联”顺序(不像!default):
    
    $bar: dynamic(blue);
    
    $bar: dynamic(red);
    
    @debug $bar;  // red
    
   这允许工具将自定义值附加到任何代码块并控制其动态变量。
    
   动态变量的赋值只能发生在文件作用域内，并且在所有的控制结构外部。
    例如，这是非法的:
    
    $bar: dynamic(blue);
    
    @if something {
        $bar: dynamic(red); // ILLEGAL
    }
    
   实现上面想要的效果，可以这样写：
    
   $bar: dynamic(if(something, red, blue));
   
此需求对于启用下面讨论的评估和提升行为是必要的。

动态变量在使用声明之后即可进行重新赋值，dynamic()可以用也可以不用。

$bar: dynamic(blue);

$bar: red;  // reassigns $bar to red

$bar: green !default;  // reassigns $bar to green

@debug $bar;  // green    

没有“default dynamic”这样的东西。

### 计算

动态变量以依赖顺序进行运算，而不是声明顺序。
声明单只适用于单个变量赋值的级联。
这可以在上面的例子中看到。
这个排序还意味着我们甚至可以删除$bar的第一个设置，而代码将具有相同的结果。

考虑一个更复杂的例子:

$bar: dynamic(mix($colorA, $colorB, 20%));

$bar: dynamic(lighten($colorC, 20%));


$bar的原始表达式使用$colorA和$colorB。
如果这是唯一的被赋值到$bar的变量，那么$bar将依赖于这两个变量并在它们之后进行计算。
由于$bar被重新赋值，并且随后只使用$colorC，在最终的分析中，$bar仅依赖$colorC。
最初的赋值到$bar变量的行为可能从来没有发生过。

### 动态变量调序
    
   为了完成所有这些，Fashion收集了所有动态变量，并在Sass代码的任何其他执行之前对它们进行运算。
    换句话说，类似于JavaScript变量，动态变量被“吊”到顶部。
   
### 变量提升
     
   当使用变量来为动态变量赋值时，这些变量会被提升为动态变量。
   
   $foo: blue;
   
   $bar: dynamic($foo);
   
   虽然$foo被声明为一个普通变量，因为$bar使用$foo，Fashion会将$foo提升为动态的。
   
   注意:这意味着$foo现在必须遵循动态变量的规则。
   
   这种行为被支持是为了使以前版本的Sencha Cmd获得最大的可移植性。
   当变量被提升时，会产生一个警告。
   在将来的版本中，这个警告将会变成一个错误。
   我们建议纠正这段代码，以正确地将所需的变量声明为dynamic()。
   
### 扩展——引用JavaScript
      
   您可以通过在JavaScript中编写代码来扩展Fashion。要从需要它的Sass代码中引用这段代码，使用require()。
      例如:
  
   require("my-module");
   
   // or
  
   require("../path/file.js"); // relative to scss file
   
   在内部，Fashion使用ECMAScript 6(ES6)中的System.import API(或者通过SystemJS（https://github.com/systemjs/systemjs）的polyfill（https://github.com/ModuleLoader/es-module-loader）)来支持加载标准的JavaScript模块。
   
 一个模块可以用pre-ES6的语法这样编写:
 
     `exports.init = function(runtime) {
         runtime.register({
             magic: function (first, second) {
                 // ...
             }
         });
     };
 `
 
 使用SystemJS，您可以让“transpilers”在任何浏览器中编写ES6代码。
 在ES6中，上面的代码是这样写的:
 
      `module foo {
          export function init (runtime) {
              runtime.register({
                  magic: function (first, second) {
                      // ...
                  }
              });
          }
      }`
## 升级
   
### 动态变量
   
   在升级到Ext JS 6时，动态变量的内部使用可能会影响这些变量在应用程序和定制主题中的赋值。
   虽然不是必需的，但我们建议您更改变量赋值为dynamic()。
   在大多数情况下，这00将是机械地将！default(在以前的版本中采用的方法)替换为dynamic():
   
       `// before:
       
       $base-color: green !default;
       
       // after:
       
       $base-color: dynamic(green);`
   分配给动态变量的更严格的语法生成的错误，将更容易分析识别。
   
### Compass的扩展
       
   使用Ruby代码的Compass功能将不可用，因为Ruby不再被使用。
   具有相同功能的模块将使用JavaScript创建。
   在很多情况下，都可以使用require()在JavaScript中实现缺失的功能。
   然而，Compass中的Sass代码包含在Fashion中，因此并不是所有的Compass功能都受到了影响。
   一般来说，如果您没有使用任何定制或基于Ruby的Compass功能，那么您很可能不需要做任何更改。
   
## 结论
     
 我们对Fashion的功能感到充满激情，希望你也是！
 快速地为你的应用程序定制样式从来都没有像现在这样容易，而扩展Sass现在可以用与框架一样的语言来完成。
 请务必在论坛（https://www.sencha.com/forum/forumdisplay.php?8-Sencha-Cmd）上留下任何反馈。
   
   