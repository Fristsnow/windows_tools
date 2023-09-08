---
title: 03-v-on的事件修饰符
publish: true
---

<ArticleTopAd></ArticleTopAd>





## v-on的事件修饰符

### v-on的常见事件修饰符

`v-on` 提供了很多事件修饰符来辅助实现一些功能。事件修饰符有如下：

- `.stop`  阻止冒泡。本质是调用 event.stopPropagation()。

- `.prevent`  阻止默认事件（默认行为）。本质是调用 event.preventDefault()。

- `.capture`  添加事件监听器时，使用捕获的方式（也就是说，事件采用捕获的方式，而不是采用冒泡的方式）。

- `.self`  只有当事件在该元素本身（比如不是子元素）触发时，才会触发回调。

- `.once`  事件只触发一次。

- `.{keyCode | keyAlias}`   只当事件是从侦听器绑定的元素本身触发时，才触发回调。

- `.native` 监听组件根元素的原生事件。

PS：一个事件，允许同时使用多个事件修饰符。

写法示范：

```html
          <!-- click事件 -->
        <button v-on:click="doThis"></button>

        <!-- 缩写 -->
        <button @click="doThis"></button>

        <!-- 内联语句 -->
        <button v-on:click="doThat('hello', $event)"></button>

        <!-- 阻止冒泡 -->
        <button @click.stop="doThis"></button>

        <!-- 阻止默认行为 -->
        <button @click.prevent="doThis"></button>

        <!-- 阻止默认行为，没有表达式 -->
        <form @submit.prevent></form>

        <!--  串联修饰符 -->
        <button @click.stop.prevent="doThis"></button>
```


### `.stop`的举例

我们来看下面这个例子：

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
        .father {
            height: 300px;
            width: 300px;
            background: pink;
        }

        .child {
            width: 200px;
            height: 200px;
            background: green;
        }
    </style>
</head>

<body>
    <div id="app">
        <div class="father" @click="fatherClick">
            <div class="child" @click="childClick">
            </div>
        </div>
    </div>
    <script>

        var vm = new Vue({
            el: '#app',
            data: {},
            methods: {
                fatherClick: function () {
                    console.log('father 被点击了');
                },
                childClick: function () {
                    console.log('child 被点击了');
                }
            }
        })

    </script>
</body>

</html>

```

上方代码中，存在冒泡的现象，父标签中包含了一个子标签。当点击子标签时，父标签也会被触发。打印顺序是：

```
  child 被点击了
  father 被点击了
```

那么问题来了，如果我不想让子标签的点击事件冒泡到父亲，该怎么做呢？办法是：给子标签加一个**事件修饰符**`.stop`，阻止冒泡。代码如下：

```html
    <div class="child" @click.stop="childClick">
```

阻止冒泡后，当点击子标签时，打印结果是：

```
  child 被点击了
```

PS：我发现一个有意思的现象。上方的这行代码中，如果把`.stop`改为`:stop`，造成的现象是，父标签被触发了，而子标签没有被触发。

### `.capture`举例

`.capture`：触发事件时，采用捕获的形式，而不是冒泡的形式。

还是采用上面的例子：当按钮点击时，如果想要采取捕获的方式，而不是冒泡的方式，办法是：可以直接在父标签上加事件修饰符`.capture`。代码如下：

```html
    <div class="father" @click.capture="fatherClick">
```

当点击子标签时，打印结果是：

```
  father 被点击了
  child 被点击了
```


### `.prevent`的举例1

比如说，超链接`<a>`默认有跳转行为，那我可以通过事件修饰符`.prevent`阻止这种跳转行为。


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
        <!-- 通过 .prevent 阻止超链接的默认跳转行为 -->
        <a href="http://www.baidu.com" @click.prevent="linkClick">百度一下</a>
    </div>
    <script>

        var vm = new Vue({
            el: '#app',
            data: {},
            methods: {
                linkClick: function () {
                    console.log('超链接被点击了');
                }
            }
        })
    </script>
</body>

</html>
```

上方代码中：

- 如果去掉`.prevent`，点击按钮后，既会打印log，又会跳转到百度页面。

- 现在加上了`.prevent`，就只会打印log，不会跳转到百度页面。



### `.prevent`的举例2

现在有一个form表单：

```html
    <form action="http://www.baidu.com">
      <input type="submit" value="表单提交">
    </form>
```


我们知道，上面这个表单因为`type="submit"`，因此它是一个提交按钮，点击按钮后，这个表单就会被提交到form标签的action属性中指定的那个页面中去。这是表单的默认行为。

现在，我们可以用`.prevent`来阻止这种默认行为。修改为：点击按钮后，不提交到服务器，而是执行我们自己想要的事件（在submit方法中另行定义）。如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="vue2.5.16.js"></script>
</head>

<body>
  <div id="app">
    <!-- 阻止表单中submit的默认事件 -->
    <form @submit.prevent action="http://www.baidu.com">
      <!-- 执行自定义的click事件 -->
      <input type="submit" @click="mySubmit" value="表单提交">
    </form>

  </div>
</body>

<script>
  new Vue({
    el: '#app',
    data: {
    },
    methods: {
      mySubmit: function() {
        alert('ok');
      }
    }
  });
</script>

</html>
```

上方代码中，我们通过`.prevent`阻止了提交按钮的默认事件，点击按钮后，执行的是`mySubmit()`方法里的内容。这个方法名是可以随便起的，我们甚至可以起名为`submit`，反正默认的submit已经失效了。



### `.self`举例

- `.self`  只有当事件在该元素本身（比如不是子元素）触发时，才会触发回调。

我们知道，在事件触发机制中，当点击子标签时，父标签会通过冒泡的形式被触发（父标签本身并没有被点击）。可如果我给父标签的点击事件设置`.self`修饰符，达到的效果是：子标签的点击事件不会再冒泡到父标签了，只有点击符标签本身，父标签的事件才会被触发。代码如下：

```html
    <div class="father" @click.self="fatherClick">
```


**疑问**：既然`.stop`和`.self`都可以阻止冒泡，那二者有什么区别呢？区别在于：前者能够阻止整个冒泡行为，而后者只能阻止自己身上的冒泡行为。


