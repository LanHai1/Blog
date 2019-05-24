---
title: TypeScript概括
---

### 浅谈-开篇前言
TypeScript是JavaScript的超集（遵循ECMAScript6语法）， 这个语言添加了基于类的面向对象编程。TypeScript作为JavaScript很大的一个语法糖，本质上是类似于css的less、sass，都是为了易于维护、开发，最后还是编译成JavaScript，只要是js可以运行的地方它都可以运行。
##### ES是什么？ES6和ES5之间的关系？和JS和TS的关系？
简单来说ES就是一种规范，ES6和ES5就是这个规范的不同版本，JS实现了ES5，TS实现了ES6，JS和TS是客户端不同的版本。
##### TypeScript相对于JavaScript的优势
- 支持 **ES6** 的规范
- 强大的 **IDE** 支持
    2.1 **类型检查**
*为变量指定类型 -> 减少开发阶段的几率*
   2.2 **语法提示**
*根据上下文，把所可能需要的类、变量方法、关键字都会有提示 -> 提高开发效率*
  2.3 **重构**
*方便修改变量、方法名、文件名(IDE会自动修改) -> 提高代码质量和效率*
- angular2 的开发语言
#### 学习TypeScript的收益
- 强大的静态类型系统
- 完善的内部代码库
- 几乎所有的**API**都能得到准确的智能提示
- 可以帮助团队更好的理解架构内容的数据流向
### 搭建TS开发环境
`sudo npm install -g typescript`
> 目前的主流浏览器还不完全支持ES6的语法 所以目前想要在浏览器运行TS代码需要Compiler编译器来将TS代码转换为浏览器支持的JS代码

`tsc 文件名.ts`
文件名.ts -> 文件名.js
#### Type类型
*所有类型都是any类型的子类型*

**元类型**(*primitive types*)
```
number、boolean、string、null、undefined
```
**对象类型**(*object types*)
```
所有类、模块、接口、字面量
```
## TypeScript 新特性
#### 字符串新类型
- 多行字符串
```
console.log(`
<a>1</a>
<sapn>2</span>
`)
```
- 字符串模版
```
let tt= 1
console.log(`你好我叫${tt}`) 
```
- 自动拆分字符串
```
function test_n(template,name,age){
    console.log(template)
    console.log(name)
    console.log(age)
}
var myname = "兰海"
var getAge = ()=>{
    return 19
}
/*
直接函数调用 函数名`` 
->会将里面的 模版string 作为第一个参数 返回一个数组['我叫','，今年','!'] 根据模版字符串切割
->第一个模版和第二个模版分别对应函数中的第二第三参数 只会返回对应的值 // 兰海  // 19
*/
test_n`我叫${myname}，今年${getAge()}!`
```
#### 参数新类型
 - 参数类型
> 在参数名称后面使用冒号来指定参数的类型
类型推断机制 -> 根据编写的值来设定该变量的参数类型
(自定义类型)(字符串数组类型)(数字数组类型)(元组类型)(联合类型)(显式类型)

*设置参数类型 类型推断机制*
```
var myname: string = "兰海"

//-> [ts] 后续变量声明必须属于同一类型。变量“myname”必须属于类型“string”，但此处却为类型“number”。
// 因为在前面已经定义了变量的类型 所以后续再给变量赋值必须遵循之前声明的类型
var myname = 1; // 报错
```
*any 其他类型 -> 元类型*
```
// any 所有类型都可以
var myts1: any = 10
myts1 = "你好"

var myts2: boolean = true
var myts3: string = "str"
var myts4: number = 1
var myts5: null = null
var myts6: undefined = undefined
```
*函数返回值*
```
// :void 该函数不需要任何返回值
function test(): void {
    
}

// :any 该函数可以返回任何类型的返回值
function test2(): any {
    return "any"
}

// 注意除了 :void 和 :any 以外其他声明的类型 都必须return一个值

// 可以规定返回值类型 string number boolean null ....
function test3(): string {
    return "1"
}

// 还可以设置函数的 形参的类型
function test4(x: number, y: string): string {
    return ""
}
// 在函数调用的时候 实参也必须遵循设置的类型
test4(1,'2')
```
*自定义类型*
```
class Person {
    age: number;
    name: string;
}
var per: Person = new Person()
per.age = 1
per.name = "兰海"
```
- 默认参数
> 在参数声明的后面用等号来指定参数的默认值
(在参数名称后面用 **:** 指定参数的**类型**)
(在参数名称后面用 **=** 指定参数的**默认值**)
*注意:带默认值的参数要声明在最后面！*
```
// 指定参数的默认值
// 不传参数会报错 所以可以设置默认值
// 带默认值的参数一定要声明在最后面！
function test6(x:number,y:number = 2):any{
    console.log(x,y)
    return y
}
var ts = test6(12)
console.log(ts)
```
- 可选参数
> 在参数声明的后面用问号来指定参数为可选
*注意:可选的参数要声明在最后面！默认值和可选不能同时设置，可选参数🉑️和指定参数类型一起设置*

