---
title: 百度搜索
---
案例使用了 本地存储历史记录 搜索联想(纯前端模拟数据 未使用数据库 后续demo会使用～) 

###### JavaScript代码几乎每行都有注释 方便阅读理解思路
`github demo 网址 => ` [https://github.com/LanHai1/search-baidu](https://github.com/LanHai1/search-baidu)
![](https://upload-images.jianshu.io/upload_images/12946880-5c8b72477e75d787.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 思路
- 输入搜索框 注册弹起键盘事件
判断是否文本框为空
清空之前联想的数据
return出去
可替换成调用历史提示记录
- 先清空之前联想的数据
- 循环遍历联想数据数组
判断是否存在联想数据
indexOf() > -1
将数据通过创建元素追加到页面中去
鼠标移入高亮
移出恢复原来的背景颜色
- search按钮注册点击事件 保存历史记录至本地
先获取本地数据 
默认值 ""
判断用户是否未输入
判断用户输入的数据是否已经存在
return
三元表达式判断是否有数据
逗号拼接数据
将新数据存放回本地储存
- 历史记录提示
给文本框注册获取光标事件
先清空之前的历史记录提示
获取本地数据
默认值 ""
字符串方法 split(",") 逗号切割字符串
用变量接收 返回数组
循环遍历数组
将数据通过创建元素追加到页面中去
鼠标移入高亮
移出恢复原来的背景颜色
注册鼠标点击事件
将当前li的值给文本框
给文本框注册失去光标事件
设置延时器
因为失去光标事件优先执行在点击事件前面
清空历史记录

#### xmind图释
![](https://upload-images.jianshu.io/upload_images/12946880-5c975b5fbbc91b97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`结构 样式`
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            padding: 0;
            margin: 0;
        }
        
        input {
            outline: none;
            box-sizing: border-box;
            height: 40px;
            position: absolute;
            z-index: 9;
        }
        
        .clearfix {
            *z-index: 1;
        }
        
        .clearfix::after {
            content: "";
            display: block;
            overflow: hidden;
            clear: both;
            visibility: hidden;
        }
        
        .cont {
            width: 641px;
            margin: 300px auto;
            position: relative;
        }
        
        .fl {
            float: left;
        }
        
        .fr {
            float: right;
        }
        
        #txt {
            width: 537px;
            padding: 10px 80px 10px 7px;
        }
        
        #search {
            width: 104px;
            -webkit-appearance: button;
            -moz-appearance: button;
            line-height: 40px;
            font-size: 16px;
            cursor: pointer;
            color: #000;
            border-left: 0;
            background-image: linear-gradient(135deg, #fff8f8, #d3d3d3);
            right: 0;
        }
        
        .ul_search {
            list-style: none;
            width: 639px;
            display: none;
            border: 1px solid #b6b6b6;
            border-top: 0;
            margin-top: -1px;
            position: absolute;
            top: 40px;
        }
        
        .ul_search>li {
            padding: 0 10px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            height: 20px;
            line-height: 20px;
            font-size: 14px;
            cursor: default;
        }
    </style>

</head>

<body>
    <div class="cont clearfix">
        <input type="text" id="txt" class="fl">
        <input type="button" value="百度一下" id="search" class="fl">
        <ul class="ul_search fl" id="ul_box"></ul>
    </div>
    <script src="index.js"></script>
</body>

</html>
```
`JavaScript代码`
```
/**
 * 获取id
 * @param {string} id 
 */
function $(id) {
    return document.getElementById(id)
}

// 联想数据
var keywords = ["林利群", "林利群为什么很黑", "林利群的经纪人是周林林吗", "林利群是谁", "广东人", "广东人爱吃", "广东人爱吃福建人", "林丹的生平", "JavaScript",
    "Java", "王思聪", "王健林", "社会王", "隔壁老王", "林绿群", "你打球像蔡徐坤", 'aaa', 'bbb', '王祖蓝', '你打球王祖蓝'
];

// 键盘弹起事件
$("txt").onkeyup = function() {
    // 每次弹起之前先清空之前的联想
    $("ul_box").innerHTML = ""

    // 为空直接跳出函数
    if (this.value.length == 0) {
        // 隐藏ul
        $("ul_box").style.display = "none"
        return
    }

    // 循环遍历 判断是否存在联想
    for (let i = 0; i < keywords.length; i++) {
        // indexOf => 为空"" 返回0 待处理
        if (keywords[i].indexOf(this.value) != -1) {
            // 存在联想 直接渲染页面
            renderUl(keywords[i])
        }
    }
}

/**
 * 渲染ul下的li
 * @param {array_string} arrString 
 */
function renderUl(arrString) {
    let li = document.createElement("li")
    li.innerHTML = arrString

    // 将li追加至ul里面
    $("ul_box").appendChild(li)

    // 显示ul
    $("ul_box").style.display = "block"
        // 高亮
    ul_li_syle(li)

    // 点击 li的值 赋值给 txt
    // 失去光标优先级大于点击 会先执行 
    // 失去光标的时候清空了ul里面的内容 所以这个时候就获取不到数据了
    // 解决方案 给失去光标延时器
    li.onclick = function() {
        $("txt").value = this.innerHTML
    }
}

/**
 * 高亮
 * @param {element} el 
 */
function ul_li_syle(el) {
    // 高亮
    el.onmouseover = function() {
        this.style.backgroundColor = "#cccccc96"
    }
    el.onmouseout = function() {
        this.style.backgroundColor = "#fff"
    }
}

// 本地存储 -> 历史记录

// 搜索后保存历史记录至本地
$("search").onclick = function() {
    // 为空不保存
    if ($("txt").value.length == 0) {
        $("ul_box").style.dispaly = "none"
        return
    }
    // 获取数据 => 进行拼接
    let old_data = localStorage.getItem("search_baidu") || ""

    // 判断数据是否已经存在本地
    if (old_data.indexOf($("txt").value) > -1) {
        return
    }

    // 数据逗号拼接
    // 判断是否右数据 拼接处理
    let new_data = old_data ? `${old_data},${$("txt").value}` : $("txt").value

    // 保存数据
    localStorage.setItem("search_baidu", new_data)
}

// 获取光标 历史记录提示
$("txt").onfocus = function() {
    // 获取数据
    let data = localStorage.getItem("search_baidu")

    // 判断是否已经有历史记录
    if (data == null) {
        return
    }
    // 逗号处理
    let arr_data = data.split(",").reverse()
    for (let i = 0; i < arr_data.length; i++) {
        renderUl(arr_data[i])
    }
}

// 失去光标 历史记录移除
$("txt").onblur = function() {
    // 延迟执行 失去光标的优先级大于点击事件
    setTimeout(() => {
        $("ul_box").innerHTML = ""
        $("ul_box").style.display = "none"
    }, 200)
}
```


