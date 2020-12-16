---
title: 【重学JavaScript】js中的变量和数据类型
date: 2020-08-16
tags:
      - 编程
categories: JavaScript
index_img: http://img.datalearn.top/javascript.png
banner_img: http://img.datalearn.top/javascript.png
---

###  变量
> JavaScript中的变量(variable)可变的量，存储和代表不同值的东西

```js
// ES3语法
var a  = 12;
a = 13
a = 'hello'  //变量赋值的时候，不写var 语句也是有效的
console.log('a :>> ', a);  //'hello'
// 输出a的代表值为13

// ES6语法
 let b = 100;
 b = 200;
 
//const 创建的常量存储的值不能被修改
 const c = 300 
 
 //创建函数也相当于创建变量
 function F (){}
 //创建类也相当于创建变量
 class A{}
 //ES6的模块导入也可以创建变量
 import B from './B.js';
 //Symbol 创建唯一值
 let  n = Symbol(100)
 let  m = Symbol(100)
 n==m //false
```
### 变量的命名规范
- 严格区分大小写

```js
let  Test  = 100
console.log(test)//undefined
```
- 使用数字、字母、下划线、$，数字不能作为开头

```js
let  $box  //一般用jquery获取的以$开头
let  _box // 一般公共变量都是_开头
```
- 使用驼峰命名法：首字母小写，其余每一个有意义单词的首字母都要大写（命名尽可能语义化明显，使用英文单词）

```js
let studentInfo;
//常用的缩写：
//add(增加)/insert（插入）/create（创建）/new（新增）/update（修改）/delete、remove(删除)//get、query、select(查询)
```
- 不能使用关键字和保留字

```js
//当下有特殊含义的是关键字、未来可能会成为关键字的叫做保留字
// var class import let  if  else  ....
```
### 常用的数据类型
- 基本数据类型
    + 数字number:  常规数字和NaN
    + 字符串string:所有用单引号、单引号、反引号包起来的都是字符串
    + 布尔boolean：true和false
    + 空对象指针null
    + 未定义undefined 
- 引用数据类型
    + 对象数据类型object
        + {} 普通对象
        + [] 数组对象
        + /^((0\d{2,3}-\d{7,8})|(1[3584]\d{9}))$/ 正则对象
        +  Math 数学函数对象
        +  日期对象
        +  ....
    + 函数数据类型function
    
####  number数据类型
> 包含：常规数字、NaN

##### NaN
> not a number :不是一个数，但它隶属于数字类型

```js
// NaN和任何值（包括自己）都不相等：NaN != NaN
//所以不能用相等的方式判断是否为有效数字
console.log(NaN==NaN) //false
```

##### isNaN
> 检测一个值是否为非有效数字，如果不是有效数字返回true，反之是有效数字返回false

```js
console.log(isNaN(10))          //false
console.log(isNaN('a'))          //true
//在使用isNaN检测的时候，首先会验证检测的值是否为数字类型，如果不是，先基于Number()方法，把值转换为数字类型，然后再检测
console.log(isNaN('10'))          //false
```

##### 把其它类型转换为数字类型
- Number([val])
- parseInt/parseFloat([val],[进制]) ：也是转换为数字的方法，对于字符串来说，它是从左到右依次查找有效数字字符，直到遇到非有效数字字符，停止查找（不管后面是否还有数字，都不在找了），把找到的当做数字返回
-  ==进行比较的时候，可能要把其它类型值转换为数字

```js
// 把字符串转换为数字，只要字符串中包含任意一个非有效数字字符（第一个点除外）结果都是NaN,空字符串会变为数字零0
console.log(Number('12.5')) // 12.5
console.log(Number('12.5px')) // NaN
console.log(Number('12.5.5')) // NaN
console.log(Number(' ')) // 0
//布尔值转换为数字
console.log(Number(true))  //1
console.log(Number(false))  //0
console.log(isNaN(false))  // false

console.log(Number(null)) // 0
console.log(Number(undefined)) // NaN
//把引用数字类型转换为数字，是先基于toString方法转换为字符串，然后在转换为数字
// {}.toString()=>[object,object]=>NaN
console.log(Number({})) // NaN
// [].toString()=>' ' =0
console.log(Number([])) // 0
// [12].toString()=>' 12' =12
console.log(Number([12])) // 12
// [12，23].toString()=>' 12，23' =NaN
console.log(Number([12,23])) // NaN

//parseInt/parseFloat转换数字
let str = '12.5px'
console.log(Number(str))        // NaN
console.log(parseInt(str))        // 12
console.log(parseFloat(str))    // 12.5
```
#### String字符串数据类型
> 所有用单引号、双引号、反引号（`）都是字符串
> 

##### 把其他类型值转换为字符串
- [val].toString()
- 字符串拼接

```js
let a = 12
console.log(a.toString())  // '12'
console.log((NaN).toString()) // 'NaN'
console.log((true).toString()) // 'true'

//null 和 undefined是禁止直接toString
//(null).toString()  //报错
// 但是和undefined一样转换为字符串的结果就是'null'/'undefined'

//普通对象.toString()的结果是 "[object object]" 
//object.prototype.toString方法不是转换为字符串的，而是用来检测数据类型的

//字符串拼接
//四则运算法则中，除加法之外，其余都是数学计算，只有加法可能存在字符串拼接（一旦遇到字符串，则不是数学计算，而是字符串拼接）
console.log('10'+10);  // '1010'
console.log('10'-10);  //    0
console.log('10px'-10);  //    NaN

