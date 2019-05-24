---
title: slider详细步骤注释
---

##### 在JavaScript代码里几乎每行都写了注释 方便新学者入门～ 也方便我回顾🙂️
`github demo ->` [https://github.com/LanHai1/slider](https://github.com/LanHai1/slider)
![](https://upload-images.jianshu.io/upload_images/12946880-4c5cc103e4a32411.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


----
#### 动画移动元素
思路:
- 执行动画之前先清除动画定时器id=>(避免用户多次点击动画速度变快)
将定时器id给每个移动元素 => el.timeId => 如果直接给有可能会造成停止一个元素动画影响其他元素
- 先获取移动动画的当前位置
- 判断每次是正走10 还是反走10 => 10||-10
- 判断元素在运动的当前位置是否还能继续走10 不能走就直接闪现到目标位置
绝对值(目标位置-当前位置)>10 绝对值是因为有可能是正走或者反走 但是每次走的距离是一样的
- 设置元素的位置
-  判断是否已经到达目标值 清理移动定时器
##### xmind 动画移动元素
![](https://upload-images.jianshu.io/upload_images/12946880-a1706ae07be8a2bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

----

#### 无缝轮播图
思路
- 鼠标移入轮播区域盒子 显示隐藏 箭头
- 声明一个与图片同步的 索引 index =声明一个与页码同步的 索引 pageIndex = 0
- 克隆轮播第一张图在最后 => 无缝轮播
- 右点击箭头注册点击事件
判断是否是当前轮播是否是最后一张图(克隆过来的第一张图)
对应索引值index 和 整个ul(轮播图片)的children的长度 - 1 判断
直接闪现到第一张图
index = 0
再次调用动画函数
每次点击 index++
每次点击 pageIndex++
判断当前页码是否到了最后一个
pageIndex == 整个ul(轮播图片)的children的长度 - 1
pageIndex = 0
直接闪现到第一个页码
调用设置对应页码功能函数
传入页码li 和 pageIndex
调用设置对应页码功能函数
传入页码li 和 pageIndex
调用动画移动元素
传入移动的元素
传入移动的目标位置
-index*轮播外盒子=>轮播一张图片的宽度
取负值 左箭头和右箭头 都是往右移动
注意 这个🉐️需要在每次点击index++ 之前做判断
- 左箭头注册点击事件
判断当前轮播是否是第一张图片 并且继续往左点击
对应索引值index==0
直接闪现到对应的最后一张图
index = 整个ul的children的长度 - 1
再次调用动画函数
和右箭头一样 不同的是每次点击 index-- 和 pageIndex--
判断当前页码是否是一个 并且继续往左点击
pageIndex == -1
pageIndex =  整个ul(轮播图片)的children的长度 - 2
调用设置对应页码功能函数
传入页码li 和 pageIndex
注意 这个🉐️需要在每次点击index-- 之前做判断
- 设置对应页码功能
循环变量每个页码li
给每个li设置一个自定义属性 存放对应的索引值
为每个li设置点击事件
index = 当前li的自定义属性=>存放的索引值
调用动画函数
调用设置页码排他样式的函数
- 设置对应页码排他样式
循环遍历 进行排他 清除所有li的点击高亮的样式
为当前点击的li设置高量的样式
- 设置定时器定时轮播
声明一个变量定时器
页码加载开始 直接开启一个定时器
setInterval(封装右箭头点击函数,3000)
鼠标移入轮播区域关闭轮播定时器
鼠标移出开启一个 轮播定时器
##### xmind 无缝轮播图
![](https://upload-images.jianshu.io/upload_images/12946880-d1cb3b3299ad2d1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 无缝轮播图 图解
![](https://upload-images.jianshu.io/upload_images/12946880-d4756cef96b030ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`样式结构`
```
<!DOCTYPE html>
<html lang="en">

<head>

    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {
            padding: 0;
            margin: 0;
            list-style: none;
            border: 0;
        }
        
        .all {
            width: 500px;
            height: 200px;
            padding: 7px;
            border: 1px solid #ccc;
            margin: 100px auto;
            position: relative;
        }
        
        .screen {
            width: 500px;
            height: 200px;
            overflow: hidden;
            position: relative;
        }
        
        .screen li {
            width: 500px;
            height: 200px;
            overflow: hidden;
            float: left;
        }
        
        .screen ul {
            position: absolute;
            left: 0;
            top: 0;
            /*要足够宽才放到一行里*/
            width: 3000px;
        }
        
        .all ol {
            position: absolute;
            right: 10px;
            bottom: 10px;
            line-height: 20px;
            text-align: center;
        }
        
        .all ol li {
            float: left;
            width: 20px;
            height: 20px;
            background: #fff;
            border: 1px solid #ccc;
            margin-left: 10px;
            cursor: pointer;
        }
        /*点到哪个页码就让哪个页码加一个current类就变黄了*/
        
        .all ol li.current {
            background: yellow;
        }
        
        #arr {
            display: none;
        }
        
        #arr span {
            width: 40px;
            height: 40px;
            position: absolute;
            left: 5px;
            top: 50%;
            margin-top: -20px;
            background: #000;
            cursor: pointer;
            line-height: 40px;
            text-align: center;
            font-weight: bold;
            font-family: '黑体';
            font-size: 30px;
            color: #fff;
            /*透明度：这个是指全透明*/
            opacity: 0.3;
            border: 1px solid #fff;
        }
        
        #arr #right {
            /*距离父元素右边5像素*/
            right: 5px;
            /*代表把left的值清空（设置为默认值）*/
            left: auto;
        }
    </style>
</head>


<body>

    <div class="all" id='box'>

        <!-- 显示图片的区域 -->
        <div class="screen">
            <ul>
                <li><img src="images/1.jpg" width="500" height="200" /></li>
                <li><img src="images/2.jpg" width="500" height="200" /></li>
                <li><img src="images/3.jpg" width="500" height="200" /></li>
                <li><img src="images/4.jpg" width="500" height="200" /></li>
                <li><img src="images/5.jpg" width="500" height="200" /></li>
            </ul>
            <!-- 页码 -->
            <ol>
                <li class="current">1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
            </ol>

        </div>

        <!-- 左右箭头那一部分 -->
        <div id="arr"><span id="left">&lt;</span><span id="right">&gt;</span></div>
    </div>
    <script src="index.js"></script>

</body>

</html>
```
`行为 JavaScript代码`
```
// 通过id获取元素
function $(id) {
    return document.getElementById(id)
}

// 最外层盒子
let box = $("box")

// 箭头盒子
let arr_box = $("arr")

// 左箭头
let left_arr = $("left")

// 右箭头
let right_arr = $("right")

// 轮播图片区域宽度
let screenWidth = document.querySelector(".screen").offsetWidth

// 轮播ul
let ul_banner = document.querySelector(".screen > ul")

// 页码
let ol_banner_current = document.querySelectorAll(".screen > ol > li")

// 克隆轮播第一张图片 => 无缝轮播
ul_banner.appendChild(ul_banner.children[0].cloneNode(true))

//定时器
let timeId
timeId = setInterval(nextPage, 3000)

// 鼠标移入移除显示隐藏 箭头
box.onmouseover = function() {
    // 清除定时器 用户操作
    clearInterval(timeId)
    arr_box.style.display = "block"
}
box.onmouseout = function() {
    // 继续轮播 开启定时器
    timeId = setInterval(nextPage, 3000)
    arr_box.style.display = "none"
}

// 左右箭头事件 下一张 上一张
// 图片索引值
let index = 0

// 页码索引
let pageIndex = 0
right_arr.onclick = function() {
    nextPage()
}
left_arr.onclick = function() {
    // 判断是否是第一张 并且继续往左移动
    if (index == 0) {
        // 直接闪现到图片的倒数第二张(克隆前最后一张)
        index = ul_banner.children.length - 1
        ul_banner.style.left = `${-index*screenWidth}px`
    }
    // 索引每次减一
    index--
    pageIndex--
    // 判断是否页码在第一个 并且继续往左走 => 直接去最后一个页码
    if (pageIndex == -1) {
        pageIndex = ul_banner.children.length - 2
        setPageClass(ol_banner_current, pageIndex)
    }
    setPageClass(ol_banner_current, pageIndex)
    bannerAnimation(ul_banner, -index * screenWidth)
}

// 页码功能
for (let i = 0; i < ol_banner_current.length; i++) {
    // 设置自定义属性 存放索引
    ol_banner_current[i].setAttribute("index", i)
        // 设置点击事件 触发排他设置页码样式
    ol_banner_current[i].onclick = function() {
        // 轮播区域跟着页码点击一起移动
        index = pageIdex =  +this.getAttribute("index")
        bannerAnimation(ul_banner, -index * screenWidth)
        setPageClass(ol_banner_current, +this.getAttribute("index"))
    }
}

/**
 * 设置页码样式
 * @param {*} el 页码
 * @param {*} thisIndex 高亮的元素索引
 */
function setPageClass(el, thisIndex) {
    // 循环排它
    for (let i = 0; i < el.length; i++) {
        el[i].removeAttribute("class")
    }
    // 设置高亮
    el[thisIndex].className = "current"
}

/**
 * 移动动画
 * @param {element} el 移动的元素
 * @param {number} target 目标位置
 */
function bannerAnimation(el, target) {
    // 设定定时器前先清理定时器
    clearInterval(el.timeId)

    // 将定时器id给元素里面的属性 定时器id唯一化 不会影响其他移动元素
    el.timeId = setInterval(() => {
        // 元素当前位置
        let count = el.offsetLeft

        // 每次走10 判断左走还是右走
        count += target > count ? 10 : -10

        // 判断是否还能继续走10 不能直接跳到目标位置 取绝对值 可能是负数
        count = Math.abs(target - count) > 10 ? count : target

        // 设置元素位置
        el.style.left = `${count}px`

        // 达到目标位置 结束定时器 => 结束动画
        if (target == count) {
            clearInterval(el.timeId)
        }
    }, 20)
}

/**
 * 下一张 => 定时轮播
 */
function nextPage() {
    // 判断是否是最后一张 => 闪现到第一张 => 无缝轮播
    if (index == ul_banner.children.length - 1) { // 5 = 5
        // 直接闪现到第一张图片即可
        index = 0
        ul_banner.style.left = `${-index*screenWidth}px`
    }
    // 索引每次加一
    index++
    // 页码索引加一
    pageIndex++
    // 判断是否页码到了最后一个 => 直接去第一个页码
    if (pageIndex == ul_banner.children.length - 1) {
        pageIndex = 0
        setPageClass(ol_banner_current, pageIndex)
    }

    // 页码样式
    setPageClass(ol_banner_current, pageIndex)

    // 调用动画函数
    bannerAnimation(ul_banner, -index * screenWidth)
}
```

