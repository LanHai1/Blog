---
title: webAPI
---
本篇讲解页面中的一些api与用法和兼容

🤪
## 一、什么是webAPI?
- web: 网页
- API: 接口 一套别人封装的属性和方法
- webAPI: 专门操作网页的方法和属性
`万物皆对象，在webAPI中把网页中所有元素 <element> 都当成对象来处理`
----
## 二、文档树
### 2.1 三个部分
- 文档: document
- 元素: 网页中的标签
- 节点: 网页中所有的内容都叫节点(包括标签、属性、文本、注释)
### 2.2 节点的三个属性
```
1. nodeType(节点类型)
2. nodeName(节点名字)
3. nodeValue(节点值)
```

#### 2.2.1 元素节点
`元素节点就是标签节点`
1. nodeType: 1
2. nodeName: 标签名(大写) 
3. nodeValue: null

#### 2.2.2 属性节点
`属性包括属性名和属性值`
1. nodeType: 2
2. nodeName: 属性名
3. nodeValue: 属性值

#### 2.2.3 文本节点
1. nodeType: 3
2. nodeName: #text
3. nodeValue: 文本内容

#### 2.2.4 注释节点
1. nodeType: 8
2. nodeName: #comment
3. nodeValue: 注释内容
----
## 三、window对象
- window对象代表浏览器 
- window对象是JavaScript中的顶级对象
- 任何**全局变量**和**全局函数**声明后 在预解析的过程中都会自动保存为window对象里面的属性和方法
- window的两个特殊属性名不能再次声明!! 
`name: name属性名不管赋值任何值都会转换为字符串 let name = {}; -> [object Object] `
`top`
- window的两个方法
```
window.open(href); /*打开一个网页*/
window.close(); /*关闭网页*/
```
----
## 四、事件
#### 4.1 什么是事件?
> 在计算机中 事件代表捕捉了用户进行了什么操作 再给对应的事件处理
#### 4.2 事件三大部分
`事件源 事件类型 事件处理函数`
##### 4.2.1 事件源
> 用户操作的是什么元素 事件中的this就是事件的事件源(谁触发这个事件那this就是谁)

##### 4.2.2 事件类型
> 用户进行了什么操作
###### 4.2.2.1 on
- onmouseover 鼠标移入
- onmouseout 鼠标移出
- window.onload 当页面加载完成后执行
`入口函数 如果想将js代码写在html之前 就可以用这个事件 将js代码写在事件处理函数中`
- window.onunload 当页面退出前执行 
`可以保存用户信息 => 用户未保存信息直接退出网页`
- onfocus 获得焦点
文本框光标闪烁
- onblur 失去焦点
- onkeyup 键盘弹起
- onkeydown 键盘按下
- onscroll 滚动
`当滚动页面滚动条时触发`
- onclick 点击
`onclick 和 onmousedown 区别`
`onclick 按下弹起才触发`
`onmousedown 按下就会触发`

*实现拖拽*
*要考虑元素是否有margin 移动的x和y🉐️减去元素原有的margin*
*给元素添加鼠标按下事件*
*在按下事件里面给页面添加鼠标移动事件*
*给元素注册鼠标弹起事件 => 清除页面鼠标移动事件*
- onmousedown 鼠标按下
- onmouseup 鼠标弹起
- onmousemove 鼠标移动

