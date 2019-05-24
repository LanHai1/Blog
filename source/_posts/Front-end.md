---
title: 前端题 n道
---

----
#### 基础强化
----
- 1. 聊一聊前端存储。
  (1)老朋友cookie
  (2)短暂的sessionStorage
(3)简易强大的localStorage
(4) websql 与indexeddb
详细见 [https://segmentfault.com/a/1190000005927232](https://segmentfault.com/a/1190000005927232)

----
- 2. BFC
(1) w3c 规范中的 BFC 定义
浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline- blocks, table-cells，和 table- captions），以及 overflow 值不为 visiable'的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。
在 BFC 中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的 margin 值所決定的。在一个 BFC 中，两个相部的块级盒子的垂直外边距会产生折叠。
在 BFC 中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘（border-left)（对于从右到左的格式来说，则触碰到右边缘）。
(2) BFC 的通俗理解：
首先 BFC 是一个名词，是一个独立的布局环境，我们可以理解为一个箱子（实际上是看不见 摸不着的），箱子里面物品的摆放是不受外界的影响的。转换为 BFC 的理解则是：BFC 中的元素的布局是不受外界的影响（我们往往利用这个特性来消除浮动元素对其非浮动的兄弟元素和其子元素带来的影响。）并且在一个 BFC 中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。
详细参见
http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html
https://www.zhihu.com/question/28433480
----
#### 前端工程化
----
- 3. 场景：你是第一天来公司上班的，项目代码托管在 Gitlab，项目地址: `gt@lab.com: org/project.gt`，现在有一处代码需要你修改。请下完成此项任务中，与 git/gitlab相关的操作步骤。
第一步：`$> ssh-keygen-trss-zhangsan@abc.com `
第二步：`拷贝公钥到 gitlab `
第三步：`$> git config -global user name zhangsan` => `$> gitconfig-globaluser.emailzhangsan@abc.com `
第四步：`$> gitclonegit (@lab. com:org/project.git`
第五步：`$> git checkout -b project-20170227- zhangsan- bugfix `
第六步：`修改代码`
第七步：`git status `
第八步：`git add`
第九步：`git commit- am ' bugfix`
第十步：`git push- set-upstream origin project20170227- zhangsan- bugfix`
----
- 4. CSS, JS 代码压缩，以及代码 CDN 托管，图片整合。
 (1) CSS, JS 代码压缩
可以应用 gulp 的 gulp- uglify, gulp- minify-CSs 模块完成；可以应用 webpackf 的 Uglify Jsplugin 压缩插件完成。
 (2) CDN
内容分发网络（CDN）是一个经策略性部署的整体系统，包括分布式存储、负载均衡、网络请求的重定向和内容管理 4 个要件。主要特点有：本地 Cache 加速，镜像服务，远程加速，带宽优化。关键技术有：内容发布，内容路由，内容交换，性能管理。CDN 网站加速适合以咨询为主的网站。CDN 是对域名加速不是对网站服务器加速。CDN 和镜像站比较不需要访客手动选择要访问的镜像站。CDN 使用后网站无需任何修改即可使用 CDN 获得加速效果。如果通过 CDN 后看到的网页还是旧网页，可以通过 URL 推送服务解決，新增的网页和图片不需要 URL 推送。使用动态网页可以不缓存即时性要求很高的网页和图片。CDN 可以通过 gt 或 SVN 来管理
 (3) 图片整合
减少网站加载时间的最有效的方式之一就是減少网站的 HTPい请求数。实现这一目标的一个有效的方法就是通过 CSS Spritesーー将多个图片整合到一个图片中，然后再用 CSS来定 位。缺点是可维护性差。可以使用百度的 fis/webpack 来自动化管理 sprite
----
- 5. 如何利用 webpack 把代码上传服务器以及转码测试？
  (1) 代码上传
可以使用 sftp-webpack-plugin，但是会把子文件夹给提取出来，不优雅。可以使用 gulp + webpack 来实现  
(2) 转码测试
webpack 应用 babel 来对 ES6 转码，开启 devtool: "source-map"来进行浏览器测试。应用 karma 或 mocha 来做单元测试。
----

