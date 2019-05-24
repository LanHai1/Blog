---
title: animation动画
---

做了个类似jquery的动画函数封装
`gitDemo => `[https://github.com/LanHai1/animation](https://github.com/LanHai1/animation)
![](https://upload-images.jianshu.io/upload_images/12946880-5f2fe63703015a6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 思路
- 三参数
		el 动画元素
		attrs 动画属性{key:value}
		callback 回调函数
- 1- 每次调用前先清除动画定时器 el.timeId
- 2- 开启定时器
- 3- 声明一个flag 属性是否全部到达目标点
- 4- forin attrs
		判断key 是否是透明度 处理
			获取元素当前状态 * 100
			元素目标状态 * 100
			每次执行步数
				(目标位置-当前位置)/10
					目标大于当前 向上取整 Math.ceil
					目标小于当前 向下取整 Math.floor
				当前位置 += 每次执行步数
			当前位置 / 100
			设置元素样式
			判断是否还有值未到达目标点
				(target / 100 != current)
- flag = false
		其他属性处理
			获取元素当前状态 => 取整
			元素目标状态
			每次执行步数
				(目标位置-当前位置)/10
					目标大于当前 向上取整 Math.ceil
					目标小于当前 向下取整 Math.floor
				当前位置 += 每次执行步数
			设置元素样式
			判断是否还有值未到达目标点
				(target  != current)
- flag = false
- 5- 判断是否全部到达目标点
		清除动画定时器 el.timeId
		判断callback是否是函数
			调用callback
#### xmind图解
![](https://upload-images.jianshu.io/upload_images/12946880-86d67db50215f532.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
`样式`
```
        .box {
            width: 100px;
            height: 100px;
            background-color: pink;
            position: absolute;
        }
        
        .box1 {
            width: 200px;
            height: 200px;
            background-color: hotpink;
            position: absolute;
        }
```
`结构`
```
    <input type="button" value="点击400" id="btn1">
    <input type="button" value="点击800" id="btn2">
    <div class="box"></div>
    <div class="box1"></div>
```
`行为`
```
let box = document.querySelector(".box");
let box1 = document.querySelector(".box1");

/**
 * 根据id获取元素
 * @param {string} id 
 */
let $ = id => document.getElementById(id);

$("btn1").onclick = function() {
    animation_s(box, {
        width: 300,
        height: 600
    }, () => {
        animation_s(box, {
            left: 100,
            opacity: 0.3
        }, () => {
            animation_s(box1, {
                left: 500,
                height: 10
            })
        })
    })
}
$("btn2").onclick = () => {
    animation_s(box1, {
        left: 500,
        height: 10
    })
}

/**
 * 缓动动画
 * @param {element} el 
 * @param {attributeJson{key:value}} attrs 
 * @param {fn} callback 
 */
function animation_s(el, attrs, callback) {
    // 每次调用前先清除已有的定时器
    clearInterval(el.timeId);
    el.timeId = setInterval(() => {
        // 全部到达目标点
        let flag = true;
        for (const key in attrs) { // 还需考虑其他属性的处理 如background-color...
            if (key == "opacity") { // 透明度处理
                // 元素当前状态
                let current = +(window.getComputedStyle(el)[key]) * 100;
                // 元素目标状态
                let target = attrs[key] * 100;
                // 每次执行步数
                // 目标大于当前 向上取整 => 目标小于当前 向下取整
                let temp = target > current ? Math.ceil((target - current) / 10) : Math.floor((target - current) / 10);
                current += temp;
                current = current / 100;
                // 设置元素样式
                el.style[key] = `${current}`;
                // 判断是否全部到达目标点
                if (target / 100 != current) {
                    flag = false;
                }
            } else {
                // 元素当前状态
                let current = parseInt(window.getComputedStyle(el)[key]);
                // 元素目标状态
                let target = attrs[key];
                // 每次执行步数
                // 目标大于当前 向上取整 => 目标小于当前 向下取整
                let temp = target > current ? Math.ceil((target - current) / 10) : Math.floor((target - current) / 10);
                current += temp;
                // 设置元素样式
                el.style[key] = `${current}px`;
                // 判断是否全部到达目标点
                if (target != current) {
                    flag = false;
                }
            }
        }
        if (flag) {
            clearInterval(el.timeId);
            // 回调函数
            if (callback instanceof Function) {
                callback();
            }
        }
    }, 20)
}
```