*H5拖拽*
*需要为拖拽的元素添加 draggable = "true" 属性*
**拖拽元素添加**
- ondragstart 拖拽开始
- ondrag 拖拽中
- ondragend 拖拽结束
**拖拽的目标元素添加**
- ondropenter 有元素拖进来触发
- ondropleave 有元素拖拽离开触发
- ondropover 主要是为了配合ondrop使用，一定要在ondropover事件里调用`e.preventDefault()`才能让ondrop触发
- ondrop 有元素拖到我范围内并松手才触发
###### 4.2.2.2 addEventListener()
三参数
```
/*
事件名 不加on
事件处理函数
boolean值 是否事件捕捉
*/
el.addEventListener()
```
ie8 不支持 `addEventListener `
两参数
```
/*
事件名 加on
事件处理函数
*/
el.attachEvent()
```
###### 4.2.2.3 `on事件和addEventListener 添加事件的区别`
>元素.on事件 = 处理函数
(添加事件 如果是同名事件，后面的事件处理函数会覆盖前面的)
元素.addEventListener()
(添加事件 不会覆盖之前的同名事件)
###### 4.2.2.4 移除事件
> 用什么方式添加的事件就用什么方式移除事件
- on => null
```
el.onclick = function(){};
// 移除
el.onclick = null;
```
- addEventListener => removeEventListener
`注意:如果addEventListener添加的事件的事件处理函数是匿名函数不能直接移除事件 只能移除命名事件 (ie8的 attachEvent 一样)`
```
el.addEventListener("click",fn1)
// 移除
el.removeEventListener("click",fn1)
```
- attachEvent => detachEvent
```
el.attachEvent("onclick",f1)
// 移除
el.detachEvent("onclick",fn1)
```
##### 4.2.3 事件处理函数
> 用户操作后要执行的什么代码

###### 4.2.3.1 事件对象 e
- 本质也是个对象
- 保存了事件触发时的相关信息
- 在事件处理函数中的形参里写参数 e 
ie有兼容问题
兼容代码`e = e || window.event`
- 三大坐标
>screen
e.screenX 和 e.screenY
获取的是点击的位置相对于屏幕左上角的坐标

>page
e.pageX,e.pageY
获得的是点击的位置相对于页面（文档）左上角的坐标
有兼容性的问题,IE8不支持，自己计算
`pageX = e.clientX + 页面往左滚出去的距离 (scrollLeft)`
`pageY = e.clientY + 页面往上滚出去的距离 (scrollTop)`

>offset
e.offsetX,e.offsetY
获得是是点击的位置相当于事件源的左上角的位置
ie属性 有bug
`offsetX = e.clientX - 盒子到可视区域的left (el.getBoundingClientRect.left)`
`offsetY = e.clientY - 盒子到可视区域的top (el.getBoundingClientRect.top)`
###### 4.2.3.2 this、e.target、e.currentTarget区别
>this 和 e.currentTarget 是一樣的 
当前调用的是谁的事件 那么this就是谁 
e.currentTarget ie8 不支持 => 直接用this

> e.target 获取事件源(目标阶段) 正在触发事件的那个元素
ie8不支持 => e.target
兼容代码
`var target = e.target || e.srcElement`

#### 4.3 事件的三大阶段
##### 4.3.1 如何获取事件正在执行的阶段
> 在事件里可以通过 e.eventPhase 来捕获当前是哪个阶段
捕获 => 1
目标 => 2
冒泡 => 3

##### 4.3.2 三大阶段
`捕获 目标 冒泡`
###### 4.3.2.1 捕获阶段
>一种现象 与冒泡阶段相反 从window开始 依次一级一级往下调用子元素的同名事件，直到找到真正触发事件的事件源
事件捕获默认看不见 想要看到捕获阶段则需要通过 addEventListener来绑定事件，并且第三个参数传true
###### 4.3.2.2 目标阶段
>找到真正触发事件的那个元素 -> 事件源
###### 4.3.2.3 冒泡阶段
> 一种现象 当元素事件触发后 会从事件源开始依次一次一次往上调用父元素的同名事件，直到window
事件冒泡默认存在

`好处`：给父元素添加事件相当于给子元素添加了事件 -> 事件委托 -> e.target
`带来的影响(坏处)`：如果子元素和父元素有同名事件 并且业务逻辑相反 则会冲突影响
##### 4.3.3 阶段顺序
`设置捕获`
>先捕获 从window开始 依次一级一级调用子元素的同名事件
-> 找到目标(真正触发事件的元素)
>-> 从目标元素开始 依次一级一级调用父元素的同名事件 直到window

