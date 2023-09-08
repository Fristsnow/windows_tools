---
title: 02-Vue的系统指令
publish: true
---

<ArticleTopAd></ArticleTopAd>





> 本文最初发表于[博客园]()，并在[GitHub](https://github.com/smyhvae/Web)上持续更新**前端的系列文章**。欢迎在GitHub上关注我，一起入门和进阶前端。

> 以下是正文。


## 本文主要内容

- 插值表达式 {{}}

- v-cloak

- v-text

- v-html

- v-bind

- v-on

- 举例：文字滚动显示（跑马灯效果）

- v-on的事件修饰符



## Vue初体验

新建一个空的项目，引入`vue.js`文件。写如下代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--1、导入Vue的包-->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<!--这个div区域就是MVVM中的 View-->
<div id="div1">
    {{name}}
</div>
</body>

<script>
    // 2、创建一个Vue的实例
    //new出来的对象就是MVVM中的 View Module（调度者）
    var myVue = new Vue({
        el: '#div1', //当前vue对象将接管上面的div1区域
        data: {//data就是MVVM中的 module
            name: 'smyhvae'
        }
    });
</script>
</html>
```


显示效果：

![](http://img.smyhvae.com/20180313_0955.png)

如果我们在控制台输入`myVue.$data.name = 'haha'`，页面会**自动更新**name的值。意思是，当我们直接修改data数据，页面会自动更新，而不用去操作DOM。


下面来讲一下Vue的各种系统指令。


## 插值表达式 {{}}

数据绑定最常见的形式就是使用 “Mustache” 语法（双大括号）的文本插值。例如：

```html
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 msg 属性（msg定义在data对象中）的值。
无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会**自动更新**。

`{{}}`对JavaScript 表达式支持，例如：

```javascript
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ name == 'smyhvae' ? 'true' : 'false' }}

{{ message.split('').reverse().join('') }}
```


但是有个限制就是，每个绑定都**只能包含单个表达式**，如下表达式无效：

```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```


代码举例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <span>content:{{name}}</span>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                name: 'smyhvae'
            }
        })
    </script>

</body>

</html>
```

运行结果：


![](http://img.smyhvae.com/20180506_2240.png)


## v-cloak

`v-cloak`：保持和元素实例的关联，直到结束编译后自动消失。

v-cloak指令和CSS 规则一起用的时候，能够**解决插值表达式闪烁的问题**（即：可以隐藏未编译的标签直到实例准备完毕）。

就拿上一段代码来举例，比如说，`{{name}}`这个内容，**在网速很慢的情况下，一开始会直接显示`{{name}}`这个内容**，等网络加载完成了，才会显示`smyhvae`。那这个**闪烁的问题**该怎么解决呢？

解决办法是：通过`v-cloak`隐藏`{{name}}`这个内容，当加载完毕后，再显示出来。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>

    /*2、在样式表里设置：只要是有 v-cloak 属性的标签，我都让它隐藏。
    直到 Vue实例化完毕以后，v-cloak 会自动消失，那么对应的css样式就会失去作用，最终将span中的内容呈现给用户 */
    [v-cloak] {
      display: none;
    }
  </style>
</head>

<body>
  <div id="app">
    <!-- 1、给 span 标签添加 v-cloak 属性 -->
    <span v-cloak>{{name}}</span>

  </div>
</body>

<script src="vue2.5.16.js"></script>
<script>
  new Vue({
    el: '#app',
    data: {
      name: 'smyhvae'
    }
  });
</script>

</html>

```


## v-text

v-text可以将一个变量的值渲染到指定的元素中。例如：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <!--vue的版本：2.5.15-->
  <script src="vue.js"></script>
</head>

<body>
  <div id="div1">
    <span v-text="name"></span>
  </div>
</body>

<script>
  new Vue({
    el: '#div1',
    data: {
      name: 'hello smyhvae'
    }
  });
