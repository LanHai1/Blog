---
title: JavaScript ECMAScript语法概括
---
*本篇讲解ecma语法*

## JavaScript是什么 ？
 - JavaScript是一门脚本语言 
  脚本语言:不需要编译，直接运行(js,python...)
  编译语言:需要把代码翻译成计算机所认识的二进制语言，才能运行(c,c++,java...)
- JavaScript是一门**基于**对象的语言
  基于 而不是 面向
  JavaScript可以模拟其他语言的特性(封装、继承、多态)
- JavaScript是一门动态类型的语言
- JavaScript是一门弱类型的语言
- JavaScript是一门解释性的语言
---
## JavaScript能做什么？
- 前端开发
- 后端开发
- 底层
- 操作系统开发
- 大数据
- 区块链
- 人工智能
- ... js无所不能
---
## 引入js的两种方式
- 内联
  在html中 body标签内的最下方通过script标签 编写js代码
   `<script>var a = 1;</script>`
- 外联
  引入外部文件 在html中 body标签内的最下方通过script标签的src属性引入文件
 `<script src="./index.js"></script>`
---
## 为什么要有变量？
> 为了存储和复用一个数据，方便修改

`当声明一个变量的时候 var a = 10 ,会在内存中开辟以一个空间 存放变量名和变量值 ,会将变量和值存放到window对象中 a:10`

---
#### 1. 全局变量
> 全局作用域中的变量 可以在该脚本文件中的任何地方访问到
#### 2. 局部变量
> 局部作用域中的变量 局部变量只能在该函数中访问到

##### 注意
`在局部作用域中(函数中)声明变量 如果未加关键字 var || let || const` 这个变量就会变为隐式全局变量
```
function a(){
  num = 1;
}
=>
var num;
function a(){
  num = 1;
}
```
```
/*只有num1 是局部变量 ， num2 和 num3 是隐式全局变量*/
function a(){
  var num1 = num2 = num3 = 100;
}
=>
var num2;
var num3;
function a(){
  var num1 = 100;
  num2 = 100;
  num3 = 100;
}
```
---
## JavaScript代码规范以及变量命名
>声明关键词 `var let const`
变量名区分大小写

`命名规范`
> 一般以字母、$、下划线、数字组成(不能以数字开头)
不能使用系统关键词和保留字
一般遵循驼峰命名法(第一个单词首字面小写后面单词首字母大写)
变量名需得有意义
---
## 预解析
`JavaScript在执行的过程中，浏览器的js引擎会将代码扫描一遍(预解析) 并把 **声明式** 函数 和 变量 提升到 **该作用域** 的最顶端`
---
#### 1. 变量预解析
> 变量预解析只会将变量的声明提升到该作用域的最前面

```
console.log(a) // undefined
var a = 10;
=>
var a 
console.log(a) // undefined
a = 10 
```
#### 2. 函数预解析
> 变量预解析只会将变量的声明提升到该作用域的最前面

```
b()
function b(){
}
=>
function b(){
}
b()
```
---
## 断点调试
`直接在浏览器中Sources添加断点`
`或者在编辑器中在语句之后添加 debugger`
---
## 数据类型
---
#### 什么是数据类型？
>计算机里面有很多数据，JS为了方便调试与使用，提供了一些对应的辅助方法，将它们做了归类，就是数据类型。

---
#### 简单数据类型
---
##### 1. Number
1.1. **整数** => 正数 负数
1.2. **小数**
1.3. **NaN**
  *Not a Number => 不是一个数字*
  判断一个数字是否是非数字`isNaN(x)` 返回布尔值 **true**是一个非数字，**false**不是一个非数字
```
isNaN(undefined) // true
isNaN(null) // false
```
>NaN不等于任何值，包括它自己

- 数值的取之范围
```
Infinity // 无穷大
-Infinity // 无穷小
Number.MAX_VALUE /*最大值*/
Number.MIN_VALUE /*最小值*/
```
- 运算符
算数运算符
> +:加
-:减
*:乘
/:除
%:求模
任何数除以0为无穷大
任何数`-*/%`字符串为NaN (除了+)

复合运算符
>+= <a+=1 => a = a+1>
-= <a-=1 => a = a-1>
 "*=" <a*=1 => a = a*1> (乘等于)
/= <a/=1 => a = a/1>
>"%=" <a%=1 => a = a%1>

"==||==="
两个等于和三个等于的区别
两个等于只对比值 三个等于对比值和数据类型

自增自减运算符
`
a++
++a
a--
--a
`
- 前置自增 ++a `先运算、再返回`
- 后置自增 a++ `先返回、再运算`


