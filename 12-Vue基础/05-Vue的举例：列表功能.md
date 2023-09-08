---
title: 05-Vue的举例：列表功能
publish: true
---

<ArticleTopAd></ArticleTopAd>





## 列表功能举例

### 步骤 1：列表功能

完整的代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .table {
            width: 800px;
            margin: 20px auto;
            border-collapse: collapse; /*这一行，不能少：表格的两边框合并为一条*/
        }

        .table th {
            background: #0094ff;
            color: white;
            font-size: 16px;
            border: 1px solid black;
            padding: 5px;

        }

        .table tr td {
            text-align: center;
            font-size: 16px;
            padding: 5px;
            border: 1px solid black;
        }
    </style>

    <script src="vue2.5.16.js"></script>
</head>

<body>

<div id="app">

    <table class="table">
        <th>编号</th>
        <th>名称</th>
        <th>创建时间</th>
        <th>操作</th>
        <tr v-for="item in list">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.ctime}}</td>
            <td><a href="#">删除</a></td>
        </tr>
    </table>
</div>

</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            list: [{id: 1, name: '奔驰', ctime: new Date}, {id: 2, name: '大众', ctime: new Date}]
        }
    })

</script>

</html>
```

**代码分析**：数据是存放在data的list中的，将data中的数据通过`v-for`遍历给表格。


上方代码运行的效果：

![](http://img.smyhvae.com/20180401_1517.png)

### 步骤 2：无数据时，增加提示

如果list中没有数据，那么表格中就会只显示表头`<th>`，这样显然不太好看。

为此，我们需要增加一个`v-if`判断：当数据为空时，显示提示。如下：

```html
			<tr v-show="list.length == 0">
				<td colspan="4">列表无数据</td>
			</tr>
```

代码解释：`colspan="4"`指的是让当前这个`<td>`横跨4个单元格的位置。如下：

![](http://img.smyhvae.com/20180401_1535.png)

### 步骤 3：item的添加

具体实现步骤如下：

（1）用户填写的数据单独存放在data属性里，并采用`v-model`进行双向绑定。

（2）用户把数据填好后，点击add按钮。此时需要增加一个点击事件的方法，将data中的数据放到list中（同时，清空文本框中的内容）。

（3）将数据展示出来。`v-for`有个特点：当list数组发生改变后，vue.js就会自动调用`v-for`重新将数据生成，这样的话，就实现了数据的自动刷新。

完整的代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .table {
            width: 800px;
            margin: 20px auto;
            border-collapse: collapse; /*这一行，不能少：表格的两边框合并为一条*/
        }

        .table th {
            background: #0094ff;
            color: white;
            font-size: 16px;
            border: 1px solid black;
            padding: 5px;
        }

        .table tr td {
            text-align: center;
            font-size: 16px;
            padding: 5px;
            border: 1px solid black;
        }

        .form {
            width: 800px;
            margin: 20px auto;
        }

        .form button {
            margin-left: 10px;
        }
    </style>

    <script src="vue2.5.16.js"></script>
</head>

<body>

<div id="app">

    <div class="form">

        编号：<input type="text" v-model="formData.id">
        名称：<input type="text" v-model="formData.name">

        <button v-on:click="addData">添加</button>
    </div>

    <table class="table">
        <th>编号</th>
        <th>名称</th>
        <th>创建时间</th>
        <th>操作</th>
        <tr v-show="list.length == 0">
            <td colspan="4">列表无数据</td>
        </tr>
        <tr v-for="item in list">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.ctime}}</td>
            <td><a href="#">删除</a></td>
        </tr>
    </table>
</div>

</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            list: [{id: 1, name: '奔驰', ctime: new Date}, {id: 2, name: '大众', ctime: new Date}],
            //用户添加的数据
            formData: {
                id: 0,
                name: ""
            }
        },

        methods: {
            addData: function () {
                //将数据追加到list中
                var p = {id: this.formData.id, name: this.formData.name, ctime: new Date()};
                this.list.push(p);

                //清空页面上的文本框中的数据
                this.formData.id = 0;
                this.formData.name = '';
            }
        }
    });

</script>

</html>

```

### 步骤 4：item的删除

html部分：

```html
            <!--绑定delete事件，根据括号里的参数进行删除-->
            <td><a href="#" v-on:click="delData(item.id)">删除</a></td>
```

js部分：

