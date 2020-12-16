---
title: Vue学习笔记（基础一）
date: 2019-12-21 
tags:
    - 前端
    - 学习
    - Vue
index_img: http://img.datalearn.top/vue-index.jpeg
banner_img: http://img.datalearn.top/vue-index.jpeg
---
###  Vue的声明式渲染
下面是最简单的一个Vue声明式渲染的例子，只需要在html文件中引入vue的js文件。
然后敲下以下代码，你就成功创建了vue的第一个应用。

```html
<body>
    <div id="app">
        <div >{{message}}</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                message: 'Hello Vue!'
            },
        })
    </script>
</body>

```
现在数据`message`和 DOM 已经被建立了关联，所有东西都是响应式的。打开浏览器的  控制台 ，并修改 vm.message 的值，可以看到页面上的数据也进行了更新。

![](http://img.datalearn.top/learnvue1-1.gif)

当然上面的代码`<div >{{message}}</div>`也可以写成`<div v-text="message"></div>`
Vue的大部分指令都是以`v-`开头的，比如说：`v-text`、`v-for`、`v-model`、`v-if`等，这些指令在下面的章节中都会讲到。

###  Vue的模板语法

#### 插值-绑定文本数据
数据绑定最常见的形式就是使用(双大括号) 语法 的文本插值,当然也可以使用`v-text`进行简单的数据绑定。

```html
<body>
    <div id="app">
        <div >{{data1}}</div>
        <div v-text="data2" ></div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
               data1:'用{{}}进行绑定的',
               data2:'用v-text进行绑定的'
            },
        })
    </script>
</body>
```

打开控制台可以看到数据已经成功地被渲染到DOM结构中去了。

![](http://img.datalearn.top/learnvue1-2.png)

在声明式渲染中提到过，vue的数据绑定是响应式的，数据发生改变，页面上的数据也会作出改变，但是你如果用`v-once`进行数据绑定。当数据改变时，插值处的内容不会更新，这个指令的意思是你只能进行一次的插值。

```html
<body>
    <div id="app">
        <div v-once>{{data3}}</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
               data3:'我不会被改变',
            },
        })
    </script>
</body>
```
#### 绑定原始 HTML代码
双大括号或者`v-text`指令会将html代码解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，vue提供了 `v-html `指令：

```html
<body>
    <div id="app">
        <div >{{data4}}</div>
        <div v-text="data4" ></div>
        <div v-html="data4"></div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
               data4:'<h1>我是一个h1的标题<h1>',
            },
        })
    </script>
</body>
```
在浏览器中，可以发现使用`v-html `指令绑定的数据，已经成功将数据渲染成html代码内容

![](http://img.datalearn.top/learnvue1-3.png)


####  在指令中使用 JavaScript 表达式
Vue中对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。你可以绑定一个求和运算公式、拆分数组或者三元表达式。

```html
<body>
    <div id="app">
        <div >计算5+6的值：{{data5+data6}}</div>
        <div >三元表达式：{{data7 ? 'YES' : 'NO' }}</div>
        <div>拆分重排数组：{{ data8.split('-').reverse().join('.') }}</div>
    
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
               data5:5,
               data6:6,
               data7:true,
               data8:'2019-12-20'
            },
        })
    </script>
</body>
```
在浏览器的DOM结构中你可以看到，逻辑执行完成后的结果都已经成功的被渲染到绑定的元素上去了。

![](http://img.datalearn.top/learnvue1-4.png)

#### Vue中的指令
Vue中提供了很多指令，你可以通过这些指令很简单地实现一些功能。指令就像单个的JavaScript 表达式。当指令的值改变时，将其产生的连带影响，响应式地作用于 DOM结构中。

```html
<body>
    <div id="app">
        <div v-if="seen">现在你看到我了</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
               seen:true
            },
        })
    </script>
</body>
```

上面代码中当`seen`的值为`true`时，会插入对应的`div`元素,当`seen`的值为`false`时，vue会移除绑定的`div`元素,在浏览器中的控制台键入`vm.seen=false`,即可看到页面中的元素消失了。

![](http://img.datalearn.top/learnvue1-5.gif)

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。比如说a标签的`href`则可以用`:href`来表示，img标签的`src`可以用`:src`来表示。

```html
<body>
    <div id="app">
        <a :href="baiduhref">跳转百度</a>
        <img :src="baiduimage" >
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
               baiduhref:'http://www.baidu.com',
               baiduimage:'https://m.baidu.com/se/static/img/iphone/logo_web.png'
            },
        })
    </script>
</body>
```
在浏览器中，打开控制台，可以很清楚的看到，跳转地址的链接和图片地址被绑定到对应的元素上去了。

![](http://img.datalearn.top/learnvue1-6.png)

###  计算属性computed和侦听器watch
#### 计算属性computed
Vue的模板语法的表达式非常便利，用于简单运算也非常方便。但是如果在模板中放入太多的逻辑会让模板过重且难以维护，也尽量减少在模板内书写逻辑。比如说在模板内写入之前的数组拆分排序代码：

```html
  <div>拆分重排数组：{{ data8.split('-').reverse().join('.') }}</div>
```

所以，vue建议任何复杂的逻辑，都应该使用计算属性computed来实现。

```html
<body>
    <div id="app">
        <div v-text="fullWorld"></div>
    </div>
    
    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                first: "hello",
                last: "world",
            },
            computed: {
                fullWorld(){
                        return this.first + " , " + this.last;
    
                }
            },
        })
    </script>
</body>
```
在页面中你可以看到输出了`hello , world`,说明计算属性中绑定的数据，已经成功地被渲染到DOM结构中去了。这里声明了一个计算属性 `fullWorld`，并且在模板中`div`标签绑定了这个计算属性`fullWorld`

![](http://img.datalearn.top/learnvue2-1.png)

计算属性 `fullWorld`的值 依赖于 `vm.first`和`vm.last`的值，所以当`vm.first`和`vm.last`的值 其中一个发生改变时，所有依赖 `fullWorld`的绑定的值也会更新,这就是计算属性的响应式更新。

![](http://img.datalearn.top/learnvue2-2.gif)

#### 计算属性的getter和setter
计算属性默认自带getter和setter两个方法，计算逻辑默认在getter方法中执行。
所以下面两段代码结果是相同的。

```html
   computed: {
          fullWorld() {
            return this.first + " , " + this.last;
          }
        }
```
在get中执行

```html
        computed: {
          fullWorld:{
            get(){
                return this.first + " , " + this.last;
            }
          }
        }
```
计算属性中setter方法会接收一个参数 `set(value)`——即计算属性的值 ，你可以通过这个值，来对它进行一些处理，这是个异步的方法，所以setter中处理后的方法并不会立即渲染到DOM结构中去。

```html
<body>
    <div id="app">
        <div > {{resultWord}}</div>
        <button @click="handleClick">改变数值</button>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                firstWord:"a",
                secondWord:"b",
            },
            computed: {
                resultWord:{
                    get(){
                       return this.resultWord = this.firstWord + this.secondWord
                    },
                    set(value){
                        console.log("当前resultWord的值是:"+value)
                        this.firstWord = value+"c";
                        console.log("计算属性setter中firstWord的值:"+this.firstWord)
                    }
                }
            },
            methods: {
                handleClick(){
                    this.secondWord = "z"
                    console.log("执行方法后resultWord的值:"+this.resultWord)
                }
            },

        })
    </script>
</body>
```
在浏览器中执行代码，你可以清楚的看到控制台中输出了`setter`执行过后，`resultWord`计算属性的所依赖的的`firstWord`值是`abc`，但是页面显示的依然是getter中执行后的结果`ab`。

![](http://img.datalearn.top/learnvue2-3.gif)

#### 侦听器watch
虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，用watch比较合适。
watch 的一个特点是，最初绑定的时候是不会执行的，要等到数据发生变化时才会执行监听计算。

```HTML
<body>
    <div id="app">
        <div > fullName的值：{{fullName}}</div>
        <div > firstName的值：<input type="text" v-model="firstName"></div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                firstName:"Michael",
                lastName:"Jackson",
            },
            watch: {
                firstName(){
                    this.fullName = this.firstName+ this.lastName
               }
            },

        })
    </script>
</body>
```
上面代码在页面初次渲染的时候，watch没有执行，而当firstName发生改变的时候，watch才会执行监听方法。

![](http://img.datalearn.top/learnvue2-4.gif)

> ` 计算属性computed`和`侦听器watch`其实都是vue对监听器的实现，只不过computed主要用于对同步数据的处理，watch则主要用于观测某个值的变化去完成一段开销较大的复杂业务逻辑。能用computed的时候优先用computed，避免了多个数据影响其中某个数据时多次调用watch的尴尬情况。
###  Class 和Style样式的 绑定
我们在开发中，经常会涉及到改变元素样式的功能，这就需要我们动态的去给元素绑定和改变样式，vue也对这种需求提供了很友好的支持。
#### 使用对象语法绑定class
在Vue中,我们可以通过传给 `:class `一个对象，实现动态地切换 class

```html
<style>
    .active{
        color: green;
    }
</style>
<body>
    <div id="app">
       <div :class="{ active: isActive }">isACtive是true的时候我就是绿色的</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data() {
              return {
                isActive:false
              }
          }
        })
    </script>
</body>
```
上述代码中，active 这个 class 是否绑定到元素上了，取决于isActive的值是true还是false，当isActive是false的时候，可以看到元素中的class是空的，而当isActive的值变为true的时候，元素中的class便增加了一个active。

![](http://img.datalearn.top/learnvue3-1.gif)

当然，你也可以在元素中传入多个属性来动态切换多个 class。

```html
<body>
    <div id="app">
       <div :class="{ active: isActive,default:isDefault,danger:isDanger }">绑定多个属性</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data() {
              return {
                isActive:false,
                isDefault:true,
                isDanger:true,
              }
          }
        })
    </script>
</body>
```
此时，由于  isDefault和  isDanger的值为true，所以元素上绑定的class仅有default和danger。

![](http://img.datalearn.top/learnvue3-2.png)

当然，`:class` 指令也可以与普通的 `class `属性也是共存的，这时候，div元素的class便是default和danger。
`<div class="default" :class="{ danger:isDanger }">两种方式共存</div>`
上述代码便渲染成：
`<div class="default  danger"></div>`
当然，我们也可以用计算属性computed来进行绑定，比如以下示例代码：

```html
<style>
    .active{
        color: green;
    }
    .default{
        width: 100px;
        height: 20px;
        background: #999;
    }
    .danger{
        font-weight: 900;
    }
    .error{
        font-style: italic; 
    }
</style>
<body>
    <div id="app">
       <div class="active" :class="{ error:isError }">计算属性绑定</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data() {
              return {
                isActive:false,
                isDefault:true,
                isDanger:true,
              }
          },
          computed:{
              isError(){
                    return (this.isDefault && this.isDanger)
              }
          }
        })
    </script>
</body>
```
上述代码的意思为，由于`isDefault`和`isDanger`的值是相同的，都为`true`，所以计算属性中`this.isDefault && this.isDanger`的值也为`true`，所以isError的值为`true`，那么目前的元素的绑定class就是：

```html
<div class="active error">计算属性绑定</div>
```
而当  `isDefault`或者`isDanger`的其中一个值变为`false`，那么此时isError的值变成为了`false`，那么现在元素绑定的class便是：

```html
<div class="active ">计算属性绑定</div>
```
#### 使用数组语法绑定class
在Vue中，我们也可以通过一个数组传给`:class`，实现元素绑定一个calss列表：

```html
<style>
    .active{
        color: green;
    }
    .error{
        font-style: italic; 
    }
</style>
<body>
    <div id="app">
        <div  :class="[activeClass, errorClass]" >数组形式</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data() {
              return {
                activeClass:'active',
                errorClass:'error'
              }
          }
        })
    </script>
</body>
```
此时，对应div元素便渲染为：

```html
<div class="active error"></div>
```
当然也可以根据条件切换列表中的 class，可以用三元表达式来实现：

```html
<div :class="[isActive ? activeClass : errorClass,activeClass]">根据条件切换列表中的class</div>
```
此时，页面中的对应div元素绑定的class便为：
```html
<div class="error active">根据条件切换列表中的class</div>
```
当然，在数组语法中也是可以用对象语法来实现动态绑定class的：
```html
<div v-bind:class="[{ active: isActive }, errorClass]">数组语法中使用对象语法</div>
```
#### 使用对象语法绑定style样式
在Vue中可以使用`:style`语法进行样式的绑定:

```html
<body>
    <div id="app">
        <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">style的绑定</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                activeColor: "red",
                fontSize: 20,
            },
        })
    </script>
</body>
```
此时，页面中对应的div元素的样式便被渲染成为：

```html
<div style="color: red; font-size: 20px;">style的绑定</div>
```
当然也可以直接绑定到一个样式对象中去：

```html
<body>
    <div id="app">
        <div :style="styleObject">直接绑定一个样式对象</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                styleObject: {
                    color: 'blue',
                    fontSize: '16px'
        }
            },
        })
    </script>
</body>
```
这样看起来也更加清晰明了，效果如下图所示：

![](http://img.datalearn.top/learnvue3-3.png)

当然，对象语法也可以结合返回对象的计算属性使用：

```html
<body>
    <div id="app">
        <div :style="extraStyle">计算属性绑定</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            computed: {
               extraStyle(){
                   return {
                    color: 'green',
                    fontSize: '20px'
                   }
               }
            },
        })
    </script>
</body>
```
上述代码执行效果如下图所示：

![](http://img.datalearn.top/learnvue3-4.png)

#### 使用数组语法绑定style样式
`:style `的数组语法可以将多个样式对象应用到同一个元素上：

```html
<body>
    <div id="app">
        <div :style="[ object1, object2]">数组语法绑定多个对象</div>
    </div>

    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                object1:{
                    color:'grey'
                }
            },
            computed: {
               object2(){
                    return {
                        fontSize:'24px'
                    }
               }
            },
        })
    </script>
</body>
```
那么此时页面中对应style的样式便是：

```html
<div style="color: grey; font-size: 24px;">数组语法绑定多个对象</div>
```
