---
title: html5+css3+基础JavaScript语法题
---
一些很重要的点 都通过注释写在了代码里面
- 求以下的i输出多少?
```
        var i = 10; // 全局作用域的this 是window
        function demo() {
            i = 20; // 函数中未写关键字直接赋值 i为全局变量 
            for (var i = 0; i < 10; i++) { // 执行 10 次 当第十次的时候 i = 10 
            //进行条件判断为false 跳出循环 (var 关键词 变量名相同 向上覆盖)
                console.log(i); // 0 1 2 3 4 5 6 7 8 9
            }

            for 循环 拆构
            var i = 0；
            for (; i < 10;) {
                代码块
                i++
            }

            console.log(i); // 10
            console.log(this.i); // 10 函数中的this 是 window
        };
        demo();
        console.log(i); // 10
```
- 求以下输出的值
```
        var obj = {
            age: 18
        };
        // ('age' in obj) => 判断obj对象中是否存在age属性名 //true
        if (!('age' in obj)) { // ! 取反 (!true)=> false 
            var a = 20
            console.log(a)
        } // 不会输入任何值
```
- arr的值为?
```
        var arr = [1,2,1,3,5,9]
        // 第一个参数 索引值->下标
        // 第二个参数是 从索引值开始删除几个元素
        arr.splice(1,1) // [1,1,3,5,9]
```
- 请写出以下结果
```
        var name = 'hello window';
        var obj = {
            name: "objName",
            getName: function () {
                // this.name => obj.name 
                console.log(this.name) // 这里的this是这个对象obj this->当前作用域
            }
        }
        obj.getName(); //打印什么? //“objName”
        // 两个等于对比值 
        //          obj.name == window.name
        //          "objName" == "hello window"
        console.log(obj.getName() == name); // false or true // false 
```
- 将var str = ['a','b','c',3,4,5,'e','f','g','a','b','c','a','b'],写出3条js语句实现以下3个功能
- 去掉数组中的a,b,c字符,得出结果345efg ;
```
        var arr = ['a', 'b', 'c', 3, 4, 5, 'e', 'f', 'g', 'a', 'b', 'c', 'a', 'b']

        function splice_abc(str) {
            // 正叙循环有可能删不干净 -> 删除一个后 数组的长度就会改变 每个元素的索引值也会改变
            for (let index = str.length; index >= 0; index--) { // 倒叙循环 
                if (str[index] == 'a' || str[index] == 'b' || str[index] == 'c') {
                    str.splice(index, 1)
                }
            }
            return str
        }
        console.log(splice_abc(arr))
```
- 将数组中的数字用中括号括起来,得出结果abc[3][4][5]efgabcab;
```
        var arr = ['a', 'b', 'c', 3, 4, 5, 'e', 'f', 'g', 'a', 'b', 'c', 'a', 'b']

        function parentheses(str) {
            let new_str = '';
            for (let index = 0; index < str.length; index++) {
                if (typeof str[index] == "number") { // 判断 该元素是否为 数字类型
                    str[index] = `[${str[index]}]`
                }
                new_str += str[index]
            }
            return new_str
        }
        console.log(parentheses(arr))
```
- 将数组中的数字都乘以2,得出结果 abc6810efgabcab;
```
        var arr = ['a', 'b', 'c', 3, 4, 5, 'e', 'f', 'g', 'a', 'b', 'c', 'a', 'b']

        function mult(str) {
            let new_str = ''
            for (let index = 0; index < str.length; index++) {
                if (typeof str[index] == "number") { // 判断 该元素是否为 数字类型
                    str[index] = str[index] * 2
                }
                new_str += str[index]
            }
            return new_str
        }
        console.log(mult(arr))
```
- css的选择器有哪些?优先级是怎样的?
选择器有：{
基础选择器![](https://upload-images.jianshu.io/upload_images/12946880-09e0d6d617479259.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  组合选择器![](https://upload-images.jianshu.io/upload_images/12946880-b4ac92507d328d6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  属性选择器(个人最常用的)![](https://upload-images.jianshu.io/upload_images/12946880-0da45104b94f9b78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  伪类选择器![](https://upload-images.jianshu.io/upload_images/12946880-80abd55cf83ef71b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  伪元素选择器![](https://upload-images.jianshu.io/upload_images/12946880-40356635096e7bba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

}
优先级：(从上到下)
!important
内联(1,0,0,0) 1000
id: (0,1,0,0) 100
类：(0,0,1,0) 10
伪类/属性
元素：(0,0,0,1) 1
通配符
- 标准盒模型和怪异盒模型有什么区别?
标准盒模型: 一个块的总宽度 = width+padding 左右+margin 左右 + border 左右
怪异盒模型: 一个块的总宽度 = width + margin 左右
box-sizing:border-box 
手机端用的更多 pc端也不少 
- 将一个不定宽高的盒子设置水平垂直居中  方法越多越好
- 通过 定位加 translate 无需手动计算值
```
        .con {
            width: 500px;
            height: 500px;
            background-color: coral;
            position: relative;
        }
        
        .cent {
            width: 100px;
            height: 100px;
            background-color: hotpink;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%)
        }
```
- 通过定位加上 手动计算 设置margin
```
        .con {
            width: 500px;
            height: 500px;
            background-color: coral;
            position: relative;
        }
        
        .cent {
            width: 100px;
            height: 100px;
            background-color: hotpink;
            position: absolute;
            left: 50%;
            top: 50%;
            margin-top: -50px;
            margin-left: -50px;
        }
```
- 父盒子设置为弹性盒子 水平居中 垂直居中属性
```
        .con {
            width: 500px;
            height: 500px;
            background-color: coral;
            display: flex;
            /* 弹性盒子 */
            justify-content: center;
            /* 水平居中 */
            align-items: center;
            /*垂直居中*/
        }
        
        .cent {
            width: 100px;
            height: 100px;
            background-color: hotpink;
        }
```
- 将元素转换为表格特性
```
        .con {
            width: 500px;
            height: 500px;
            background-color: coral;
            display: table-cell;
            /* 作为一个表格单元格显示 td th*/
            text-align: center;
            /*文本居中*/
            vertical-align: middle;
        }
        
        .cent {
            width: 100px;
            height: 100px;
            background-color: hotpink;
            display: inline-block;
            /* 行类块元素*/
            vertical-align: middle;
            /*把此元素放置在父元素的中部*/
        }
```
```
    <div class="con">
        <div class="cent"></div>
    </div>
```
- 实现以下布局   左边固定宽度200px  右边自适应
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
        
        .content {
            position: fixed;
            left: 0;
            top: 0;
            right: 0;
            bottom: 0;
            display: flex;
        }
        
        .left {
            height: 100%;
            width: 200px;
            background-color: blueviolet;
        }
        
        .main {
            flex: 1;
            background-color: brown;
        }
    </style>
</head>

<body>
    <!-- 实现以下布局   左边固定宽度200px  右边自适应 -->
    <div class="content">
        <div class="left"></div>
        <div class="main"></div>
    </div>
</body>

</html>
```
- 用过哪些h5c3的新特性?
语义化的标签、表单的很多新属性、音屏、视频、canvas、webstorage、websocket、地理位置
- 封装一个函数,实现求最大值和最小值.
```
        // 封装一个函数,实现求最大值和最小值.
        function getMaxMin(arr, fn) {
            let max = min = arr[0]
            let type = typeof arr[0] == "string" // 判断数组的第一个元素是否为字符串
            if (!fn) throw new Error('fn is no incoming') // 抛异常
            for (let index = 0; index < arr.length; index++) {
                if (type ? fn(max, arr[index]) < 0 : arr[index] > max) {
                    max = arr[index]
                }
                if (type ? fn(min, arr[index]) > 0 : arr[index] < min) {
                    min = arr[index]
                }
            }
            return {
                max,
                min
            }
        }
        // 兼容代码 字符串做比较
        function jr(str1, str2) {
            if (str1.length > str2.length) {
                return 1
            } else if (str1.length < str2.length) {
                return -1
            } else {
                return 0
            }
        }
        let arr_n = [1, 2, 3, 4, 1, 2, 4, 8]
        let obj = getMaxMin(arr_n, jr)
        console.log(obj.max, obj.min)

        let arr = ["sdas", "sdsss", "22", "bsjbdjasdbsaj", "1"]
        let obj1 = getMaxMin(arr, jr)
        console.log(obj1.max, obj1.min)
```
- 封装一个排序
```
        function sort_n(arr) {
            for (let row = 0; row < arr.length; row++) {
                for (let index = 0; index < arr.length - row; index++) {
                    if (arr[index] > arr[index + 1]) {
                        let temp = arr[index]
                        arr[index] = arr[index + 1]
                        arr[index + 1] = temp
                    }
                }
            }
            return arr
        }

        let arr = [1, 2, 5, 6, 8, 4]
        console.log(sort_n(arr))
```
- 控制台会输出什么,为什么会这样输出
```
// 声明的函数名、对象名、数组名 在内存中 存放在栈里面
// 通过内存地址相关联
// 函数体和形参列表、对象体、数组值 存放在堆里面
// 复杂数据类型的变量与变量之间的赋值 不是直接将数据复制一份到变量中 是将内存地址复制一份给变量
var a = {age:18};
var b = a;
a.age = 15;
console.log(b.age) // 15
```
- null 和 undefined的区别?
null 表示一个值被定义了 定义为“null”
undefined 表示一个值被声明了 但是未赋值
所以设置一个值为null是合理的 设置一个值为undefined是不合理的
null == undefined (true)
null === undefined (false) => 值相同 (boolean转换都为0) 但是数据类型不同
- 执行这段代码,输出的结果
```
function test(){
console.log(a); // undefined

console.log(foo()); // 2
var a = 1;
function foo(){
return 2;
}
}
test()
// 预解析 声明是函数和变量都会提升到作用域的最上面
//     函数会将整个函数代码都提升
//     变量只会提升变量的声明
function test(){
	var a;
	console.log(a); // undefined
	function foo(){
		return 2;
	}
	console.log(foo()) // 2
	a = 1;
}
```
- 请使用三元表达式完成下列判断
```
var a = 1;
if(a < 1){
  a = 0
}else{
  a = 2
}
// : 前面的为true ，: 后面的为false
var a = a<1?0:2
```
- 实现数组去重   方法越多越好  
var a = [1,3,4,6,1,1,9,4,3]
 - 双循环 ->效率不高
```
        function getN(arr) {
            let now = [arr[0]] // 新数组 -> 将传递进来的数组第一个元素push进去
            for (let index = 0; index < arr.length; index++) { // 循环传递进来的数组
                let flag = true // 声名一个flag 用来判断是否重复
                for (let i = 0; i < now.length; i++) { // 循环新数组 
                    if (arr[index] === now[i]) { // 判断是否有一样的元素
                        flag = false 
                        break
                    }
                }
                if (flag) {
                    now.push(arr[index]) 
                }
            }
            return now
        }
        console.log(getN([1, 2, 11, 2, 1, 2, 5, 22, 11, 545, 12]))
```
- es6新方法
```
        // es6 新提供的方法
        // Set 数据不重复
        function es6_set(arr) {
            return new Set(arr)
        }

        console.log(es6_set([1, 2, 11, 2, 1, 2, 5, 22, 11, 545, 12]))
```
- indexof
- 布局中,造成盒子margin塌陷的原因是什么? 怎么解决塌陷问题?
Margin 塌陷 是什么？
当父元素包裹着子元素(都为block块级元素)，当给子元素设置margin-top时。子元素没有位移 反而父元素位移了 
如何解决？
给父元素 加border
给父元素 溢出隐藏 overflow: hidden;
给父元素 加浮动
给父元素 设置 display:table
给父元素 设置 position:fixed
给子元素 设置 display:inline-block
通过伪元素来解决margin塌陷
- 如何清除浮动? 方法越多越好
额外标签法 在浮动标签的后面添加一个元素 该元素设置 clear:both
使用overflow 给父盒子设置 overflow:hidden
伪元素清除浮动
双伪元素清除浮动
[https://www.jianshu.com/p/25766fe5b34f](https://www.jianshu.com/p/25766fe5b34f)