```javascript
            delData: function (id) {
                // 0 提醒用户是否要删除数据
                if (!confirm('是否要删除数据?')) {
                    //当用户点击的取消按钮的时候，应该阻断这个方法中的后面代码的继续执行
                    return;
                }

                // 1 调用list.findIndex()方法根据传入的id获取到这个要删除数据的索引值（在数组中的索引值）
                var index = this.list.findIndex(function (item) {
                    return item.id == id
                });

                // 2.0 调用方法：list.splice(待删除的索引, 删除的元素个数)
                this.list.splice(index, 1);
            }
```


代码解释：`find()`和`findIndex()`是ES6中为数组新增的函数。详细解释如下：

```javascript
    // 根据id得到下标

    // 默认去遍历list集合，将集合中的每个元素传入到function的item里，
    var index = this.list.findIndex(function(item){
    //根据item中的id属性去匹配传进来的id
    //如果是则返回true ；否返回false,继续下面的一条数据的遍历，以此类推
    return item.id ==id; //如果返回true，那么findIndex方法会将这个item对应的index
    });
```

也就是说，我们是根据 item.id 找到这个 item 是属于list 数组中的哪个index索引。找到了index，就可以根据index来删除数组中的那个元素了。

当item被删除后，v-for会被自动调用，进而自动更新view。

完整版代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .table {
            width: 800px;
            margin: 20px auto;
            border-collapse: collapse;  /*这一行，不能少：表格的两边框合并为一条*/
        }

        .table th {
            background: #0094ff;
            color: white;
            font-size: 16px;
            border: 1px solid black;
            padding: 5px;
        }

        .table tr td {
            text-align: center;
            font-size: 16px;
            padding: 5px;
            border: 1px solid black;
        }

        .form {
            width: 800px;
            margin: 20px auto;
        }

        .form button {
            margin-left: 10px;
        }
    </style>

    <script src="vue2.5.16.js"></script>
</head>

<body>

    <div id="app">

        <div class="form">

            编号：
            <input type="text" v-model="formData.id"> 名称：
            <input type="text" v-model="formData.name">

            <button v-on:click="addData">添加</button>
        </div>

        <table class="table">
            <th>编号</th>
            <th>名称</th>
            <th>创建时间</th>
            <th>操作</th>
            <tr v-show="list.length == 0">
                <td colspan="4">列表无数据</td>
            </tr>
            <tr v-for="item in list">
                <td>{{item.id}}</td>
                <td>{{item.name}}</td>
                <td>{{item.ctime}}</td>
                <!--绑定delete事件，根据括号里的参数进行删除-->
                <td>
                    <a href="#" v-on:click="delData(item.id)">删除</a>
                </td>
            </tr>
        </table>
    </div>

</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            list: [{ id: 1, name: '奔驰', ctime: new Date }, { id: 2, name: '大众', ctime: new Date }],
            //用户添加的数据
            formData: {
                id: 0,
                name: ""
            }
        },

        methods: {
            addData: function () {
                //将数据追加到list中
                var p = { id: this.formData.id, name: this.formData.name, ctime: new Date() };
                this.list.push(p);

                //清空页面上的文本框中的数据
                this.formData.id = 0;
                this.formData.name = '';
            },  //注意：方法之间用逗号隔开，这个逗号不要忘记了

            delData: function (id) {
                // 0 提醒用户是否要删除数据
                if (!confirm('是否要删除数据?')) {
                    //当用户点击的取消按钮的时候，应该阻断这个方法中的后面代码的继续执行
                    return;
                }

                // 1 调用list.findIndex()方法根据传入的id获取到这个要删除数据的索引值
                var index = this.list.findIndex(function (item) {
                    return item.id == id
                });

                // 2 调用方法：list.splice(待删除的索引, 删除的元素个数)
                this.list.splice(index, 1);
            }


        }
    });

</script>

</html>
```

### 步骤 5：按条件筛选item

现在要求实现的效果是，在搜索框输入关键字 keywords，列表中仅显示匹配出来的内容。也就是说：

- 之前， v-for 中的数据，都是直接从 data 上的list中直接渲染过来的。

- 现在， 我们在使用`v-for`进行遍历显示的时候，不能再遍历全部的 list 了；我们要自定义一个 search 方法，同时，把keywords作为参数，传递给 search 方法。即`v-for="item in search(keywords)"`。

在 search(keywords) 方法中，为了获取 list 数组中匹配的item，我们可以有两种方式实现。如下。

**方式一**：采用`forEach + indexOf()`

```javascript
    search(keywords) { // 根据关键字，进行数据的搜索，返回匹配的item

        //实现方式一：通过 indexOf() 进行匹配。
        var newList = [];
        this.list.forEach(item => {
            if (item.name.indexOf(keywords) != -1) {  //只要不等于 -1，就代表匹配到了
                newList.push(item)
            }
        })
        return newList
    }
