---
title: 【js红宝书】第3章语言基础：变量、数据类型和操作符
date: 2020-12-23
tags:
      - JavaScript
      - js红宝书学习笔记
categories: 前端
index_img: https://img9.doubanio.com/view/subject/s/public/s33703494.jpg
banner_img: https://img9.doubanio.com/view/subject/s/public/s33703494.jpg
---
# 语法
## 区分大小写
ECMAScript中**一切都区分大小写**。无论是变量、函数名还是操作符，都区分大小写。变量test和变量Test是两个完全不同的变量。typeof不能作为函数名，因为它是一个关键字。但是Typeof 是一个完全有效的函数名。
## 标识符
标识符，就是变量、函数、属性和函数参数的名称。标识符可以由一个或多个字符组成。
* 第一个字符必须是字母、下划线_ 或美元符号$
* 剩下的其它字符可以是字母、下划线、美元符号或数字。
按照惯例，ECMAScript标识符可以使用驼峰大小写形式，即第一个单词的首字母小写，后面每个单词的首字母大写。
如：firstSecond  myCar  doSomethingImportant
# 关键字和保留字
ECMA-262 描述了一组保留的关键字，这些关键字有特殊用途。按照规定，保留的关键字不能用作标识符或属性名：

``` 
break       do
case        else
catch       export
class       extends
const       finally
continue    for
debugger    function    
default     if          
delete      import      
in          typeof
instanceof   var
new           void
return        while
super         with
switch        yield
this          throw
try
```
规范中也描述了一组未来的保留字，同样不能用作标识符或属性名，虽然保留字在语言中没有特定用途，，但它们是保留给将来做关键字用的。
始终保留：
```
enum
```
严格模式下保留：
```
implements  package     public
interface   protected   static
let         private
```
模块代码中保留：
```
await
```
最好不要使用关键字和保留字作为标识符和属性名，以确保兼容过去和未来的ECMAScript版本。
# 变量
ECMAScript变量是**松散类型**的，意思是比那辆可以用于保存任何类型的数据。每个变量只不过是一个用于保存任意值的命名占位符。
有3个关键字可以声明变量：var、const、和let 。
var在ECMAScript的所有版本中都可以使用。
const和let 只能在ECMAScript6及更晚的版本中使用。
## var
可以使用var操作符定义变量

```js
var message;
```
上述代码定义了一个名为message的变量，可以用它保存任何类型的值。（不初始化的情况下，变量的值为undefined）。
```js
var message = 'hi';
```
现在message被定义为一个保存字符串值hi的变量。像这样初始化变量不会将它标识为字符串类型，只是一个简单的赋值而已。后面可以改变保存的值，也可以改变值的类型。

```js
var message = 'hi' //值类型为字符串
message = 100 ;//现在的值类型变为了number，不推荐这种写法
```
### var声明作用域
使用var操作符定义的变量会成为包含它的函数的局部变量。使用var在一个函数内部定义一个变量，在函数退出时，该变量就会被销毁。

```js
function test1(){
            var a = 'hello' //局部变量
        }
        test1();
 console.log(a)  // 报错：a is not defined
```
当去掉var操作符后，这个变量就变成了全局变量，只要调用一次函数，就会定义这个变量，并且可以再函数外部访问到。

```js
function test2(){
            b = 'world' //全局变量
        }
        test2();
console.log(b)  // world
```
虽然可以通过这种方式定义全局变量，但不推荐这么做。

如果要定义多个变量，可以再每一条语句中用逗号分隔每个变量。

```js
var name="Mike",age = '18',grade="12" //同时声明多个变量
console.log(`名字叫${name}今年${age}岁了，在读${grade}年级`)
```
### var变量提升
使用var声明的变量会自动提升到函数作用域的顶部。

```js
 console.log(c)  //undefined
  var c = 'height'
```
ECMAScript运行时把它看成等价于下面的代码：
```js
var c;
console.log(c)  //undefined
c = 'height'
```
这就是var的变量提升：把所有变量声明都放到函数作用域的顶部。

## let
let和var的作用差不多。但是，**let声明的范围是块作用域，var声明的范围是函数作用域**。

```js
if(true){
  var weight = '50kg'
}
console.log(weight) //正确输出

if(true){
  let weight2 = '60kg'
}
// console.log(weight2)  //报错
```
weight2变量之所以不能在if块外部被引用，是因为它的作用域仅限于该块内部。块作用域是函数作用域的子集，因此适用于var的作用域限制同样适用于let。
let也不允许在同一个块作用域中出现冗余声明，这样也会报错：