---
##### 2. String
2.1 字符串的长度`str.length`
2.2 字符串加号运算 会将数据与数据拼接`字符串 + 任何数据类型 = 拼接之后的新的字符串`
---
##### 3. Boolean
`true => 1`
`false => 0`
> 用布尔值来判断条件真假
---
##### 4. Undefined
未定义

---
##### 5. Null
空

---
 - undefined和null的区别
>null 表示一个值被定义了，定义为null
undefined 表示一个值声明了 但是未定义(未赋值)
所以设置一个值为null是合理的 设置一个值为undefined是不合理的
null == undefined // true
null === undefined // false
说明 null 和 undefined 在转boolean 的时候都为 0 值相同 所以 == 为true  ，数据类型不一样 === 为false

---
#### 复杂数据类型
---
##### 1. Array
- 创建数组的方式

`new`
```
new Array()
new Array(初始长度)
new Array( 1 , 2 , 3 )
```
`字面量`
```
[ ]
[ 1 , 2 , 3 ]
```
- 数组的三个概念
> 元素: 数组中的值就是元素
> 下标(索引): 下标从0开始
> 长度: length 数组的长度 从1开始
- 遍历数组

`正叙遍历`
```
for(let i = 0; i < arr.length; i++){
  /*代码块*/
}
```
`倒叙遍历`
```
for(let i = arr.length; i > 0; i--){
  /*代码块*/
}
```
- 栈和队列操作

`栈操作 先进后出`
```
arr.unshift() /*从头部放入*/
arr.shift() /*从头部取出*/
```
```
arr.push(); /*从尾部放入*/
arr.pop(); /*从尾部取出*/
```
`队列操作 先进先出`
```
arr.unshift() /*从头部放入*/
arr.pop(); /*从尾部取出*/
```
```
arr.push(); /*从尾部放入*/
arr.shift() /*从头部取出*/
```
---
##### 2. Function
- 函数的声明

`声明式创建函数`
```
function fn (){
  //代码块
}
```
`匿名方式创建函数`
```
let fn = function(){
  //代码块
}
```
`函数对象`
```
let fn = new Function("形参","形参","代码块") /*前面的都是参数，最后面的参数是代码块*/
```
- 函数的默认值
`形参默认值 形参 = 形参 || 默认值`
- 函数的调用
`函数名(实参列表)`
- return 关键字
> 函数中 return 将数据给调用者
> 函数中遇到 return 直接跳出函数 后面代码不执行
> 函数默认 return undefined
- 函数的预解析
>只有声明式的函数和变量才会预解析 
  先调用 -> 后声明
在js执行的时候 浏览器会执行 预解析的操作
`预解析会把所有的 声明式 创建的函数(包括变量) 提升到当前作用域的最顶端`

- 回调函数
`函数作为参数传递`
`函数作为返回值`

- arguments 属性
>函数内部的特殊属性 用来保存实参列表
arguments 是一个 `伪数组` => 不是数组 但是具有数组的特性(下标索引、长度、可以进行循环遍历)
>可以模拟函数重载 => 同一个函数，因为传递的实参数量不同，可以执行不同的操作
- 工厂函数 和 构造函数
```
/*工厂函数*/
function createStr(name,age){
  let obj = new Object()
  obj.name = name
  obj.age = age
  obj.sayHi = function(){
    console.log(this.obj)
  }
  return obj
}
let obj1 = createStr("蓝海",11)
```
```
/*构造函数*/
function StuInFo(name,age){
  this.name = name
  this.age = age
  this.sayHi = function(){
    console.log(this.obj)
  }
}
let obj2 = new StuInFo("蓝海",11)
```
>构造函数相对于自定义类(class)
>1.变量名一般用名词，首字母大写
2.构造函数中 不需要我们去创建对象、返回对象
3.构造函数一般 被 new 关键词调用

>new 关键字 做的四件事
>1. 创建空对象
>2. 将空对象 传递给 构造函数 里面的 this
>3. 调用构造函数
>4. 返回对象
- 自调用函数
> 声明的时候直接调用执行
```
(function("形参列表"){
  /*代码块*/
})("实参列表")
```

- 函数内存存放
`函数名存放在栈里面`
`通过内存地址相关联`
`函数体和形参列表存放在堆里面`
---
##### 3. Object
 - 创建对象的方式

`new`
```
let obj = new Object()
||
let obj = new Object({
  name : "蓝海",
  age : 11
})
```
`字面量`
```
let obj = { }
||
let obj = {
  name : "蓝海",
  age : 11
}
```
- 访问对象中的属性
`对象.属性`
`对象["属性"]` => 注意 用中括号的方式访问对象中的属性 属性名🉐️加引号
`访问对象中不存在的属性` => undefined
- 对象属性操作
 `添加属性` => 直接对象 . 属性名 = 属性值
