
---
title: 【js红宝书】第2章：html中的JavaScript
date: 2020-12-23
tags:
      - JavaScript
      - js红宝书学习笔记
categories: 前端
index_img: https://img9.doubanio.com/view/subject/s/public/s33703494.jpg
banner_img: https://img9.doubanio.com/view/subject/s/public/s33703494.jpg
---
## \<script>元素的属性

将JavaScript方法引入HTML的主要方法是使用\<script>元素。
目前\<script>元素常用的属性有：
* crossorigin:配置相关请求的CORS（跨域资源共享）设置，默认不使用CORS，crossorigin="anonymous"配置文件请求不必设置凭据标志。crossorigin="use-credentials"表示设置凭据标志，发出的请求需要包含凭据。
* integrity:可选，允许比对接收到的资源和指定的加密签名以验证子资源的完整性。不匹配的话页面会报错，这个功能可以确保CDN不会提供恶意内容。
* src:可选，表示包含要执行的代码的外部文件。
* type:可选，表示代码块中脚本语言的内容类型。默认值为"text/javascript"，如果这个值是module，则代码会被当成ES6模块，这时候代码中才可以出现import和export关键字。

```js
//crossorigin需要设置凭据标志
<script crossorigin="use-credentials"></script>
//资源的响应头设置Access-Control-Allow-Credentials 为 true

//crossorigin不需要设置凭据标志
<script crossorigin></script>  
<script crossorigin=""></script>
<script crossorigin="anonymous"></script>

//integrity 

/*值分成两个部分，第一部分指定哈希值的生成算法（目前支持 sha256、sha384 及 sha512），第二部分是经过 base64 编码的实际哈希值，两者之间通过一个短横（-）分割。*/
<script src="https://demo.com/demo.js"
integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
crossorigin="anonymous"></script>
//sha384 前缀说明使用的是 sha384 哈希方法
//哈希值部分：oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC
```
type="module"的用法
```js
//export.js
export default function fn1 (){
    console.log('打印export内容')
}

//import.js
import fn1 from './export.js'
console.log("import文件内容")
fn1()

//html 文件引入
<script type="module">
        import fn1 from './export.js'
        fn1()
    </script>
<script src="./import.js" type="module"></script>

//控制台输出
//打印export内容
//import文件内容
//打印export内容

//当不加type="module"时
<script src="./import.js" ></script>
//报错信息：Uncaught SyntaxError: Cannot use import statement outside a module
```
## \<script>元素的执行
要在html文件中执行JavaScript代码，直接把代码放在\<script>元素中就行。

```js
  <script>
        function sayHello(){
            console.log('Hello')
        }
        sayHello()
        
    </script>
```
包含在\<script>内的代码会被从上到下去执行，在\<script>元素中的代码被计算完成之前，页面的其余内容不会被加载，也不会被显示。

要执行外部文件的JavaScript，必须使用标签的src属性，这个属性的值是一个URL，指向包含JavaScript代码的文件，

```js
 <script src="example.js"></script>
```
在执行外部JavaScript文件时，页面也会被阻塞，阻塞时间也包含下载这个文件的时间。
**注意：**使用了src属性的\<script>元素不应该在标签内再包含其他JavaScript代码。**如果两者都提供的话，浏览器只会下载文件并执行外部文件，忽略行内代码。**

## \<script>标签的位置
现在为了保证页面加载和渲染的速度，通常会把所有的JavaScript引用放在<body>元素中的最后面。

```js
 <!DOCTYPE html>
    <html>
<head>
<title>Example HTML Page</title> </head>
<body>
<!-- 这里是页面内容 -->
<script src="example1.js"></script> 
<script src="example2.js"></script> </body>
</html>

```
这样的话，页面会在执行JavaScript代码之前渲染好页面，用户会感觉页面加载更快了，因为浏览器显示空白页面的时间变短了。

## 推迟执行脚本
HTML4.0.1为\<script>元素定义了一个叫defer的属性，这个属性表示脚本在执行的时候不会改变页面的结构。也就是说，脚本会被延迟到整个页面都解析完毕之后再运行。**立即下载但最后执行**。

```js
<body>
    <script defer src="./defer.js"></script>  
</body>
```
HTML5规定，defer属性只对外部文件才有效。
## 异步执行脚本
HTML5为\<script>元素定义了async属性，从改变脚本处理方式上看，async属性和defer类似。两者都只能用于外部文件引用，都会让浏览器立即开始下载。但是，和defer不同的是，标记为async的脚本并不保证能按照它们出现的次序执行。

```js
<body>
    <script async src="./defer01.js"></script>  
    <script async src="./defer02.js"></script>  
</body>
```
上面代码中，第二行代码可能会先于第一行代码来执行，双方之间没有依赖关系。所以异步脚本不应该在加载期间修改DOM.
## 动态加载脚本
除了使用\<script>标签，还有其它方式来加载脚本。因为JavaScript可以使用DOM API ，所以通过向DOM中动态添加script元素，同样可以加载指定的脚本。
```js
   <script>
        let script  = document.createElement('script')
        script.src = './extra.js'
        document.head.appendChild(script)
    </script>
```
在把HTMLElement元素添加到DOM且执行到这段代码前不会发送请求，这种方式默认情况下是以异步加载的没相当于添加了async属性。
但并不是所有浏览器都支持async属性。如果要统一动态脚本的加载行为，可以将async属性设置成false 表示同步加载
```js
   <script>
        let script  = document.createElement('script')
        script.src = './extra.js'
        script.async = false
        document.head.appendChild(script)
    </script>
```
这种方式获取资源对浏览器预加载器是不可见的，这回严重影响需要加载的资源在资源获取队列中的优先级，影响程序的性能。解决方式就是可以再文档头部显示声明
```js
    <link rel="preload" href="extra.js">
```
## 小结
* 要使用外部JavaScript文件，必须将scr属性设置成包含文件的URL，文件可以跟网页在同一台服务器，也可以不在同一个域。
* 所有的\<script>元素会按照在网页中出现的顺序被执行
* 对不拖迟执行的脚本，浏览器会在完全执行完\<script>元素中的代码后，才会继续渲染页面剩下来的部分，所以通常应该把\<script>元素放在页面的<body>末尾处.
* 可以使用defer属性将脚本推迟到文档渲染完毕后再执行。执行的原则按照顺序执行。
* 可以使用async属性表示脚本不需要等待其他脚本的执行，同时不会阻止页面渲染，这就是异步加载。异步加载不能按照它们在页面中出现的顺序执行。