```js
let age;
let age; //报错
```
JavaScript引擎会记住变量声明的标识符及其所在的块作用域，因此在不同块级作用域中嵌套使用相同标识符不会报错。

```js
   let myAge = 30;
    console.log(myAge);    // 30
    if (true) {
    let myAge = 26;
      console.log(myAge);  // 26
    }
    console.log(myAge); // 30
```
声明冗余报错不会因为混用var和let受影响。这两个关键字的区别只是指出变量在相关的作用域如何存在而已。

```js
    var name;
    let name; // SyntaxError
    let age;
    var age; // SyntaxError
```
### 暂时性死区
let声明的变量不会再作用域中被提升。

```js
// age 不会被提升
console.log(age); // ReferenceError:age 没有定义 
let age = 26;
```
在解析代码的时候JavaScript引擎也会注意出现在块后面的let声明，只不过在此之前都不能用任何当时去引用未声明的变量。在let声明之前的执行瞬间，被称为“**暂时性死区”**，在此阶段，引用任何后面才声明的变量都会抛出ReferenceError。

### 非全局声明
使用let在全局作用域中声明的变量不会称为window对象的属性。而var声明的变量则会这样。

```js
var name = 'Matt';
console.log(window.name); // 'Matt'
let age = 26;
console.log(window.age);  // undefined
```
此时，let声明仍然是在全局作用域发生的，相应变量会在页面的声明周期内存续。所以为了避免SyntaxError，必须确保页面不会重复声明同一个变量。

### for中的let声明
在let出现以前，for循环定义的迭代变量会渗透到循环体外部：

```js
for (var i = 0; i < 5; ++i) { 
// 循环逻辑
}
console.log(i); // 5
```
改用let之后，这个问题就消失了，因为迭代变量的作用域仅限于for循环快内部：

```js
for (let i = 0; i < 5; ++i) { 
// 循环逻辑
}
console.log(i); // ReferenceError: i 没有定义
```
在使用var的时候，最常见的问题就是对迭代变量的奇特声明和修改：

```js
  for (var i = 0; i < 5; ++i) {
        setTimeout(() => console.log(i), 0)
}
// 你可能以为会输出0、1、2、3、4 // 实际上会输出5、5、5、5、5
```
这是因为在退出循环时，迭代变量保存的是导致循环退出的值：5，在之后执行超时逻辑时，所有的i都是同一个变量，因而输出的都是同一个最终值。
使用let声明迭代变量时，JavaScript引擎在后台回味每个迭代循环声明一个新的迭代变量。每个setTimeout引用的都是不同的变量实例。

```js
for (let i = 0; i < 5; ++i) {
        setTimeout(() => console.log(i), 0)
}
// 会输出0、1、2、3、4
```
这种每次迭代声明一个独立变量实例的行为适用于所有风格的for循环，包括for-in和for-of循环。

## const
const和let基本类似，但是用const声明的变量必须同时初始化变量，且这个变量的值**不可修改**，也**不可重复声明**，**作用域也是块级作用域**。

```js
const level = 1
level = 2  // 报错,不可修改
const level = 3  //报错，不可重复声明

const lastName = 'kyree'
if(true)  {
 const lastName = 'jack' //块级作用域
}
console.log(lastName)  //kyree
```
const声明的限制只适用于它只想的变量的引用。
如果const变量引用的是一个对象，那么修改这个对象里面的属性是可以的。

```js
const person = 'kyree'
 person.weight = '62kg'
```
## 声明风格
### 不使用var
使用var声明会导致很多问题，自己编写的代码中只使用let和const声明变量有助于提升代码的质量，因为变量有了明确的作用域、声明位置和不变的值。
### 优先使用const
const声明的变量不会改变，也可以避免不合法的赋值操作，这样可以在开发的时候预期范围内保证有些变量的值永远不会变。

# 数据类型
## typeof操作符
因为ECMAScript的类型是松散的，所以需要一种手段来确定任意变量的数据类型。typeof就是来确定任意变量的数据类型，它返回的永远是一个string类型的数据。

* "undefined"表示值未定义;
* "boolean"表示值为布尔值;
* "string"表示值为字符串;
* "number"表示值为数值;
* "object"表示值为对象(而不是函数)或 null;
* "function"表示值为函数;
* "symbol"表示值为符号。


```js
console.log(typeof('hello')) // string
let newFn = Function()
console.log(typeof(newFn))  //function
console.log(typeof(200))  //number
console.log(typeof(false))  //boolean
let sym = Symbol()
console.log(typeof(sym))  //symbol
console.log(typeof(undefined)) //undefined
console.log(typeof(null))  //object
console.log(typeof({name:'kyree',age:18}))  //object
```

