---
title: 03-v-on的事件修饰符
publish: true
---

<ArticleTopAd></ArticleTopAd>





## 前言

本文主要内容：

- v-model

- v-for

- v-if

- v-show

## v-model：双向数据绑定


> 重点：**双向数据绑定，只能用于表单元素，或者用于自定义组件**。

之前的文章里，我们通过v-bind，给`<input>`标签绑定了`data`对象里的`name`属性。当`data`里的`name`的值发生改变时，`<input>`标签里的内容会自动更新。

可我现在要做的是：我在`<input>`标签里修改内容，要求`data`里的`name`的值自动更新。从而实现双向数据绑定。该怎么做呢？这就可以利用`v-model`这个属性。

**区别**：

- v-bind：只能实现数据的**单向**绑定，从 M 自动绑定到 V。

- v-model：只有`v-model`才能实现**双向**数据绑定。注意，v-model 后面不需要跟冒号，

**注意**：v-model 只能运用在**表单元素**中，或者用于自定义组件。常见的表单元素包括：input(radio, text, address, email....) 、select、checkbox 、textarea。

代码举例如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="vue.js"></script>
</head>
<body>
<div id="app">

    <form action="#">
    	<!-- 将 input 标签中的value值双向绑定到 Vue实例中的data。注意，v-model 后面不需要跟冒号 -->
        <input type="text" id="username" v-model="myAccount.username">
        <input type="password" id="pwd" v-model="myAccount.userpwd">

        <input type="submit" v-on:click="submit1" value="注册">

    </form>
</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        //上面的标签中采用v-model进行双向数据绑定，数据会自动更新到data里面来
        data: {
            name: 'qianguyihao',
            myAccount: {username: '', userpwd: ''}
        },
        //在methods里绑定各种方法，根据业务需要进行操作
        methods: {
            submit1: function () {
                alert(this.myAccount.username + "  pwd=" + this.myAccount.userpwd);
            }
        }
    });
</script>

</html>
```

此时，便可实现我们刚刚要求的双向数据绑定的效果。

## v-model举例：实现简易计算器

题目：现在两个输入框，用来做加减乘除，将运算的结果放在第三个输入框。

实现代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="vue2.5.16.js"></script>
</head>

<body>
    <div id="app">
        <input type="text" v-model="n1">

        <select v-model="opt">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
        </select>

        <input type="text" v-model="n2">

        <input type="button" value="=" @click="calc">

        <input type="text" v-model="result">
    </div>

    <script>
        // 创建 Vue 实例，得到 ViewModel
        var vm = new Vue({
            el: '#app',
            data: {
                n1: 0,
                n2: 0,
                result: 0,
                opt: '+'
            },
            methods: {
                calc() { // 计算器算数的方法
                    // 逻辑判断：
                    switch (this.opt) {
                        case '+':
                            this.result = parseInt(this.n1) + parseInt(this.n2)
                            break;
                        case '-':
                            this.result = parseInt(this.n1) - parseInt(this.n2)
                            break;
                        case '*':
                            this.result = parseInt(this.n1) * parseInt(this.n2)
                            break;
                        case '/':
                            this.result = parseInt(this.n1) / parseInt(this.n2)
                            break;
                    }

                    //上面的逻辑判断，可能有点啰嗦，我们还可以采取下面的这种方式进行逻辑判断
                    // 注意：这是投机取巧的方式，正式开发中，尽量少用
                    // var codeStr = 'parseInt(this.n1) ' + this.opt + ' parseInt(this.n2)'
                    // this.result = eval(codeStr)
                }
            }
        });
    </script>
</body>

</html>
```

注意上方代码中的注释，可以了解下`eval()`的用法。

## Vue中通过属性绑定为元素设置class 类样式

注意，是**类样式**。

### 引入

我们先来看下面这段代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .my-red {
            color: red;
        }

        .my-thin {
            /* 设置字体的粗细 */
            font-weight: 200;
        }

        .my-italic {
            font-style: italic;
        }

        .my-active {
            /* 设置字符之间的间距 */
            letter-spacing: 0.5em;
        }
    </style>
</head>

<body>
    <h1 class="my-red my-thin">我是千古壹号，qianguyihao</h1>
</body>

</html>
```

上面的代码中，我们直接通过正常的方式，给`<h1>`标签设置了两个 class 类的样式。代码抽取如下：

```html
    <h1 class="my-red my-thin">我是千古壹号，qianguyihao</h1>