`删除属性` =>  delete 对象 . 属性名 || delete 对象 . 方法名
- 判断一个对象中是否存在某个属性或某个方法
`"属性" in 对象` => `"name" in obj`
- 对象中的 this 
>对象中的this就是这个对象 可以通过this.属性名 在对象的方法中访问对象里的其他属性或方法
- 对象内存存放
`对象名存放在栈里面`
`通过内存地址相关联`
`对象里面的方法和属性存放在堆里面`
---
## 获取数据类型
#### 1. typeof
`typeof`检查简单数据类型[值类型] 返回的是 字符串类型的数据类型
```
typeof(123) => "number"
/*注意点*/
typeof(typeof(123)) => "string"
typeof(null) => "object"
typeof(Infinity) => "number"
typeof(NaN) => "number"
typeof(new Date()) => "object" => /*typeof检查其他new出来的对象 都返回 object*/
/*typeof() 复杂数据类型object、array 和 null 都返回 object
  typeof(function) => function // 函数除外
*/
```
#### 2. instanceof
`instanceof`检查复杂数据类型[引用类型] 返回boolean值
```
数据名 instanceof 数据类型 /*[Function||Object||Array]*/
返回 false 或 true
只能检查复杂数据类型 简单数据类型都返回false
```
#### typeof  和 instanceof 的区别
`typeof`用来检测简单数据类型 返回值是一个字符串的数据类型
`instanceof`用来检测复杂数据类型 返回的是boolean值 如果用来检测简单数据类型都会返回false
--- 
## 数据类型转换
---
#### 1. 转换为字符串
`str.tostring()` *默认模式*把变量转换为字符串 || *基模式*
`String(str)` 把特殊值转换为字符串 String(undefined) => “undefined”
`加号拼接字符串`
#### 2. 转换为数组
`parseFloat(str)` 把字符串转换为浮点数(小数)
`parseInt(str)` 把字符串转换为整数

{`Number(str)` 任何数据转换为整数
>如果字符串截去开头和结尾的空白字符后，
不是纯数字字符串，那么最终返回NaN。
如果字符串中只包含了数字，(包括前面带正号和负号)，
则将其转换为十进制数值，即"1"->1，”011“->11
如果字符串是空的""则返回0
如果字符串包含除了上面的格式之外的字符，则将其转换为NaN
```
boolean  true转换为1 false转换为0
null 返回 0
undefined 返回 NaN
```
}
#### 3. 转换为布尔值
`Boolean()` 
> 将布尔字符串转成布尔值 
> 除了 0、false 、undefiend、NaN、null 转换为false  其他都为true
#### 4. 转义符
`console.log("转义符的应用\"嗯") ->[转义符的应用“嗯]`
*常用*
- \n 换行符
- \" 双引号
- \' 单引号
-  \ \ 斜杠\
---
## JavaScript内置对象
[戳一下👇](https://www.jianshu.com/p/861983d569f3) 之前总结的常用内置对象博客

---
## 条件判断
*一些很简单条件判断这里就不说了哈* 😶
#### 1. 三元运算符
`条件 ? true : false`
#### 2. switch-case
```
/*相对于 if (a === 1) 用来做全等判断*/
break /*每个case里面的代码断都要写 break 否则会穿透*/
/*合理使用switch穿透 <当多个case要执行的代码需求一样的时候>*/
default /*如果case都不匹配的情况下 会执行 default 里面的代码*/
```
#### 3. && || !
`&&` 与 一假则假
`||` 或 一真则真
`!` 非 取反
---
## 循环
---
#### 1. for
*for 循环体上面的 i 变量的作用域是全局作用域 如果在函数中使用就是局部作用域 一般建议使用let 关键词来声明 i*
```
for (let i = 1;i < 10; i++){
  /*代码块*/
}
/*拆解为*/
let i = 1;
for(;i<10;){
  /*代码块*/
  i++;
}
```
#### 2. do while
`先执行do里面的代码块 再条件判断`
```
do{
  /*代码块*/
}while(boolean)
```
#### 3. while
`先条件判断 再执行代码块`
```
while(boolean){
  /*代码块*/
}
```
##### do-while 与 while 的区别
> do-while 不管条件是否成立 都会执行一次do里面的代码块 然后再进行条件判断
while 先进行条件判断 再执行代码块

#### 3. for in
`遍历对象`
```
for(let "属性名" in "对象"){
  /*访问对象中的属性或方法*/ /*这里的属性名已经是string类型 所以无需再加引号*/
  obj[name] => /*对象[属性名]*/
}
```
##### break 和 continue 的区别
> break 跳出整个循环 后面的循环不再执行
continue 跳出本次循环 继续执行后面的循环
---
![xmind知识导图](https://upload-images.jianshu.io/upload_images/12946880-a93529cf1a80804e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
*部分很简单的知识点未写入博客*





