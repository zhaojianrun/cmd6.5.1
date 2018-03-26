# 使用Cmd 6.5.0构建先进的Web应用程序（Progressive Web App（PWA））
Cmd 6.5+可以帮助你将任何一个Ext JS应用变成一个先进的网页应用。
  
先进的web应用程序提供了使用现代web技术的原生应用体验。
Google提供了一个优秀的入门级Web应用程序介绍：[你的第一个PWA](https://developers.google.com/web/fundamentals/getting-started/codelabs/your-first-pwapp/)

Cmd特别允许开发者有如下权限：

- 显示一个应用程序横幅，邀请Android用户将应用安装到主屏幕上。

- 通过基于service-worker的缓存提供脱机支持。
目前只有Chrome和Firefox支持这一功能。[Edge的支持](https://developer.microsoft.com/en-us/microsoft-edge/platform/status/serviceworker/)很快就会到来。

注意：您必须首先在本地安装[Node JS](https://nodejs.org/en/)，才能使用Cmd渐进式Web应用程序集成。

## 需求
使用PWA功能需要Node JS 6+

## 添加到主屏幕的横幅
要在你的应用程序中添加一个“添加到主屏”的横幅，请在你的app.json中添加一个progressive的配置对象。
progressive配置对象有两个项目:manifest和serviceWorker。
manifest配置包含一个[web应用程序清单](https://developer.mozilla.org/en-US/docs/Web/Manifest)的数据。在构建时，一旦这个配置就绪，Cmd会自动地将所需的`<link rel="manifest" />`标签添加到你的应用程序的index.html中。

web应用程序清单的出现并不一定意味着受支持的浏览器会立即显示添加到主屏幕的banner。
例如，Chrome使用一组标准，包括使用服务工作者、SSL状态和访问频率试探法来定何时显示横幅。

例子:

    "progressive": {
      "manifest": {
        "name": "My App",
        "short_name": "My App",
        "icons": [{
            "src": "resources/icon-small.png",
            "sizes": "96x96"
        }, {
            "src": "resources/icon-medium.png",
            "sizes": "192x192"
        }, {
            "src": "resources/icon-large.png",
            "sizes": "256x256"
        }],
        "theme_color": "#054059",
        "background_color": "#054059",
        "display": "standalone",
        "orientation": "portrait",
        "start_url": "/index.html"
      }
    }
## 使用Service Worker缓存
  也许PWA最重要的特性是提供离线功能。
  一般来说，这是通过缓存应用程序外壳（HTML、JavaScript、CSS、字体以及显示应用程序所需的图像资源）以及内容来完成的，这些内容通常是通过AJAX调用REST API来获取的。
  
  PWA使用[services workers](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers)提供缓存。
  当“progressive”配置出现在您的app.json时，Cmd在构建时使用[sw-precache](https://github.com/GoogleChrome/sw-precache)创建一个service worker，它会自动缓存应用程序shell。
  您还可以通过在您的代码中添加@sw-cash注释来让(service worker)服务工作线程缓存AJAX请求。
  
#### // @sw-cache
  假设一个商店从服务器检索即将发生的事件的列表。
  我们可以通过在url上添加@sw-cache注释让Cmd生成的service worker缓存API调用，以便在离线时使用：     
  
    Ext.define('App.store.UpcomingEvents', {
      extend: 'Ext.data.Store',
      proxy: {
        type: 'ajax',
    
        // @sw-cache
        url: '/api/upcoming-events.json',
        reader: {
            type: 'json'
        }
      }
    });

@sw-cache注释可以放在任何字符串或对象属性之上。
它所连接的字符串应该是被缓存的url。
当url是动态的（例如，它在path或查询字符串中包含一个id）时，您可以选择指定一个“urlPattern”配置来控制哪些请求被缓存。
例如，想象一个针对特定事件的AJAX请求： 
   
    Ext.define('App.model.Event', {
      extend: 'Ext.data.Model',
      fields: ['id', 'name', 'date'],
    
      proxy: {
        type: 'rest',
    
        // @sw-cache { urlPattern: "\\/api\\/events\\/\\d+" }
        url : '/events'
      }
    });    

## 控制缓存行为
@sw-cache注释接受了许多选项：

- **urlPattern**:当匹配正则表达式时，会导致响应被缓存

- **处理程序**:默认为“networkFirst”。可以是以下的一个：

  - **networkFirst** 尝试通过从网络获取请求来处理请求。
如果成功，将响应存储在缓存中。
否则，尝试从缓存中执行请求。
用于基本的通读缓存的策略。
对于非常适用于您总是想要最新的数据API请求，而一旦网络数据不可能，可以以用旧的缓存数据，而非无数据可用。

  - **cacheFirst** 如果请求与缓存条目匹配，则使用cacheFirst进行响应。
否则，尝试从网络获取资源。
如果网络请求成功，则更新缓存。
这个选项对不会变化的资源有很适用，或者有其他的更新机制的资源。

  - **fastest** 最快地从缓存和网络中并行请求资源。
谁最先返回就使用谁响应。
通常使用的是缓存的版本，前提是有可用的缓存。
一方面，这个策略总是会生成一个网络请求，即使资源被缓存了。
另一方面，如果网络请求完成，缓存将被更新，因此未来的缓存读取数据将更加及时。

  - **cacheOnly** 从缓存中解析请求，或者失败（即使失败也不发网络请求）。
当你需要保证不发出网络请求的时候，这个选项是很好的，例如在移动设备上保存电池电量。

  - **networkOnly** 通过从网络获取URL来处理请求。
如果fetch失败，则响应失败请求信息。
本质上，这与没有为URL创建路由是一样的。

- **选项**：具有以下属性的对象：

  - **debug**[Boolean] 决定是否将额外的信息打印到浏览器的控制台。

  - **networkTimeoutSeconds**[Number] 应用于toobbox.networkFirst 内置处理程序的超时。
如果设置了networkTimeoutSeconds，那么任何耗时超过该时间的网络请求都会自动回到缓存响应中，如果缓存存在的话。
当networkTimeout没有设置时，浏览器的本机联网超时逻辑就会被应用。

  - **cache**[Object]

    - **name**[String] 用来存储响应对象的高速缓存的名称。
使用一个惟一的名称允许您定制缓存的最大大小和条目的过期时间。
maxEntries号为通过各种内置处理程序缓存的条目使用了最近使用的缓存过期策略。
您可以使用这个缓存专门为动态的资源集存储条目，而这些资源没有自然的限制。
设置缓存。
    - **maxEntries**[Number] 对通过各种内置处理程序缓存的条目使用一个最近使用的缓存过期策略。                            您可以使用这个缓存专门为动态的资源集存储条目，而这些资源没有自然的限制。
设置cash.maxEntries到，例如，10意味着在第11个条目被缓存之后，最近使用的条目将被自动删除。
                             缓存永远不应该超过缓存。
                             maxEntries条目。
                             这个选项只有在缓存.name被设置时才会生效。它可以单独使用，也可以与cache.maxAgeSeconds一起使用。
这个选项只有在缓存.name被设置时才会生效。它可以单独使用，也可以与cache.maxAgeSeconds一起使用。
maxAgeSeconds编号为缓存条目设置了最大的年龄，以秒为单位。
您可以使用这个缓存专门为动态的资源集存储条目，而这些资源没有自然的限制。
设置缓存。
maxAgeSeconds，例如，86400意味着任何超过一天的条目都会被自动删除。
这个选项只有在cache.name被设置时才会生效。它可以单独使用，也可以与cache.maxEntries一起使用。
示例：限制API调用的缓存响应数量
如果我们只希望从上面的例子中缓存最近访问的10个最近的事件，我们可以通过添加    

        