```

上面的效果，我们还可以用Vue来写。这就引入了本段要讲的方式。

### 方式一：数组

**方式一**：直接传递一个数组。注意：这里的 class 需要使用  v-bind 做数据绑定。

代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="vue2.5.16.js"></script>
    <style>
        .my-red {
            color: red;
        }

        .my-thin {
            /* 设置字体的粗细 */
            font-weight: 200;
        }

        .my-italic {
            font-style: italic;
        }

        .my-active {
            /* 设置字符之间的间距 */
            letter-spacing: 0.5em;
        }
    </style>
</head>

<body>
    <div id="app">

        <!-- 普通写法 -->
        <h1 class="my-red my-thin">我是千古壹号，qianguyihao</h1>

        <!-- vue的写法1：数组的形式 -->
        <h1 :class="['my-red', 'my-thin']">我是qianguyihao，千古壹号</h1>

    </div>

    <script>

        var vm = new Vue({
            el: '#app'
        });

    </script>
</body>

</html>
```

代码抽取如下：

```html
        <!-- vue的写法1：数组的形式 -->
        <h1 :class="['my-red', 'my-thin']">我是qianguyihao，千古壹号</h1>
```

上方代码中，注意，数组里写的是字符串；如果不加单引号，就不是字符串了，而是变量。

演示效果如下：

![](http://img.smyhvae.com/20180509_1058.png)

### 写法二：在数组中使用三元表达式

```html
<body>
    <div id="app">
        <!-- vue的写法2：在数组中使用三元表达式。注意格式不要写错-->
        <!-- 通过data中布尔值 flag 来判断：如果 flag 为 true，就给 h1 标签添加`my-active`样式；否则，就不设置样式。 -->
        <h1 :class="[flag?'my-active':'']">我是qianguyihao，千古壹号</h1>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                flag:true
            }
        });
    </script>
</body>
```

上方代码的意思是，通过data中布尔值 flag 来判断：如果 flag 为 true，就给 h1 标签添加`my-active`样式；否则，就不设置样式。

注意，三元表达式的格式不要写错了。


### 写法三：在数组中使用 对象 来代替 三元表达式（提高代码的可读性）

上面的写法二，可读性较差。于是有了写法三。

**写法三**：在数组中使用**对象**来代替**三元表达式**。

代码如下：

```html
<body>
    <div id="app">
        <!-- vue的写法3：在数组中使用对象来代替三元表达式。-->
        <h1 :class="[ {'my-active':flag} ]">我是qianguyihao，千古壹号</h1>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                flag: true
            }
        });
    </script>
</body>
```

### 写法四：直接使用对象

写法四：直接使用对象。代码如下：

```html
        <!-- vue的写法4：直接使用对象-->
        <!-- 在为 class 使用 v-bind 绑定 对象的时候，对象的属性是类名。由于 对象的属性名可带引号，也可不带引号，所以 这里我没写引号；  属性的值 是一个标识符 -->
        <h1 :class="{style1:true, style2:false}">我是qianguyihao，千古壹号</h1>
```

上方代码的意思是，给`<h1>`标签使用样式`style1`，不使用样式`style2`。注意：

1、既然class样式名是放在对象中的，这个样式名不能有中划线，比如说，写成`:class="{my-red:true, my-active:false}`，是会报错的。

2、我们也可以对象通过存放在 data 的变量中。也就是说，上方代码可以写成：

```html
<body>
    <div id="app">
        <!-- vue的写法4：直接使用对象-->
        <!-- 在为 class 使用 v-bind 绑定 对象的时候，对象的属性是类名。由于 对象的属性名可带引号，也可不带引号，所以 这里我没写引号；  属性的值 是一个标识符 -->
        <h1 :class="classObj">我是qianguyihao，千古壹号</h1>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                classObj:{style1:true, style2:false}
            }
        });
    </script>
</body>
```

## Vue中通过属性绑定为元素设置 style 行内样式

注意，是行内样式（即内联样式）。

### 写法一

**写法一**：直接在元素上通过 `:style` 的形式，书写样式对象。

例如：

```html
        <h1 :style="{color: 'red', 'font-size': '20px'}">我是千古壹号，qianguyihao</h1>
```

### 写法二

**写法二**：将样式对象，定义到 `data` 中，并直接引用到 `:style` 中。

也就是说，把写法一的代码改进一下。代码如下：

```html
<body>
    <div id="app">
        <h1 :style="styleObj">我是千古壹号，qianguyihao</h1>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                styleObj: { color: 'red', 'font-size': '20px' }
            }
        });
    </script>
</body>
```

### 写法三

写法二只用到了一组样式。如果想定义**多组**样式，可以用写法三。

**写法三**：在 `:style` 中通过数组，引用多个 `data` 上的样式对象。

代码如下：

```html
<body>
    <div id="app">
        <h1 :style="[ styleObj1, styleObj2 ]">我是千古壹号，qianguyihao</h1>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                styleObj1: { color: 'red', 'font-size': '20px' },
                styleObj2: { 'font-style': 'italic' }
            }
        });
    </script>
</body>
```


## v-for：for循环的四种使用方式

**作用**：根据数组中的元素遍历指定模板内容生成内容。

### 引入

比如说，如果我想给一个`ul`中的多个`li`分别赋值1、2、3...。如果不用循环，就要挨个赋值：

```html
<body>
  <div id="app">
    <ul>
      <li>{{list[0]}}</li>
      <li>{{list[1]}}</li>
      <li>{{list[2]}}</li>
    </ul>
  </div>
</body>

<script>
  var vm = new Vue({
    el: '#app',
    data: {
      list: [1, 2, 3]
    }

  });
</script>
```

效果：

![](http://img.smyhvae.com/20180329_1713.png)

为了实现上面的效果，如果我用`v-for`进行赋值，代码就简洁很多了：

```html
<body>
  <div id="app">
    <ul>
      <!-- 使用v-for对多个li进行遍历赋值 -->
      <li v-for="item in list">{{item}}</li>
    </ul>
  </div>
</body>

<script>
  var vm = new Vue({
    el: '#app',
    data: {
      list: [1, 2, 3]
    }

  });
</script>
```

接下来，我们详细讲一下`v-for`的用法。需要声明的是，Vue 1.0的写法和Vue 2.0的写法是不一样的。本文全部采用Vue 2.0的写法。

### 方式一：普通数组的遍历

针对下面这样的数组：

```html
<script>
  new Vue({
    el: '#app',
    data: {
      arr1: [2, 5, 3, 1, 1],
    }
  });
</script>
```

将数组中的**值**赋给li：

```html
      <li v-for="item in arr1">{{item}}</li>
```

将数组中的**值和index**赋给li：


```html
      <!-- 括号里如果写两个参数：第一个参数代表值，第二个参数代表index 索引 -->
      <li v-for="(item,index) in arr1">值：{{item}} --- 索引：{{index}}</li>
```

效果如下：

![](http://img.smyhvae.com/20180329_1856.png)

### 方式二：对象数组的遍历

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="vue2.5.16.js"></script>
    <style>
    </style>
</head>

<body>
    <div id="app">
        <ul>
            <!-- 对象数组的遍历。括号里如果写两个参数：第一个参数代表数组的单个item，第二个参数代表 index 索引-->
            <li v-for="(item, index) in dataList">姓名：{{item.name}} --- 年龄：{{item.age}} --- 索引：{{index}}</li>

        </ul>
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                //对象数组
                dataList: [
                    { name: 'smyh', age: '26' },
                    { name: 'vae', age: '32' },
                    { name: 'xiaoming', age: '20' }
                ]
            }
        });
    </script>