</script>

</html>
```

结果：


![](http://img.smyhvae.com/20180313_1645.png)

### 插值表达式和 v-text 的区别

```html
  <!-- 插值表达式 -->
  <span>content:{{name}}</span>

  <!-- v-text -->
  <span v-text="name">/span>
```

**区别1**： v-text 没有闪烁的问题，因为它是放在属性里的。

**区别2** :插值表达式只会替换自己的这个占位符，并不会把整个元素的内容清空。v-text 会**覆盖**元素中原本的内容。

为了解释区别2，我们来用代码举例：

```html
  <!-- 插值表达式 -->
  <p>content:++++++{{name}}------</p>

  <!-- v-text -->
  <p v-text="name">------++++++</p>
```

上方代码的演示结果：

![](http://img.smyhvae.com/20180506_2320.png)

其实，第二行代码中，只要浏览器中还没有解析到`v-text="name"`的时候，会显示`------++++++`；当解析到`v-text="name"`的时候，name的值会直接替换`------++++++`。


## v-html


`v-text`是纯文本，而`v-html`会被解析成html元素。

注意：使用v-html渲染数据可能会非常危险，因为它很容易导致 XSS（跨站脚本） 攻击，使用的时候请谨慎，能够使用{{}}或者v-text实现的不要使用v-html。

代码举例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <p>{{msg}}</p>
        <p v-text="msg"></p>
        <p v-html="msg"></p>

    </div>
    <script src="vue2.5.16.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: '<h1>我是一个大大的h1标题</h1>'
            }
        })
    </script>

</body>

</html>
```

运行结果：

