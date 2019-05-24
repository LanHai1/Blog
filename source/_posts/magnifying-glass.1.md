---
title: magnifying-glass 放大镜
---
本篇讲解仿京东 鼠标移入商品 放大显示

🤪
![效果图](https://upload-images.jianshu.io/upload_images/12946880-d9fb00e800fbcc04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
`git demo => `[https://github.com/LanHai1/magnifying-glass](https://github.com/LanHai1/magnifying-glass)
#### 思路

- 1- 鼠标移入移出小图盒子 显示和隐藏 mask阴影层 和 大图盒子
- 2- 为小图盒子添加鼠标移动事件
2-1- 为事件对象e做兼容处理
2-2- 获取鼠标相对于小盒子里面的x和y
2-2-1- offsetX和offsetY 有bug ，自己计算
2-2-2- x = e.clientX - 盒子自身到可视区域的left
2-2-3- y = e.clientY - 盒子自身到可视区域的top 
2-2-4- 鼠标在阴影层的中间 
所以最后计算出来的x和y还要减去 mask阴影层的宽高各一半
2-3- 获取阴影盒子的最大和最小移动范围
2-3-1- 小盒子宽度 - 阴影盒子的宽度
2-3-2- 小盒子高度 - 阴影盒子的高度
2-4- 判断最大值和最小值
2-4-1- 最小值
`x = x > 0 ? x : 0;`
2-4-2- 最大值
`y = y < maxY ? y : maxY;`
2-5- 将值给mask阴影层的left和top
2-6- 实现大图跟着鼠标移动
2-6-1- 算出计算比例
2-6-1-1- 大图片的宽度 / 小图片的宽度
2-6-2- 设置大图的top和left
2-6-2-1- 移动的x值乘计算出来的x比例
#### xmind图释
![](https://upload-images.jianshu.io/upload_images/12946880-a7901c9a10a6cd6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
`样式`
```
        * {
            margin: 0;
            padding: 0;
        }
        
        .box {
            width: 350px;
            height: 350px;
            margin: 100px;
            border: 1px solid #ccc;
            position: relative;
        }
        
        .big {
            width: 400px;
            height: 400px;
            position: absolute;
            top: 0;
            left: 360px;
            border: 1px solid #ccc;
            overflow: hidden;
            display: none;
        }
        
        .mask {
            width: 175px;
            height: 175px;
            background: rgba(255, 255, 0, 0.4);
            position: absolute;
            top: 0;
            left: 0;
            cursor: move;
            display: none;
        }
        
        .small {
            position: relative;
        }
        
        .box img {
            vertical-align: top;
        }
        
        #bigBox>img {
            position: absolute;
        }
```
`结构`
```
    <div class="box" id="box">

        <div class="small" id="smallBox">
            <img id="smallImg" src="images/001.jpg" width="350" alt="" />

            <div class="mask" id="mask"></div>
        </div>

        <div class="big" id="bigBox">
            <img id="bigImg" src="images/0001.jpg" width="800" alt="" />
        </div>

    </div>
```
`行为`
```
/**
 * 根据id获取元素
 * @param {*} id 
 */
let $ = id => document.getElementById(id);

// 鼠标移入移出显示隐藏 mask阴影和大图
$("smallBox").onmouseover = function() {
    $("mask").style.display = $("bigBox").style.display = "block";
};
$("smallBox").onmouseout = function() {
    $("mask").style.display = $("bigBox").style.display = "none";
};

// 为小盒子添加鼠标移动事件
$("smallBox").onmousemove = function(e) {
    // 事件对象e做兼容
    e = e || window.event;
    // 获取鼠标相对于小盒子里面的x和y
    // offsetX和offsetY有bug 自己计算 
    // x = e.clientX - 盒子自身到可视区域的left
    // y = e.clientY - 盒子自身到可视区域的top 
    // 因为想要鼠标在阴影层的中间 所以减去阴影盒子宽高的一半
    let x = e.clientX - $("smallBox").getBoundingClientRect()["left"] - $("mask").offsetWidth / 2;
    let y = e.clientY - $("smallBox").getBoundingClientRect()["top"] - $("mask").offsetHeight / 2;

    // 获取阴影盒子最大和最小区域范围
    // 小盒子宽度 - 阴影盒子的宽度
    let maxX = $("smallBox").offsetWidth - $("mask").offsetWidth;
    let maxY = $("smallBox").offsetHeight - $("mask").offsetHeight;

    // 最小值
    x = x > 0 ? x : 0;
    y = y > 0 ? y : 0;

    // 最大值
    x = x < maxX ? x : maxX;
    y = y < maxY ? y : maxY;

    setXY($("mask"), {
        left: x,
        top: y
    })

    // 大图跟着移动
    // 计算比例 大图片的宽度 / 小图片的宽度
    let xProportion = $("bigImg").offsetWidth / $("smallImg").offsetWidth;
    let yProportion = $("bigImg").offsetHeight / $("smallImg").offsetHeight;

    // 设置对应值 移动的x值乘计算出来的x比例
    setXY($("bigImg"), {
        left: -x * xProportion,
        top: -y * yProportion
    })
};
/**
 * 设置元素的left 和 top
 * @param {element} el 
 * @param {json} attrs 
 */
function setXY(el, attrs) {
    for (const key in attrs) {
        el.style[key] = `${attrs[key]}px`;
    }
}
```