</body>

</html>

```

效果如下：

![](http://img.smyhvae.com/20180509_1500.png)

### 方式三：对象的遍历

针对下面这样的对象：

```html
<script>
  new Vue({
    el: '#app',
    data: {
      obj1: {
        name: 'qianguyihao',
        age: '26',
        gender: '男'
      }
    }
  });
</script>
```

将上面的`obj1`对象的数据赋值给li，写法如下：

```html
<body>
  <div id="app">
    <ul>
      <!-- 括号里如果写两个参数：则第一个参数代表value，第二个参数代表key -->
      <li v-for="(value,key) in obj1">值：{{value}} --- 键：{{key}} </li>

      <h3>---分隔线---</h3>

      <!-- 括号里如果写三个参数：则第一个参数代表value，第二个参数代表key，第三个参数代表index -->
      <li v-for="(value,key,index) in obj1">值：{{value}} --- 键：{{key}} --- index：{{index}} </li>
    </ul>
  </div>
</body>

```

效果如下：

![](http://img.smyhvae.com/20180329_1850.png)

### 方式四：遍历数字

`in`后面还可以直接放数字。举例如下：

```html
        <ul>
            <!-- 对象数组的遍历 -->
            <!-- 注意：如果使用 v-for 遍历数字的话，前面的 myCount 值从 1 开始算起 -->
            <li v-for="myCount in 10">这是第 {{myCount}}次循环</li>
        </ul>