```
// 参数可选 ? 可以传递也可以不传递
// 注意 在函数内部得处理可选参数
// 带可选的参数一定要声明在最后面 不能同时设置可选和默认值！
function test7(x: string, y?: number) {
    console.log(x)
    if (y) {
        console.log(`参数已传递`)
    } else {
        console.log(y,`参数未传递`)
    }
}
test7("11")
test7("122",999)
```
#### 函数新类型
- Rest and Spread 操作符 (...)
> 用来声明任意数量的方法参数
```
// 用来声明任意数量的方法参数 
// agrss -> arguments 伪数组
function ts1(...argss){
    argss.forEach(arg => {
        console.log(arg)
    });
}
ts1(1,2,3,4,5,6)
ts1(1,2)
```
- generator 函数
> 控制函数的执行过程，手工暂停和恢复代码执行 (* yield)
```
function* ts3() {
    console.log('start')
    yield
    console.log('center')
    yield
    console.log('end');
}

var ts3_3 = ts3()
ts3_3.next() // statt
ts3_3.next() // center
ts3_3.next() // end


function* getStock(stock) {
    while (true) {
        yield Math.random() * 100
    }
}

var stockGenerator = getStock('BaiDu')
var limPrice = 15
var price = 100
// 当返回的值大于15会一直循环 小于15就会停止循环
while (price > limPrice) {
    price = stockGenerator.next().value
    console.log(`the generator return ${price}`)
}
```
- destructuring析构表达式
> 通过表达式将对象或数组拆解成任意数量的变量
直接将对象里对应名称的变量定义和赋值到外部
 ```
// var {a,b} = 对象 直接将对象里对应名称的变量定义和赋值到外部
// 注意 析构表达式里面的变量要和对象里面的变量的名字是相同的
function ts6() {
    return {
        name_t: '蓝海',
        age: 18
    }
}

var { name_t, age } = ts6()
console.log(name)
console.log(age)

// 如果需要后续自定义变量名
var { name_t: myname, age } = ts6()
console.log(myname)
console.log(age);
```
*嵌套变量取值*
```
// 取嵌套属性
var obj = {
    t1: '睡觉睡觉',
    t2: {
        ts2_1: "其擦好",
        ts2_2: "好的"
    }
}

var { t1, t2: { ts2_2 } } = obj
console.log(t1)
console.log(ts2_2)
```
```
// var [1,2,3,4] = 数组 直接将数组里对应名称的变量定义和赋值到外部
var arr = [1, 2, 3, 4]
var [num1, num2] = arr
console.log(num1, num2) // 1 2

var [, , num1, num2] = arr
console.log(num1, num2) // 3 4

var [num1, , , num2] = arr // 1 4
console.log(num1, num2)

var [num1, , num2] = arr // 1 3
console.log(num1, num2)

var [num1, num2, ...other] = arr
console.log(num1, num2, other) // 1 2 arr[3,4]
```
*析构表达式 结合 Rest and Spread 操作符*
```
var arr = [1, 2, 3, 4]
function getArr([num1, num2, ...other]) {
    console.log(num1, num2, other)
}
getArr(arr)
```
#### 表达式和循环
- 箭头函数
> 用来声明匿名函数，消除传统匿名函数的this指针问题(setInterval)
```
var arr = (ag1, ag2) => ag1 + ag2

var retarr = arr(1, 2)
console.log(retarr) // 3

var myarr: Array<number> = [1, 2, 3, 4, 5]

console.log(myarr.filter(value => value % 2 == 0)) // arr[2,4]
```
*解决this指向*
```
var arr_n = (name_n) => {
    this.name_n = name_n
    setInterval(() => {
        console.log(`name is:${this.name_n}`) // 这里的thi还是全局的this 并不是函数的this
    }, 1000)
}
arr_n("兰海")
```
- 循环
> forEach()、for in、for of
```
// for of 可以打印字符串
var arr_ss = []
for (const iterator of "我是一个字符串") {
    arr_ss.push(iterator)
    console.log(iterator)
}
console.log(arr_ss) //  ["我", "是", "一", "个", "字", "符", "串"]
```
## 面向对象特性
#### 类(class)
>类是TypeScript的核心，使用TypeScript平时是，大部分代码都写在类里面
(类的定义、构造函数、类的继承)