`未设置捕获`
> 找到目标(真正触发事件的元素)
-> 从目标元素开始 依次一级一级调用父元素的同名事件 直到window
##### 4.3.4 阻止事件派发
*阻止事件冒泡和阻止事件捕获*
`e.stopPropagation()`
ie8魔鬼不支持(ie8只有事件冒泡，没有事件捕获) => `e.cancelBubble = true`
---
## 五、本地存储
只能存储字符串
查看本地存储 `浏览器F12->Application->Local || Session Storage->fille://`
### 5.1 localStorage 本地存储
>可以把数据存储到本地(浏览器) 只要用户不删除 则会一直保存 每个域名都是独立的保存数据 不同域名不能互相访问 长久保存数据可以存储到 localStorage
- 保存数据 `localStorage.setItem(key,value)`
- 获取数据 `localStorage.getItem(key)` => 如果没有这个数据 则返回 `null`
- 删除一个数据 `localStorage.removeItem(key)`
- 清空所有数据 `localStorage.clear()`
### 5.2 sessionStorage 会话存储
> 短暂存储数据 可以多页面传值 相当于localStorage会更安全 浏览器关闭后就不会保存了
- 保存数据 `sessionStorage.setItem(key,value)`
- 获取数据 `sessionStorage.getItem(key)` => 如果没有这个数据 则返回 `null`
- 删除一个数据 `sessionStorage.removeItem(key)`
- 清空所有数据 `sessionStorage.clear()`
----
## 六、定时器和延时器
*给元素添加动画定时器 可以将定时器id直接给元素 `元素.timeId`*
### 6.1 定时器
>每隔一段时间执行一段代码
```
/*
参数一: 要执行的代码 可以写字符串 在字符串里面写js代码 或者写函数
参数二: 间隔事件 单位是毫秒 1000毫秒 = 1秒
*/
window.setInterval()
```
### 6.2 延时器
>一段时间后执行一段代码
```
/*
参数一: 要执行的代码 可以写字符串 在字符串里面写js代码 或者写函数
参数二: 延迟事件 单位是毫秒 1000毫秒 = 1秒
*/
window.setTimeout()
```
### 6.3 清除定时器和延时器
```
clearInterval(定时器id)
clearTimeout(延时器id)
```
---
## 七、阻止a标签默认跳转三种方式
### 7.1 href
`将a标签的href的路径改为 javascript:void(0) || javascript:void(null) || 简写 javascript:`
### 7.2 return
`给a标签添加点击事件 在事件处理函数的最后 return false`
### 7.3 事件对象 e
`阻止事件默认行为 e.preventDefault()`
>return和事件对象e阻止跳转的区别
return后面的代码不执行
e.preventDefault()不会影响后面代码执行
---
## 八、排他思想
> 先把大家恢复成默认，再让自己特殊 ` tab切换 `
---
## 九、offset家族
*只能获取行内样式*
> 只能取值 (number) 不能赋值
offsetWidth 和 offsetHeight 获取包括padding和border和width||height 
### 9.1 offsetWidth
`获取盒子的最终宽度`
### 9.2 offsetHeight
`获取盒子的最终高度`
### 9.3 offsetTop
`获取自身上外边框到定位父级上内边框的距离`
### 9.4 offsetLeft
`获取自身左外边框到定位父级左内边框的距离`
![](https://upload-images.jianshu.io/upload_images/12946880-a420cc3504d29078.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
## 十、scroll家族
> scrollWidth 和 scrollHeight 获取的包括了隐藏的部分 只能获取(number)不能修改
scrollLeft 和 scrollTop  可以获取也可以修改 想要滚动条滚动到最右变 直接赋值为 scrollWidth即可 
### 10.1 scrollWidth
`获取盒子内容的总宽度`
### 10.2 scrollHeight
`获取盒子内容的总高度`
### 10.3 scrollTop
`获取内容上边滚出去的距离`
### 10.4 scrollLeft
`获取内容左边滚出去的距离`

*scrollTop和scrollLeft有兼容问题*
*兼容代码*
```
var scrollTop = window.pageYOffset 
|| document.documentElement.scrollTop 
|| document.body.scrollTop 
|| 0;
```
```
var scrollLeft = window.pageXOffset 
|| document.documentElement.scrollLeft 
|| document.body.scrollLeft 
|| 0;
```
![](https://upload-images.jianshu.io/upload_images/12946880-a20c414ee0057481.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

----
## 十一、client家族
`如果元素有滚动条 那么这个元素的可视宽度就是 整个盒子的宽度 - 滚动条的宽度`
### 11.1 clientWidth
`获取可视区域的宽`
### 11.2 clientHeight
`获取可视区域的高`
### 11.3 clientTop
`获取左边框的宽度`
### 11.4 clientLeft
`获取上边看的宽度`

----
## 十二、操作元素的方式
### 12.1 通过id获取元素
*如果没有这个元素 则返回null 有则返回一个对象*
> 获取 id 只能通过document来获取

`document.getElementById("id")`
### 12.2 样式名获取元素
*如果没有这个元素 则返回一个空集合[伪数组]*
`document.getElementsByClassName('class')`
#### 12.2.1 什么是伪数组?
- 有下标索引、有元素、有长度 但是没有数组中的方法
### 12.3 通过标签名来获取元素
*ie8魔鬼有兼容问题*
`document.getElementsByTagName("div")`
### 12.4 通过name属性来获取元素(表单元素)
`document.getElementsByName("name")`
### 12.5 通过css选择器来获取元素 (h5新增)
- 获取的是一个对象 如果匹配到多个元素则返回第一个元素
`document.querySelector(selectors)`
- 获取的是一个伪数组
`document.querySelectorAll(selectors)`
### 12.6 获取文档
`document`
### 12.7 获取html
`document.documentElement`
### 12.8 获取head
`document.head`
### 12.9 获取body
`document.body`
### 12.10 获取子节点和子元素
- 子节点
`el.childNodes`
- 子元素
`el.childNodes`
### 12.11 获取父节点和父元素
*区别*
>el.parentNode可以获取到document
el.parentElement不能获取到document
因为document不是一个元素
- 父节点
`el.parentNode`
- 父元素
`el.parentNode`
### 12.12 获取上一个兄弟节点和上一个兄弟元素
- 找到上一个兄弟节点(文本、注释、标签)，所有浏览器都支持
`el.previousSibling`
- 找到上一个兄弟元素(只会找到元素)，IE9以下都不支持
`el.previousElementSibling`
### 12.13 获取下一个兄弟节点和下一个兄弟元素
- 找到下一个兄弟节点(文本、注释、标签)，所有浏览器都支持
`el.nextSibling`
- 找到下一个兄弟元素(只会找到元素)，IE9以下都不支持
`el.nextElementSibling`
### 12.14 添加子元素
`父元素.appendChild("子元素")`
### 12.15 删除子元素
`父元素.removeChild("子元素")`
### 12.16 在某个子元素前面插入元素
`父元素.insertBefore(插入的新元素,要在哪个元素前面插入)`
### 12.17 替换子元素
`父元素.replaceChild(新的元素,被替换的元素)`

----
## 十三、操作属性的另二种方式
### 13.1 js 操作属性
*既可以操作自带属性，也可以操作自定义属性*
- 获取属性
`el.getAttribute("属性名")`
- 修改属性
`el.setAttribute("属性名","属性值")`
- 删除属性
 `el.removeAttribute("属性名")`

### 13.2 h5 操作属性
>在自定义属性前面加 data-
如果自定义属性前面添加了 data- 
那么这些自定义属性都会保存到el.dataset '对象' 里面
想要取得属性直接遍历对象即可
取值时 data-会被去掉 并且如果后面还有- 
会把后面的-也去掉 并把-后面的首字母大写(驼峰命名法)
- 获得属性
`el.dataset["shengao"]`
`e.dataset.shengao`
- 修改属性
`el.dataset["shengao"] = "123cm"`
- 删除属性
`delete el.dataset["shengao"]`
--- 
## 十四、创建元素的三种方式
- document.write()
>只能在body添加元素，并且会覆盖之前页面中的元素
- document.createElement()
>创建一个标签存在内存里面
>再通过 父元素.appendChild(“创建出来的元素”) 渲染到页面
>appendChild细节
>给父元素追加一个元素，添加在最后，如果此元素已经存在，则被移动到最后
- el.innerHtml()
>为某元素添加内容，会覆盖原来的内容
+= 就只会追加不会覆盖
每次innerHtml赋值(+=)都是重新渲染，
所以有可能会让之前的元素丢失，
还会让之前元素的事件丢失(事件委托可解决)
渲染耗性能，大量资源浪费
`先拼接字符串 再循环完了后一次性追加到页面中`
----
## 十五、修改元素的属性
### 15.1 操作元素样式中的属性
- 获取
`元素.style.样式名`
- 修改
`元素.style.样式名 = 样式值`
*注意*
>样式名 如果在css 有 "-" 的 应使用 驼峰命名法 
`background-color => backgroundColor`
### 15.2 操作图片的路径
- 获取
`元素.scr`
- 修改
`元素.scr = "路径"`
### 15.3 操作单标签按钮的文字
- 获取
`元素.value`
- 修改
`元素.value = "值"`
### 15.4 操作元素的类名
#### 15.4.1 JavaScript
- 获取
`元素.className`
- 添加
`元素.className += " class"`
*注意 用+= 得加空格*
#### 15.4.2 HTML5
> el.classList 返回的是一个伪数组 保存的是元素上的所有类名
- 添加一个类
`el.classList.add("class")`
- 删除一个类
`el.classList.remove("class")`
- 修改一个类
`el.classList.replace("被替换的类","要替换的新类")`
- 切换一个类
`el.classList.toggle("class")`
*没有就添加这个类 有就删除这个类*
- 判断一个类是否存在
`el.classList.contains("class")`
*存在返回true 不存在返回false*
### 15.5 操作元素显示or隐藏
- 显示
`元素.style.displaye = "block"`
- 隐藏
`元素.style.displaye = "none"`
*如果想要通过一个按钮切换显示隐藏可以声明一个flag 记录显示和隐藏的状态*
>let flag = this.value == "隐藏" ? true : false
divBox.style.display = flag ? "none" : "block"
this.value = flag ? "显示" : "隐藏" // 这里用来下一次
点击再次toggle 所以需要与设置的flag相反设定对应的值

### 15.6 表单元素的禁用和启动
- el.style.disabled = true
>加上代表禁用 不加上代表启动
js里面设置它为 true 代表禁用、false 代表启用

### 15.7 操作双标签的文字
- 获取
`元素.innerText`
- 修改
`元素.innerText = "值"`
----
## 十六、innerHTML、innerText、textContent的区别
>innerHTML没有兼容问题 既可以拿到文本也可以渲染标签

> innerText和textContent都是设置标签里面的文本内容 
将数据当成字符串输出到页面 不会渲染
innerText在老版火狐里面不支持
textContent在ie9以下都不支持
----
## 十七、表单元素属性
- value
`可以获取大部分表单元素的内容(option除外)`
- type
`可以获取表单元素的类型`
- disabled
`禁用属性`
- checked
`复选框选中`
- selected
`下拉框选中`

----
## 十八、获取元素的最终样式
```
window.getComputedStyle(element)["width"]
window.getComputedStyle(element).width
```
返回的string属性值 "100px"
```
/*ie8魔鬼专用*/
element.currentStyle['width']
```
---
## 十九、获取元素到可视区域的距离
```
element.getBoundingClientRect()
element.getBoundingClientRect().left
element.getBoundingClientRect().top
```

*获取鼠标位置相对于自身的x和y*
(offsetX和offsetY有bug)
`e.clientX - 盒子到可视区域的left`
`e.clientY - 盒子到可视区域的top`

---
## XMind思维导图
![webAPI.png](https://upload-images.jianshu.io/upload_images/12946880-a59e622e50b2a619.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