```

效果如下：

![](http://img.smyhvae.com/20180509_1505.png)

### v-for中key的使用注意事项

**注意**：在 Vue 2.2.0+ 版本里，当在**组件中**使用 v-for 时，key 属性是必须要加上的。

这样做是因为：每次 for 循环的时候，通过指定 key 来标示当前循环这一项的**唯一身份**。

> 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

> 为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

key的类型只能是：string/number，而且要通过 v-bind 来指定。

代码举例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="vue2.5.16.js"></script>
</head>

<body>
    <div id="app">

        <div>
            <label>Id:
                <input type="text" v-model="id">
            </label>

            <label>Name:
                <input type="text" v-model="name">
            </label>

            <input type="button" value="添加" @click="add">
        </div>

        <!-- 注意： v-for 循环的时候，key 属性只能使用 number 或者 string -->
        <!-- 注意： key 在使用的时候，必须使用 v-bind 属性绑定的形式，指定 key 的值 -->
        <!-- 在组件中，使用v-for循环的时候，或者在一些特殊情况中，如果 v-for 有问题，必须 在使用 v-for 的同时，指定 唯一的 字符串/数字 类型 :key 值 -->
        <p v-for="item in list" :key="item.id">
            <input type="checkbox">{{item.id}} --- {{item.name}}
        </p>
    </div>

    <script>
        // 创建 Vue 实例，得到 ViewModel
        var vm = new Vue({
            el: '#app',
            data: {
                id: '',
                name: '',
                list: [
                    { id: 1, name: 'smyh' },
                    { id: 2, name: 'vae' },
                    { id: 3, name: 'qianguyihao' },
                    { id: 4, name: 'xiaoming' },
                    { id: 5, name: 'xiaohong' }
                ]
            },
            methods: {
                add() { // 添加方法
                    this.list.unshift({ id: this.id, name: this.name })
                }
            }
        });
    </script>
</body>

</html>
```

## v-if：设置元素的显示和隐藏（添加/删除DOM元素）

**作用**：根据表达式的值的真假条件，来决定是否渲染元素，如果为false则不渲染（达到隐藏元素的目的），如果为true则渲染。

在切换时，元素和它的数据绑定会被销毁并重建。

举例如下：（点击按钮时，切换和隐藏盒子）

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="vue.js"></script>
</head>

<body>
  <div id="app">
    <button v-on:click="toggle">显示/隐藏</button>
    <div v-if="isShow">我是盒子</div>
  </div>
</body>

<script>
  new Vue({
    el: '#app',
    data: {
      isShow: true
    },
    methods: {
      toggle: function() {
        this.isShow = !this.isShow;
      }
    }
  });
</script>

</html>
```

效果如下：

![](http://img.smyhvae.com/20180329_1920.gif)

## v-show：设置元素的显示和隐藏（在元素上添加/移除`style="display:none"`属性）

**作用**：根据表达式的真假条件，来切换元素的 display 属性。如果为false，则在元素上添加 `display:none`属性；否则移除`display:none`属性。

举例如下：（点击按钮时，切换和隐藏盒子）

我们直接把上一段代码中的`v-if`改成`v-show`就可以了：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="vue.js"></script>
</head>

<body>
  <div id="app">
    <button v-on:click="toggle">显示/隐藏</button>
    <div v-show="isShow">我是盒子</div>
  </div>
</body>

<script>
  new Vue({
    el: '#app',
    data: {
      isShow: true
    },
    methods: {
      toggle: function() {
        this.isShow = !this.isShow;
      }
    }
  });
</script>

</html>

```

效果如下：

![](http://img.smyhvae.com/20180329_2040.gif)

### v-if和v-show的区别

`v-if`和`v-show`都能够实现对一个元素的隐藏和显示操作。

区别：

- v-if：每次都会重新添加/删除DOM元素

- v-show：每次不会重新进行DOM的添加/删除操作，只是在这个元素上添加/移除`style="display:none"`属性，表示节点的显示和隐藏。

优缺点：

- v-if：有较高的切换性能消耗。这个很好理解，毕竟每次都要进行dom的添加／删除操作。

- v-show：**有较高的初始渲染消耗**。也就是说，即使一开始`v-show="false"`，该节点也会被创建，只是隐藏起来了。而`v-if="false"`的节点，根本就不会被创建。

**总结**：

- 如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show

- 如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if