- 6. 项目上线流程是怎样的？
(1) 流程建议
`模拟线上的开发环境`
本地反向代理线上真实环境开发即可。（apache, nginx, modest 均可实现）
`模拟线上的测试环境`
模拟线上的测试环境，其实是需要一台有真实数据的測试机，建议没条件搭 daily 的，就直接用线上数据测好了，只不过程序部分走你们的測试环境而已，有条件搭 daly 最好。
`可连调的测试环境`
可连调的测试环境，分为 2 种。一种是开发测试都在一个局域网段，直接绑 hosts 即可，不在一个网段，就每人分配一台虚拟的测试机，放在大家都可以访问到的公司内网，代码直接往上布即可。
`自动化的上线系统`
自动化的上线系统，可以采用 Jenkins。如果没有，可以自行搭建一个简易的上线系统，原理是每次上线时都抽取最新的 tunk 或 master，做一个 tag，再打一个时间戳的标记，然后分发到 cdn 就行了。界面里就 2 个功能，打 tag，回滚到某 tag，部署。
`适合前后端的开发流程`
开发流程依据公司所用到的工具，构建，框架。原则就是分散独立开发，互相不干扰，连调时有 hosts。可绑即可。
(2) 简单的可操作流程
代码通过 gt 管理，新需求创建新分支，分支开发，主干发布
上线走简易上线系统
通过 gulp+ webpack 连到发布系统，一键集成，本地只关心原码开发
本地环境通过 webpack 反向代理的 server
搭建基于 linux 的本地測试机，自动完成 build+push 功能
----
- 7. 工程化怎么管理的？
gulp 和 webpack
![](https://upload-images.jianshu.io/upload_images/12946880-ea0d9c7ec5c076a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
8. git 常用命令
Workspace：工作区 
Index/ Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
详细参见：http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html
----

- 9. Webpack 和 gulp 对比
Gulp 就是为了规范前端开发流程，突现前后端分离、模块化开发、版本控制、文件合并与压缩、mock 数据等功能的一个前端自动化构建工具。说的形象点，"Gulp 就像是一个产品的流水线，整个产品从无到有，都要受流水线的控制，在流水线上我们可以对产品进行管理。”另外，Gup 是通过 task 对整个开发过程进行构建。
Webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔等到实际需要的时候再异步加载。通过 loaders 的转换，任何形式的资源都可以视作模块，七如 Commonjs 模块、AMD 模块、ES6 模块、CSS、图片、JSON、Coffeescript、LESS 等
Gulp 和 Webpack 功能实现对比：从基本概念、启动本地 server、sass/less 预编译、模块化开发、文件合并与压缩、mock 数据、版本控制、组件控制八个方面对 Gulp 和 Webpack 进行对比。
详细参见 http://www.tuicool.com/articles/e632EbA
----
- 10. Webpack 打包文件太大怎么办？
webpack 把我们所有的文件都打包成一个 JS 文件，这样即使你是小项目，打包后的文件也会非常大。可以从去除不必要的插件，提取第三方库，代码压缩，代码分割，设置缓存几个方面着手优化。
详细参见http://www.jianshu.com/p/a64735eb0e2b
----
- 11. 不想让别人盗用你的图片，访问你的服务器资源该怎么处理？
目前常用的防盗链方法主要有两种：
(1) 设置 Referer：适合不想写代码的用户，也适合喜欢开发的用户
(2) 签名 URL：适合喜欢开发的用户
详细参见: https://yq.aliyun.com/articles/57931
----
- 12. 精灵图和 base64 如如何选择？
css 精灵，用于一些小的图标不是特别多，一个的体积也稍大，比如大于 10 K（这个没有严格的界定）。
base64, 用于小图标体积较小（相对于 css 精灵），多少都无所谓。字体图标，用于一些别人做好的图标库（也有少数自己去做的）用起来比较方便，他的图标只能用于单色，图标用只能于一种颜色。
----
- 13. webpack怎幺引入第三方的库?
jQuery实例: 
```
entry: {
page: 'path/to/page.js',
jquery: 'node_modules/jquery/dist/jquery.min.js' }
new HtmlWebpackPlugin({ 
filename: 'index.html', 
template: 'index.html',
inject: true,
chunks: ['jquery', 'page'] // script 
})
```
----
- 15. 用过Nginx吗?都用过哪些?
nginx是一个高性能的HTTP和反向代理服务器。
常使用场景:
(1)反向代理
(2)网站负载均衡
详细参见: http://www.cnblogs.com/hobinly/p/6023883.html
----
#### 移动端布局与适配
---
- 16. iscroll安卓低版本卡顿,
`如何解决?`
方案一: iScroll v5.1 .3设置momentum: true
方案二:配置probeType
方案三:开启硬价加速:给scroll元素 增加css样式: -webkit-transform:translate3d(0,0,0);
方案四:判断手机版系统版本，应用原生CSS: overflow:scroll-y
--- 
- 17. 移动布局自适应不同屏幕的几种方式
(1)响应式布局
(2) 100%布局(弹性 布局)
(3)等比缩放布局(rem)
---
- 20.你们做移动端平时在什么浏览器上测试?
Chrome, Safari, 微信X5，UC,其他手机自带浏览器
---
- 21.说说移动端是如何调试的?
`移动端调试:`
(1)模拟手机调试
(2)真机调试之android手机+Chrome
(3)真机调试之iphone + safari
(4) UC浏览器
(5)微信内置浏览器调试
(6) debuggap
(7)抓包
详细参考:  https://segmentfault.com/a/1190000005964730
----
- 22.说说ICONFONT是如何用的?
从以下几个方面做答:
(1) font-face
(2)什么是iconfont,iconfont怎么用
(3) iconfont怎 么做
(4) iconfont的利和弊
详细参见:  https://segmentfault.com/a/1190000005904616?_ea=953657
---
- 23.说说移动端Web分辨率
从以下几个方面做答:
(1) PC到移动，渲染的变迁
(2)可以更改的布局宽度
(3)再次变迁的像素
(4)又一次变迁
(5)是时候说说安卓了
详细参见: https://segmentfault.com/a/1190000005884985
----
#### 性能和效率
---
- 24.你平时如何评测你写的前端代码的性能和效率。
Chrome Dev Tools的Timeline:是用来排查应用性能瓶颈的最佳工具。
Chrome Dev Tools的Audits:对页面性能进行检测，根据测试的结果进行优化。
第三方工具Yslow。
详细参见:
http://www.cnblogs.com/-simon/p/5883336.html http://blog.csdn.net/ivan0609/article/details/45508365 http://www.wtoutiao.com/p/1305TZW.html
---
- 25.如何优化页面，加快页面的加载速度(至少5条)
(1)优化图片资源的格式和大小
(2)开启网络压缩
(3)使用浏览器缓存
(4)减少重定向请求
(5)使用CDN存储静态资源
(6)减少DNS查询次数
(7)压缩css和js内容
详细参见: http://www.mahaixiang.cn/wyzz/1589.html
----
- 26.怎么保证多人开发进行内存泄漏的检查(内存 分析
工具)
使用xcode里面的Analyze进行静态分析，为避免不必要的麻烦，多人开发的时候尽量使用
ARC。
详细参见: http://www.jianshu.com/p/424c15b9b73a
----
- 28.浏览器http请求过多怎么解决?
(1) 合并JS、CSS文件
(2)合并图片css sprite
(3)使用Image maps
(4) data嵌入 图片:如base64
(5)使用CDN,减少http请求头
----
#### Web安全
----
- 29.你所了解到的Web攻击技术
(1) XSS攻击
(2) CSRF攻击
(3)网络劫持攻击
(4)控制台注入代码
(5)钓鱼
详细参见:  http://blog.csdn.net/fengyinchao/article/details/52303118
----
- 30.如何防止XSS攻击?
(1)将前端输出数据都进行转义
(2)将输出的字符串中的反斜杠进行转义
(3)从urI中获取的信息，防止方法是由后端获取，在前端转义后再行输出
(4)使用cookie的HttpOnly属 性,保护好cookie
详细参见:http://blog.csdn.net/fengyinchao/article/details/52303118
----
- 31.项目中有没有用过加密，哪种加密算法?
`项目中没有用过，但我了解几个加密算法:`
(1) RSA加密
(2) MD5加密
(3) SHA256加密 
----
- 32.聊一聊网页的分段传输与渲染
`从下面几个方面说:`
(1) CHUNKED编码
(2) BIGPIPE
(3)分段传输与bigpipe适用场景
详细参见: https://segmentfault.com/a/1190000005989601?_ea=984496
---
- 33.百度移动端首页秒开是如何做到的?
`从几个方面优化:`
(1) 静态文件放置
(2) 缓存
(3)外链
(4)缓存DOM
(5)使用iconfont
(6)卡片的异步加载与缓存
(7)不在首屏的就要异步化
(8)少量静态文件的域名
详细参见: https://segmentfault.com/a/1190000005882953
---
- 34.前端速度统计(性 能统计)如何做?
回答下面的两个问题,
(1)网站都有哪些指标?
(2)如何统计自己网站的这些指标?
详细参见: https://segmentfault.com/a/1190000005869953
---
#### 架构
- 35.如果让你来制作一个访问量 很高的大型网站，你会如何来管理所有css、js文件、图片?
(1) 遵循自定的一套CSS, JS和图片文件和文件夹命名规范
(2)依托采用的前端工程化工具,依照工具脚手架规范(gulp, webpack, grunt, yeoman)
(3)依据采用的框架规范(Vue, React, jQuery)
----
- 36.如果没有框架、怎么搭建你的项目
`应用原生JS自己尝试搭建一个MVC架构:`
(1)基本模块
common:公共的一组件，下面的各 模块都会用到
config:配置模块，解决框架的配置问题
startup:启动模块，解决框架和Servlet如何进行整合的问题
plugin:插件模块，插件机制的实现，提供IPlugin的抽象实现
routing:路由模块，解决请求路径的解析问题，提供了lRoute的抽象实现和基本实现
controller:控制器模块，解决的是如何产生控制器
model:视图模型模块,解决的是如何绑定方法的参数
action: action模块， 解决的是如何调用方法以及方法返回的结果，提供了lActionResult的抽象实现和基本实现
view:视图模块，解决的是各种视图引擎和框架的适配
filter:过滤器模块，解决是执行Action,返回IActionResult前后的AOP功能，提供了lFilter
的抽象实现以及基本实现
(2) 扩展模块
filters: 一些IFiter的实现
results:一些IActionResult的实现
routes:一些IRoute的实现
plugins:一些IPlugin的实现
详细参考 http://www.cnblogs.com/lovecindywang/p/4444915.html
----
- 37.在选择框架的时候要从哪方面入手
`影响团队技术选型有很多因素，如技术组成，新技术，新框架，语言及发布等。为了更好的考量不同的因素，需要列出重要的象限， 如开发效率、团队喜好，依次来决定哪个框架更适合当前的团队和项目。上线时间影响框 架选择,不要盲目替换现有框架。`
(1) jQuery 
项目功能比较简单。并不需要做成一个单页面应用，就不需要MV*框架。项目是一个遗留系统。与其使用其他框架来替换，不如留着以后重写项目。
(2) AngularJS
当我们在制作一个应用，它对性能要求不是很高的时候，那么我们应该选择开发速度更快的技术栈AngularJS,她拥有混合开发能力的ionic框架。对于复杂的前端应用来说，基于Angular.js应用的运行效率，仍然有大量地改进空间。Angular2需 要学习新的语言，需慎重选择。
(3) React
选择React有两个原因，一是通过Virtual DOM提高运行效率，二是通过组件化提高开发效率。大型项目首选。选择React还有一个原因是: React Native、React VR等等，可以让React运行在不同的平台之上。我们还能通过React轻松编写出原生应用，还有VR应用。令人遗憾的是React只是一个View层，它是为了优化DOM的操作而诞生的。为了完成一个完整的应用，我们还需要路由库、执行单向流库、web API调用库、测试库、依赖管理库等等，为了完整搭建出一个完整的React项目，我们还需要做大量的额外工作。
(4) Vue.js
对于使用Vue.js的开发者来说，我们仍然可以使用熟悉的HTML和CSS来编写代码。并且，Vue.js 也使用了Virtual DOM、Reactive 及组件化的思想，可以让我们集中精力于编写应用，而不是应用的性能。对于没有Angular和React经验的团队，并且规模不大的前端项目来说，Vue.js 是一个非常好的选择。
详细参见: https://zhuanlan.zhihu.com/p/25194137
----
- 38.聊一聊前端模板与渲染
(1)页面级的渲染，后端模板
如smarty,这种方式的特点是展示数据快，直接后端拼装好数据与模板，展现到用户面前，对SEO友好。
(2)异步的请求与新增模板，前端模板
如Mustache, ArtTemplate, 前端解析模板的引擎的语法，与后端解析模板引擎语法一致。这样就达到了一份HTML前后端一起使用的效果。
详细参见: https://segmentault.com/a/1190000005916423
----
#### 混合开发:
- 39.UIWebView和JavaScript之间是怎么交互的?
UIWebView是ioS SDK中渲染网面的控件，在显示网页的时候，我们可以hack网页然后显示想显示的内容。其中就要用到JavaScript的知识，而UIWebView 与javascript交互的方法就是stringByEvaluatingJavaScriptFromString:
有了这个方法我们可以通过objc调用javascript,可以注入javascript。
Js调用OC方法原理就是利用UIWebView重定向请求，传-些命令到我们的UlWebView,在UlWebView的delegate的方法中接收这些命令，并根据命令执行相应的objc方法。这样就相当于在javascript中调用objc的方法。
在android中，我们有固有组件webview,经过设置可以让它支持我们的js的渲染，然
后在代码中设置(WebViewClient/WebChromeClient) 让应用跳转页面时在本webview中跳
转，通过webview.loadurl (String str)方法可以在需要的地方加载我们前端的页面或者调用前端所定义的方法(wv.loadUrl("javascript:sendData ToAndroid(我是来自js的呦，你看到了吗";)，我们再通过JavascriptInterface接口设置我们前端和android通讯的标识，wv.addJavascriptlnterface(new MJavascriptinterface(getApplicationContext()),
"WebViewFunc");
这样前端就可以在页面上调用我们的方法了，fun1方 法是在android中定义的
Window.WebViewFunc.fun1();
总之，前端和android或 者ios进行结合开发，我们称之为混合开发，原理就是在原生的开发语言中，我们提供了-个组件webview,这个组件就是我们的原生语言的浏览器，但是我们得自行设置让其能够完美支持我们的应用，需要设置对应的标识，然后连接起来，我们称之为JavascriptInterfac。
---
- 40.混合开发桥接api是怎么调用的，需要引入类库嘛?
调用的对象是什么?
Hybrid框架结构
HyBrid App = H5 App + Native框架
H5App用来实现功能逻辑和页面渲染
Native框架提供WebView和设备接口供H5调用
方案一重混合应用，在开发原生应用的基础上，嵌入WebView但是整体的架构使用原生应用提供，- -般这样的开发由Native开发人员和Web前端开发人员组成。Native开发人员会写好基本的架构以及API让Web开发人员开发界面以及大部分的渲染。保证到交互设计，以及开发都有一个比较折中的效果出来，优化得好也会有很棒的效果。Hybrid App技术发展的早期，Web的运行性能成为主要 瓶颈!为解决性能问题Hybrid App走向“重混”。
通过多WebView:实现流畅的多页加载和专场动画。
使用Navtive UI组件:框架、菜单、日期等。
“重混”的优缺点
优点:
提升了运行性能
增强了交互体验
缺点:
Web和Native技术交叉混杂
需要同时掌握Web和Native技术，学习难度增加
一个页面有Web组件也有Native组件，编程 调试困难需要引入各自需要的各种依赖工具
方案二:轻混合应用，使用PhoneGap、AppCan之 类的中间件，以WebView作为用户界面层，以Javascript作为 基本逻辑,以及和中间件通讯，再由中间件访问底层API的方式，进行应用开发。这种架构一般会非常依赖WebView层的性能。
随着时代的发展，手机硬件、 浏览器技术、无线网络技术都得到了大幅的提升，H5已经可以支持复杂应用，并拥有良好的运行性能。使用轻混方案的App也越来越多。
目前我们要学习的Hybrid App开发就是方案二，使用H5+Js+Native框架开 发当前轻混合应用。
Phonegap引入phonegap.js或者cordova.js,对象为navigator
Dcloud引入引入mui.js或者其他的js组件，对象为plus
apiloud引入各种第三方插件，对象为api
顺变提一下，2012年8月，微信公众平台的.上线，重新定义了移动应用:移动应用
= Iphone App + Android App +微信App
----
- 41. 说一下你对支付，推送(远程，本地)的理解.
消息的推送主要有两种:
一种是本地推送，主要应用在系统的工具中，例如:闹钟，生日提醒等;实现本地推送需要以下三个步骤:
1.实例化一个本地推送对象
2.设置通知对象的各个属性
3.添加本地推送对象
一种是远程消息推送，主要应用联网设备的信息推送，例如:邮件，各种软件的广告或优惠信息的推送。远程推送比较复杂，需要使用开发者账号进行申请证书，获得实现推送功能的配置文件，所以想要实现远程推送功能，必须要有开发者账号并且生成
配置文件
1.完成证书的申请和Xcode的配置
2.在Demo中注册远程服务对象，并设置其代理
3.找一个简单的App服务器进行消息推送(推荐使用:PushMeBaby, gitup网站 上就
有)
4.运行PushMeBaby
参考网址: http://blog.csdn.net/u014642572/article/details/26857717
远程推送流程图如下
![](https://upload-images.jianshu.io/upload_images/12946880-beb9f9f520dd693a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---

- 42.什么是代理和通知，写一下他们基本的实现方式
通知:一对一 一对多传值
**四个步骤:**
1.发送通知
2.创建监听者
3.接收通知
4.移除监听者
**使用场景:**
1.很多控制器都需要知道一个事件，应该用通知;
2.相隔多层的两个控制器之间跳转
**注意事项:**
1.一旦接收消息的对象多了，就难以控制了，可能有你不希望的对象接
受了消息并做了处理
2.创建了观察者，在dealloc里 面一定要移除;
代理:“一对一”，对同一个协议， 一个对象只能设置一个代理delegate
**六个步骤:**
1.声明一个协议,定义代理方法
2.遵循协议
3.设置一个代理对象
4.调用代理方法
5.给代理赋值
6.实现代理方法
**注意事项:**
1,单例对象不能用代理;
2,代理执行协议方法时要使用responds ToSelector检查其代理是否符合
协议(检查对象能否响应指定的消息)，以避免代理在回调时因为没有实现方法而造成程序崩溃
**使用场景:**
公共接口，方法较多也选择用delegate进行解耦
iOS最常用tableViewDelegate, textViewDelegate
iOS有很多例子比如常用的网络库AFNetwork, ASIHTTP库，UIAlertView类。
Block:Block是iOS4.0+和Mac OS X 10.6+引进的对C语言的扩展，用来实现匿名函数的特性。Blocks 语法块代码以闭包得形式将各种内容进行传递，可以是代码，可以是数组无所不能。闭包就是能够读取其它函数内部变量的函数。就是在-段请求连续代码中可以看到调用参数(如发送请求)和响应结果。所以采用Block技术能够抽象出很多共用函数，提高了代码的可读性，可维护性,， 封装性。
使用场景:
一:动画
二:数据请求回调
三:枚举回调
四:多线程gcd
注意事项:
block需要注意防止循环引用
参考网址: http://www.cnblogs.com/wenboliu/articles/5422033.html
---
- 43.UIViewController的生命周期
(1)通过alloc init 分配内存,初始化controller.
(2) loadView (loadView方法默认实现[super loadView]如果在初始化con troller时指定了xib文件名,就会根据传入的x ib文件名加载对应的xib文件,如果没传xib文件名,默认会加载跟control ler同名的xib文件,如果没找到相关联的xib文件,就会创建一个空白的UIView,然后赋給controller的view)
(3) viewDidLoad (当loadView创建完view之后， 此时view已经完成加载了，会调用viewDidLoad方法; -般我会在这里做界面上的初始化操作，比如添加按钮,子视图,等等.)
(4)viewWi llAppear (当view在load完之后 ,将要显示在屏幕之前会调用这个方法，在重写这些方法时候最好先调用一下系统的方法之后在做操作。)
(5)viewDidAppear (当view已经在屏幕上显示出来之后,会调用这个方法，当一个视图被
移除屏幕并且销毁的时候)
(6)viewWillDisappear (当视图将要从屏幕上移除时候调用)
(7)viewDidDisappear (当视图已经 从屏幕上移除时候调用)
(8) Dealloc (view被销毁时候调用，如果是手动管理内存的话，需要释放掉之前在init和
viewDidLoad中分配的内存(类似alloc, new, copy); dealloc方 法不能由我们主动调用,必须等引用计数为0时候由系统调用. )
参考网址: http://www.cnblogs.com/wujy/p/5822329.html
---
- 44.rem布局字体太大怎么处理?
一般情况下我们设置了html根节点的字体大小作为rem单位的一个基本标准，那么我们可以紧接着在body标签内设置一个字体大小为该应用的基本字体大小
针对于一些机型如果一开始就显示的字体不正常，我们可以通过判断机型然后加载不同的样式
```
<script language="javascript">
    window.onload = function() {
        alert("1");
        var u = navigator.userAgent;
        if(u.indexOf('Android') > -1 || u.indexOf('Linux')> -1){ //安卓手机
            aler("安卓手机");
        } else if(u.indexOf('iPhone')>-1){ //苹果手机
           alert("苹果手机"); 
        } else if(u.indexOf('Windows Phone') > -1) { //winphone手机
            alert("winphone手机");
        }
    }
</script>
```
---
- 45.如何调用原生的接口?
首先你得选择一个合适的框架作为自己的基础，以Dcloud为例， 页面中一定要存在一个事件，plusready， plusready实际 上是原生将桥接js注入到页面中的容器，进行任何方法调用的时候都在plusready之后。所有api方法全部都托管在了-个plus对象中。 使用语法plus.模块名称.具体方法(参数，callback)
当我们需要打开系统相册的时候，可以这样做:
Gallery模块管理系统相册，支持从相册中选择图片或视频文件、保存图片或视频文件到相册等功能。通过plus gallery获取相册管理对象。打开相plus.gallery.pick进行打开，选取多个图片{multiple:true,maximum:9,system:false}
----
- 46.微信支付怎么做?说说流程
(1)申请微信公众号及支付功能申请:根据公众号申请流程申请即可。
(2)获取商户支付配置信息及支付测试配置:
支付授权目录最多可以配置三个域名，测试授权目录只可以一个,这里需要
注意的是域名大小写必须要网站URL一致，否则会无法通过授权，提示支付请求的URL不合法。另外，测试支付的微信号必须加到测试白名单，否则无法进行支付测试。
(3)H5页面发起支付请求，请求生成支付订单 获取用户授权(获取用户的openid)
(4)调用统一下单API， 生成预付单
(5)生成JSAPI页面调用的支付参数并签名，注意时间戳timeStamp是32位字符串
(6)返回支付参数prepay_ id,paySign参 数的htmI文本给前端。
(7)微信浏览器自动调起支付JSAPI接口支付，提示用户输入密码。
(8)确认支付，输入密码，提交支付。
(9)步通知商户支付结果，商户收到通知返回确认信息。
(10)返回支付结果，并发微信消息提示。
(11)展示支付信息给用户，跳转到支付结果页面。
----
- 47.混合开发的注意点
增强WebView:原生WebView基本 是PC平台浏览器内核的移植，但对于移动场景并不完全适合，各种硬件API得不到HTML5原生支持。因此对于WebView的种种Hack、增强应运而生，甚至出现了基于增强WebView提供第三方服务的。
路由:应用内跳转由于加入了WebView而变得复杂起来，同时由于组件化、模块化带来的问题，路由也成为人们讨论的重点。
缓存:移动网络条件差,为了用户体验，必须要做资源缓存和预加载。
通信:即HTML5和Native之 间的通信。利用系统提供的桥接API可以实现，不过在应用上还有着一些坑点和安全问题。
----
- 48.说说你对手机平台的安装包后缀的理解
Android: **.apk
los:**.ipa
Windows: wp7 wp8的是xap
wp8.1以后用8.1的sdk开发的是appx
---
#### NodeJS:
- 49.谈谈你对Socket编程的理解，及实现原理，Socket之间是怎么通讯的
A、Socket定义
Socket是进程通讯的一种方式，即调用这个网络库的- -些API函数实现分布在不同主机的相关进程之间的数据交换。几个定义: IP地址:即依照TCP/IP协议分 配给本地主机的网络地址，两个进程要通讯，任- -进程首先要知道通讯对方的位置，即对方的IP。端口号:用来辨别本地通讯进程，一个本地的进程在通讯时均会占用一个端口号，不同的进程端口号不同，因此在通讯前必须要分配一个没有被访问的端口号。连接:指两个进程间的通讯链路。
B、实现原理
在TCP/IP网络应用中，通信的两个进程间相互作用的主要模式是客户/服务器(Client/Server, C/S)模式，即客户向服务器发出服务请求，服务器接收到请求后，提供相应的服务。客户/服务器模式的建立基于以下两点:首先，建立网络的起因是网络中软硬件资源、运算能力和信息不均等，需要共享，从而造就拥有众多资源的主机提供服务，资源较少的客户请求服务这一非对等作用。其次,网间进程通信完全是异步的，相互通信的进程间既不存在父子关系，又不共享内存缓冲区，因此需要一种机制为希望通信的进程间建立联系，为二者的数据交换提供同步，这就是基于客户/服务器模式的TCP/IP。
C、通讯过程服务器端:其过程是首先服务器方要先启动，并根据请求提供相应服务:
(1) 打开-通信通道并告知本地主机，它愿意在某-公认地址上的某端口(如FTP的端口 可能为21)接收客户请求;
(2)等待客户请求到达该端口;
(3)接收到客户端的服务请求时，处理该请求并发送应答信号。接收到并发服务请求，要激活-新进程来处理这个客户请求(如UNIX系统
中用fork、exec) 。新进程处理此客户请求，并不需要对其它请求作出应答。服务完成后，
关闭此新进程与客户的通信链路，并终止。
(4) 返回第(2) 步，等待另一客户请求。
(5)关闭服务器客户端:
(1)打开一通信通道,并连接到服务器所在主机的特定端口;
(2)向服务器发服务请求报文，等待并接收应答;继续提出请求.... 
(3)请求结束后关闭
通信通道并终止。从上面所描述过程可知:
(1) 客户与服务器进程的作用是非对称的，因此代码不同。
(2) 服务器进程一般是先启动的。只要系统运行，该服务进程一直存在，直
到正常或强迫终止。
详细参见:
https://www. zhihu.com/question/29637351
http://blog.csdn.net/panker2008/article/details/46502783?ref=myread
---
- 50.WEB应用从服务器主动推送Data到客户端有哪些方式?
一般的服务 器Push技术包括:
(1)基于AJAX的长轮询(long-polling) 方式，服务器Hold- -段时间后再返回信息;
(2) HTTP Streaming,通过iframe和<script>标签完成数据的传输; 
(3) TCP长连接
(4) HTML 5新引入的WebSocket,可以实现服务器主动发送数据至网页端，它和HTTP一
样，是一个基于HTTP的应用层协议，跑的是TCP, 所以本质上还是个长连接，双向通信,
意味着服务器端和客户端可以同时发送并响应请求，而不再像HTTP的请求和响应。
(5) nodejs的http://socket.io, 它是websocket的一 个开 源实现，对不支持websocket的浏
览器降级成comet / ajax轮询，http://socket.io的 良好封装使代码编写非常容易。
上述的1和2统称为comet技术。comet详 细参考: http://www.ibm.com/developerworks/cn/web/wa-lo-comet/
--- 
- 51.简述Node.js的适用场景?
IO密集而非计算密集的情景;高并发微数据(比如账号系统)的情景。特别是高并发,
Node.js的性能随并发数量的提高而衰减的现象相比其他server都有很明显的优势。
一篇外文:
Bad Use Cases:
CPU heavy appsSimple
CRUD / HTML apps
NoSQL + Node.js + Buzzword Bullshit
Good Use Cases :
JSON APIs
Single page apps
Shelling out to unix tools
Streaming data
Soft Realtime Applications
详细参见:
http://nodeguide.com/convincing_the_boss.html
---
- 52.什么是HTTPS,做什么用的呢?如何开启HTTPS?
(1)什么是HTTPS 
https是http的加密版本，是在http请求的基础上，采用ssI进行加密传输。
(2)做什么用
加密数据，反劫持，SEO
(3)如何开启
生成私钥与证书，配置nginx, 重启nginx看 效果
详细参见: https://segmentfault.com/a/1190000006199237?utm_source=:tuicool&utm_medium=referral
---
- 54.如何用NodeJS搭建中间层?
![](https://upload-images.jianshu.io/upload_images/12946880-4b4c719be3ff5b2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最上端是服务端，就是我们常说的后端。后端对于我们来说，就是一个接口的集合，服务端提供各种各样的接口供我们使用。因为有Node层，也不用局限是什么形式的服务。对于后端开发来说，他们只用关心业务代码的接口实现。
服务端下面是Node应用。
Node应用中有一层Model Proxy与服务端进行通讯。这一层主要目前是抹平我们对不同接口的调用方式，封装-些view层 需要的Model。
Node层还能轻松实现原来vmcommon,tms (引用淘宝内容管理系统)等需求。
Node层要使用什么框架由开发者自己决定。不过推荐使用express+x Template的组合，xTemplate能做到前后端公用。
怎么用Node大家自己决定，但是令人兴奋的是，我们终于可以使用Node轻松实现我们想要的输出方式:JSON/JSONP/RESTful/HTML/BigPipe/Comet/Socket/同步、异步，想怎么整就怎么整,完全根据你的场景决定。
浏览器层在我们这个架构中没有变化，也不希望因为引入Node改变你以前在浏览器中开发的认知。
引入Node,只是把本该就前端控制的部分交由前端掌控。
详细参见: http://blog.csdn.net/u011413061/article/details/50294263
----


###持续更新中～