```

上方代码中， 我们要注意 indexOf(str) 的用法。举例如下：

```javascript
        var str = 'smyhvae';

        console.log(str.indexOf('s'));  //打印结果：0

        console.log(str.indexOf(''));   //打印结果：0。（说明，即使去匹配空字符串，也是返回0）

        console.log(str.indexOf('h'));  //打印结果：3

        console.log(str.indexOf('x'));  //打印结果：-1 （说明，匹配不到任何字符串）
```

上方代码中，也就是说，如果参数为空字符串，那么，每个item都能匹配到。

**方式二**： filter + includes()方法

```javascript
    search(keywords) { // 根据关键字，进行数据的搜索，返回匹配的item

        var newList = this.list.filter(item => {
            // 注意 ： ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串')
            //  如果包含，则返回 true ，否则返回 false
            if (item.name.includes(keywords)) {
                return item
            }
        })

        return newList
    }
```

注意：forEach   some   filter   findIndex，这些都属于数组的新方法，都会对数组中的每一项，进行遍历，执行相关的操作。这里我们采用数组中的 filter 方法，

总的来说，方式二的写法更优雅，因为字符串的 includes()方法确实很实用。

完整版代码如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .table {
            width: 800px;
            margin: 20px auto;
            border-collapse: collapse;/*这一行，不能少：表格的两边框合并为一条*/
        }

        .table th {
            background: #0094ff;
            color: white;
            font-size: 16px;
            border: 1px solid black;
            padding: 5px;
        }

        .table tr td {
            text-align: center;
            font-size: 16px;
            padding: 5px;
            border: 1px solid black;
        }

        .form {
            width: 800px;
            margin: 20px auto;
        }

        .form button {
            margin-left: 10px;
        }
    </style>

    <script src="vue2.5.16.js"></script>
</head>

<body>

    <div id="app">

        <div class="form">

            编号：
            <input type="text" v-model="formData.id"> 名称：
            <input type="text" v-model="formData.name">

            <button v-on:click="addData">添加</button>
            搜索：
            <input type="text" v-model="keywords">

        </div>

        <table class="table">
            <th>编号</th>
            <th>名称</th>
            <th>创建时间</th>
            <th>操作</th>
            <tr v-show="list.length == 0">
                <td colspan="4">列表无数据</td>
            </tr>
            <tr v-for="item in search(keywords)">
                <td>{{item.id}}</td>
                <td>{{item.name}}</td>
                <td>{{item.ctime}}</td>
                <!--绑定delete事件，根据括号里的参数进行删除-->
                <td>
                    <a href="#" v-on:click="delData(item.id)">删除</a>
                </td>
            </tr>
        </table>
    </div>

</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            list: [{ id: 1, name: '奔驰', ctime: new Date }, { id: 2, name: '大众', ctime: new Date }],
            //用户添加的数据
            formData: {
                id: '',
                name: ""
            },
            keywords: ""
        },

        methods: {
            addData: function () {
                //将数据追加到list中
                var p = { id: this.formData.id, name: this.formData.name, ctime: new Date() };
                this.list.push(p);

                //清空页面上的文本框中的数据
                this.formData.id = '';
                this.formData.name = '';
            },  //注意：方法之间用逗号隔开，这个逗号不要忘记了

            delData: function (id) {
                // 0 提醒用户是否要删除数据
                if (!confirm('是否要删除数据?')) {
                    //当用户点击的取消按钮的时候，应该阻断这个方法中的后面代码的继续执行
                    return;
                }

                // 1 调用list.findIndex()方法根据传入的id获取到这个要删除数据的索引值
                var index = this.list.findIndex(function (item) {
                    return item.id == id
                });

                // 2 调用方法：list.splice(待删除的索引, 删除的元素个数)
                this.list.splice(index, 1);
            },

            search(keywords) { // 根据关键字，进行数据的搜索，返回匹配的item

                var newList = this.list.filter(item => {
                    // 注意 ： ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串')
                    //  如果包含，则返回 true ，否则返回 false
                    if (item.name.includes(keywords)) {
                        return item
                    }
                })

                return newList
            }
        }
    });

</script>

</html>

```

备注：在1.x 版本中可以通过filterBy指令来实现过滤，但是在2.x中已经被废弃了。