let b = 10 + null +true+[]+undefined+'前端'+null+[]+10+false
/*
10+null > 10+0 >10
10+true>10+1 >11
11+[] >11+' ' >'11'   空数组变为数字，先要变成空字符串，遇到字符串直接变为字符串拼接
'11'+undefined>'11undefined'
...
11undefined前端null10false

*/
console.log(b) // 11undefined前端null10false
```

#### boolean布尔数据类型
> 只有两个值 true/false

##### 把其它类型值转换为布尔类型
> 只有0、NaN、' ' 、null、undefined、五个之转换为false，其余都转换为true（没有任何特殊情况）

- Boolean([val])
- !//!!
- 条件判断

```js
console.log(Boolean(0))                 //false
console.log(Boolean(''))                 //false
console.log(Boolean('   '))               //true
console.log(Boolean(null))                //false
console.log(Boolean(undefined))             //false
console.log(Boolean([]))                    //true
console.log(Boolean([12]))                  //true

//!：取反（先转换为布尔值，然后取反）
//!!：取反再取反，只相当于转换为布尔值=== Boolean
console.log(!1)  //false
console.log(!!1) //true
//条件判断，如果条件只是一个值，是要把这个值转换为布尔类型，然后验证真假
if   (1)    {   //1  true
}
if ('3px' + 3 ) {  // '3px3'

}
if ('3px' + 3 ) {  // 'NaN'  > false

}
```
####  null / undefined
> null和undefined都代表的是没有

- null：意料之中（一般是开始不知道值，手动设置为null，后期再给予复制操作）

```js
let num = null // let num = 0 ; 一般最好用null作为初始的空值，因为零不是空值，他在栈内存中有自己的存储空间（占了位置）
num =12
```

- undefined:意料之外（不是我能决定的）

```js
let num //  创建变量没有赋值，默认值是undefined
num =12
```

#### object对象数据类型-普通对象
> {[key]:[value],...} 任何一个对象都是由零到多组键值对（属性名：属性值）组成的（并且属性名不能重复）

```js
let  person={
        name:'mike',
        age:'24',
        1:100
}
// 获取属性名对应的属性值
//  对象.属性名
//  对象[属性名]  > 属性名是数字或者字符串格式的
// 如果当前属性名不存在，默认的属性值是undefined
// 如果属性名是数字，则不能使用点的方式获取属性值
console.log(person.name)
console.log(person['age'])
console.log(person.sex)       //undefined
console.log(person[1])   // 100
console.log(person.1)   //  SyntaxError : 语法错误

// 设置属性名属性值
//  属性名不能重复，如果属性名已经存在，不属于新增属于修改属性值
person.weight = '65kg'
console.log(person.weight) // '65kg'

//删除属性
// 真删除：把属性彻底删掉
delete person[1];
// 假删除：属性还在，值为空
person.weight = null ;
```

#### Array数组特殊对象
```js
/*
数组是特殊的对象
1.我们中括号中设置的是属性值，它的属性名是默认生成的数字，
从零开始递增，而且这个数字代表每一项的位置，我们把其称
为 "索引" =》从零开始，连续递增，代表每一项位置的数字属性名
2.天生默认一个属性名length，存储数组的长度
*/
let  array = [12,'哈哈',true,13]
console.log(array.length)
console.log(array['length'])
//第一项索引0，最后一项索引array.length-1
console.log(array[0)  //12
console.log(array[array.length-1])  //13
//向数组末尾追加内容
array[array.length] = 100

```
###数据类型之间的区别
 
```js
let a  = 12;
let b = a;
b = 13
console.log(a)  //12

let n = {
  name:'前端'
}
let  m = n;
m.name = '学习'
console.log(n.name)  //学习

let n = [10,20]
let  m = n;
let x = m;
m[0]=100
x = [30,40];
x[0] = 200;
m = [50,60]
m = x
m[1] = 300
n[2] = 400
console.log(n,m,x) //[ 100, 20, 400 ] [ 200, 300 ] [ 200, 300 ]

let a = {
    n:1
}
let b = a;
a.x = a = {
    n:2 
}
console.log(a.x)  //undefined
console.log(b)   // { n: 1, x: { n: 2 } }
```

基本类型：按值操作（直接操作的是值），所以也叫做值类型
引用类型；操作的是堆内存的地址（按引用地址操作）
### js数据类型的检测
 - typeof[val]: 用来检测数据类型的运算符
 - instanceof: 用来检测当前实例是否属于某个类
 -  constructor: 基于构造函数检测数据类型（基于类的方式）
 -  Object.prototype.toString.call(): 检测数据类型最好的办法
 
 
```js
/*
基于typeof检测出来的结果
1.首先是一个字符串
2.字符串中包含对应的类型
局限性
1.typeof null=》"object"  但是null并不是对象
2.基于typeof无法细分出当前值是普通对象还是数组对象等，
因为只要是对象数据类型，返回结果都是‘object’
*/
console.log(typeof 1) ;  // 'number'
let a = NaN
console.log(typeof  a); //  'number'
console.log(typeof  '12'); //   'string'
console.log(typeof  true); //   'boolean'
console.log(typeof  null); //   'object'
console.log(typeof undefined); //   'undefined'
console.log(typeof []); //   'object'
console.log(typeof  function fn(){}); //   'function'

//因为typeof检测的结果都是字符串，所以只要两个及以上同时检测，最后结果必然是'string'
console.log(typeof typeof typeof []); //   'string'
```