- 访问控制符
`public` ：类的内部外部都能访问(默认)
`private` ：只能在类的内部访问(私有的)
`protected` ：能在类的内部和子类(继承)中使用(受保护的)
- 类的构造函数
>constructor(){} 不能在外部访问到，只有在类的实例化的时候才会被调用(1次)
*用途:该类实例化的时候必须带某个参数*
```
class person {
    // 构造函数 
    // 该类实例化的时候必须带ages 属性
    constructor(public ages:number){ // 在构造函数上必须明确声明访问控制符
        this.ages = ages 
        console.log(`只会在实例化过后执行一次`)
    }
    public name_n; // public 公开的 默认
    private food; // prevate 私有的 只能在类的内部访问
    protected eat() { // protected 受保护的 只能在类的内部和子类{继承}中使用
        console.log(`${this.name_n}在吃${this.food}`)
    }
}
var per1: person = new person(1)
per1.name_n = "兰海"
console.log(per1.ages)
```
- 类的继承
>子类 extends 父类
super() 调用父类的构造函数
super.fn() 调用父类的方法
```
class persons {
    // 必须传入名字
    constructor(public name_n: string) {
        this.name_n = name_n
        console.log(`haha`)
    }
    private age: number;
    public eat() {
        console.log(`${this.name_n}在吃东西`)
    }
}
// extends 继承来的函数不能声明构造器 可以使用super()
class student extends persons {
    constructor(name_n: string) {
        super(name_n) // 调用父类的构造函数 并执行函数
    }
    public sex: string
    public work() {
        super.eat() // 调用父类的函数(方法)
        console.log(`工作${super.name_n}`)
    }
}
var stu = new student("兰海");
stu.eat()
stu.work()
```
#### 泛型(generic)
>参数化的类型，一般用来限制集合(数组)里面值的数据类型
```
// 尖括号里面的就是泛型 规定arr数组里面的值只能是number类型
var arr: Array<number> = new Array()
```
#### 接口(Interface)
>用来建立某种代码约定，使得其它开发者在调用某个方法或创建新的类时必须遵循接口所定义的代码约定
- 接口Interface 用户规定参数的类型
- implements 实现某接口的类 必须实现该接口中的方法
```
// interface 规定参数的类型
interface IPerson {
    name_n: string,
    age?: number
}
class Person {
    constructor(public config: IPerson) {
    }
}
var p1 = new Person({
    name_n: "兰"
})

// implements 实现某接口的类，必须实现该接口中的方法
interface Ipers{
    eat();
}
class per implements Ipers {
    eat(){
        console.log(`实现接口中的方法`)
    }
}
```
#### 模块(Module)
>模块可以帮助开发者将代码分割为可重用的单元，开发者可以自己决定将模块中的哪些资源(类、方法、变量)暴露出去供外部使用，一个文件就是一个方法

- `export` 对外暴露
- `import` 引入
#### 注释(annotation)
>注解为程序的元素(类、方法、变量)加上更直观更明了的说明，这些说明信息与程序的业务逻辑无关，而是供指定的工具或框架使用的
#### 类型定义文件(*.d.ts)
>类型定义文件用来帮助开发者在TypeScript中使用已有的js工具包如果:jquery

`npm install @types/jquery`

## 总结
TypeScript的好处很明显，在编译的时候就能检查出很多语法问题，而不是在运行后，不过由于ts是基于类的面向对象编程，如果是纯前端的人员(没有后端的语言基础)，那么用起来会比较吃力。

![xmind知识导图](https://upload-images.jianshu.io/upload_images/12946880-e55d8a924b377a8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 极致的代码提示
#### 极致的检查错误
#### 极致的代码重构
#### 极致的模块管理
#### 极致的JS支持
#### 极致的ES6