![](http://img.smyhvae.com/20180506_2330.png)


## v-bind：属性绑定机制

`v-bind`：用于绑定**属性**。

比如说：

```html
    <img v-bind:src="imageSrc +'smyhvaeString'">

    <div v-bind:style="{ fontSize: size + 'px' }"></div>
```


上方代码中，给属性加了 v-bind 之后，属性值里的整体内容是**表达式**，属性值里的`imageSrc`和`size`是Vue实例里面的**变量**。

也就是说， v-bind的属性值里，可以写合法的 js 表达式。

上面两行代码也可以简写成：

```html
    <img :src="imageSrc">

    <div :style="{ fontSize: size + 'px' }"></div>
```

**举例：**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
  </style>
</head>

<body>
  <div id="div1">
    <!-- value里的值只是简单的字符串 -->
    <input type="text" value="name">
    <!-- 加上 v-bind 之后，value里的值是 Vue 里的变量 -->
    <input type="text" v-bind:value="name">
    <!-- 超链接后面的path是 Vue 里面的变量 -->
    <a v-bind="{href:'http://www.baidu.com/'+path}">超链接</a>

  </div>
</body>

<script src="vue.js"></script>
<script>
  new Vue({
    el: '#div1',
    data: {
      name: 'smyhvae',
      path: `2.html`
    }
  });
</script>

</html>

```

上面的代码中，我们给`value`这个属性绑定了值，此时这个值是一个变量。

效果：

![](http://img.smyhvae.com/20180313_1745.png)



## v-on：事件绑定机制

### `v-on:click`：点击事件

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <!--vue的版本：2.5.15-->
  <script src="vue.js"></script>
</head>

<body>
  <!--这个div区域就是MVVM中的 View-->
  <div id="div1">
    <!-- 给button节点绑定按钮的点击事件 -->
    {{name}}
    <button v-on:click="change">改变name的值</button>
  </div>


</body>

<script>
  //new出来的对象就是MVVM中的 View Module
  var myVue = new Vue({
    el: '#div1', //当前vue对象将接管上面的div区域
    data: { //data就是MVVM中的 module
      name: 'smyhvae'
    },

    //注意，下方这个 `methods` 是Vue中定义方法的关键字，不能改
    //这个 methods 属性中定义了当前Vue实例所有可用的方法
    methods: {
      change: function() { //上面的button按钮的点击事件
        this.name += '1';
      }
    }
  });
</script>

</html>
```


上方代码中，我们给button按钮绑定了点击事件。注意，这个button标签要写在div区域里（否则点击事件不生效），因为下方的View module接管的是div区域。

### `v-on`的简写形式

例如：

```html
    <button v-on:click="change">改变name的值</button>
```

可以简写成：

```html
    <button @click="change">改变name的值</button>
```


### v-on的常用事件

v-on 提供了click 事件，也提供了一些其他的事件。


- v-on:click

- v-on:keydown

- v-on:keyup

- v-on:mousedown

- v-on:mouseover

- v-on:submit

- ....

## 举例：文字滚动显示（跑马灯效果）

我们利用上面几段所学的内容，做个跑马灯的小例子。要实现的效果是：类似于LED屏幕上，滚动显示的文字。

**文字滚动显示的思路**：

（1）每次点击按钮后，拿到 msg 字符串，然后调用字符串的`substring`来进行字符串的截取操作，把第一个字符截取出来，放到最后一个位置即可。这就实现了滚动的效果。
（2）为了实现文字**自动连续滚动**的效果，需要把步骤（1）中点击按钮的操作，放到**定时器**中去。

我们先来看一下 点击事件里的代码改怎么写。

**步骤 1**：每次点击按钮，字符串就滚动一次。代码如下：

```javascript
    startMethod: function () {
        // 获取 msg 的第一个字符
        var start = this.msg.substring(0, 1);
        // 获取 后面的所有字符
        var end = this.msg.substring(1);
        // 重新拼接得到新的字符串，并赋值给 this.msg
        this.msg = end + start;
    }
```

**步骤2**：给上面的操作添加定时器。代码如下：

```javascript
    startMethod: function () {
        var _this = this;
        //添加定时器：点击按钮后，让字符串连续滚动
        setInterval(function () {
            // 获取 msg 的第一个字符
            var start = _this.msg.substring(0, 1);
            // 获取 后面的所有字符
            var end = _this.msg.substring(1);
            // 重新拼接得到新的字符串，并赋值给 this.msg
            // 注意： VM实例，会监听自己身上 data 中所有数据的改变，只要数据一发生变化，就会自动把 最新的数据，从data 上同步到页面中去
            _this.msg = end + start;
            console.log(_this.msg);
        }, 400);
    }
```

上面的代码中，我们发现，如果在定时器中直接使用this，这个this指向的是window。为了解决这个问题，我们是通过`_this`来解决了这个问题。

另外，我们还可以利用箭头函数来解决this指向的问题，因为箭头函数总的this指向，会继承外层函数的this指向。如下。

**步骤2的改进版**：用箭头函数来改进定时器，解决this指向的问题。代码如下：

```javascript
    startMethod: function () {
        //添加定时器：点击按钮后，让字符串连续滚动
        setInterval(() => {
            // 获取 msg 的第一个字符
            var start = this.msg.substring(0, 1);
            // 获取 后面的所有字符
            var end = this.msg.substring(1);
            // 重新拼接得到新的字符串，并赋值给 this.msg
            // 注意： VM实例，会监听自己身上 data 中所有数据的改变，只要数据一发生变化，就会自动把 最新的数据，从data 上同步到页面中去
            this.msg = end + start;
            console.log(_this.msg);
        }, 400);
    }
```


**步骤3**：停止定时器。如下：

我们还需要加一个按钮，点击按钮后，停止文字滚动。也就是停止定时器。

提示：我们最好把定时器的id放在全局的位置（放到data里），这样的话，开启定时器的方法和停止定时器的方法，都可以同时访问到这个定时器。

代码如下：

```javascript
    data: {
        msg: '千古壹号，前端教程～～～',
        intervalId: null
    },
    methods: {
        startMethod: function () {
            //添加定时器：点击按钮后，让字符串连续滚动
            this.intervalId = setInterval(() => {  //【注意】这个定时器的this，一定不要忘记了
                // 获取 msg 的第一个字符
                var start = this.msg.substring(0, 1);
                // 获取 后面的所有字符
                var end = this.msg.substring(1);
                // 重新拼接得到新的字符串，并赋值给 this.msg
                // 注意： VM实例，会监听自己身上 data 中所有数据的改变，只要数据一发生变化，就会自动把 最新的数据，从data 上同步到页面中去
                this.msg = end + start;
                console.log(_this.msg);
            }, 400);
        },

        stopMethod: function () {
            //停止定时器：点击按钮后，停止字符串的滚动
            clearInterval(this.intervalId);
        }
    }
```


**【重要】步骤4**：一开始的时候，还需要判断是否已经存在定时器。如下：

步骤3中的代码，虽然做了停止定时器的操作，但是有个问题：在连续多次点击“启动定时器”按钮的情况下，此时再点击“停止定时器”的按钮，是没有反应的。因此，我们需要改进的地方是：

- **在开启定时器之前，先做一个判断**：如果定时器不为 null，就不继续往下执行了（即不再开启新的定时器），防止开启了多个定时器。

- **停止定时器的时候，虽然定时器停止了，但定时器并不为 null**。因此，最后我们还需要手动将定时器设置为null。这样，才能恢复到最初始的状态。

**完整版代码**：

针对上面的四个步骤，为了实现这个案例，完整版代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <p>{{msg}}</p>
        <input type="button" value="跑马灯走起" @click="startMethod">
        <input type="button" value="跑马灯停止" @click="stopMethod">

    </div>
    <script src="vue2.5.16.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: '千古壹号，前端教程～～～',
                intervalId: null
            },
            methods: {
                startMethod: function () {

                    //【重要】在开启定时器之前，先进行判断，避免出现多个定时器
                    //如果出现了多个定时器，后面的“停止定时器”操作是无效的
                    if (this.intervalId != null) return; //【注意】这个定时器的this，一定不要忘记了

                    //添加定时器：点击按钮后，让字符串连续滚动
                    this.intervalId = setInterval(() => {
                        // 获取 msg 的第一个字符
                        var start = this.msg.substring(0, 1);
                        // 获取 后面的所有字符
                        var end = this.msg.substring(1);
                        // 重新拼接得到新的字符串，并赋值给 this.msg
                        // 注意： VM实例，会监听自己身上 data 中所有数据的改变，只要数据一发生变化，就会自动把 最新的数据，从data 上同步到页面中去
                        this.msg = end + start;
                        console.log(this.msg);
                    }, 400);
                },

                stopMethod: function () {
                    // 停止定时器：点击按钮后，停止字符串的滚动
                    clearInterval(this.intervalId);
                    // 每当清除了定时器之后，需要重新把 intervalId 置为 null
                    this.intervalId = null;
                }
            }
        })
    </script>

</body>

</html>
```


**上方代码的总结**：

- 在Vue的实例中，如果想要获取data里的属性、methods里面的方法，都要通过`this`来访问。这里的**this指向的是Vue的实例对象**。

- VM实例，会监听自己身上 data 中所有数据的改变，只要数据一发生变化，就会自动把最新的数据，从 data 上同步到页面中去。这样做 的好处是：**程序员只需要关心数据，不需要考虑如何重新渲染DOM页面；减少DOM操作**。

- 在调用定时器 id 的时候，代码是`this.intervalId`，这个`this`一定不要漏掉了。


## 我的公众号

想学习<font color=#0000ff>**更多技能**</font>？不妨关注我的微信公众号：**千古壹号**（id：`qianguyihao`）。

扫一扫，你将发现另一个全新的世界，而这将是一场美丽的意外：

![](http://img.smyhvae.com/2016040102.jpg)

