# 						JS操纵html+css

------

### 问题一：js创建获取div的宽高

```javascript
可以使用JavaScript中的`offsetWidth`和`offsetHeight`属性来获取一个元素的宽度和高度。在这种情况下，您可以通过以下代码获取宽度和高度：

javascript
const div = document.querySelector('div'); //获取div元素
const width = div.offsetWidth; //获取宽度
const height = div.offsetHeight; //获取高度

```

### 问题二：js移动div

您可以使用JavaScript中的CSS动画（CSS Animation）来实现让一个盒子从右边移动到左边的效果。

以下是如何使用CSS动画和JS实现这个效果：

1. 首先，在CSS中定义动画。例如，可以使用`@keyframes`规则创建一个名为`moveLeft`的动画，该动画将元素从右侧移动到左侧。关键帧可以设置为`from {right: 0;}`到`to {left: 600px;}`，表示从页面的右端开始移动到距离页面左侧600像素的位置。

```css
@keyframes moveLeft {
  from {
    right: 0;
  }
  to {
    left: 600px;
  }
}
```

2. 接下来，将动画应用于要移动的元素上。例如，可以使用类选择器`.moveBox`将动画应用于具有此类的所有元素。在此示例中，假设我们的div元素具有`id="myDiv"`。

```css
.moveBox {
  animation: moveLeft 2s forwards;
  /* 上面的2s指的是动画持续时间，forwards表示动画结束后保持最后状态 */
}
```

3. 最后，在JavaScript代码中，您需要获取要移动的元素，并向其添加类以启动动画。例如，可以使用`document.querySelector()`方法选择具有`id="myDiv"`的元素，并将`moveBox`类添加到该元素。这将启动由CSS定义的动画，使目标元素向左侧移动600像素。

```js
const myDiv = document.querySelector('#myDiv');
myDiv.classList.add('moveBox');
```

请注意，此示例代码假设您的div元素的初始位置处于页面的右侧，并且没有其他CSS规则覆盖了默认的`position: static;`定位方式。如果需要，您可以根据实际情况更改CSS和JavaScript代码以适应不同的场景。

### 问题三：js创建div元素

您可以使用JavaScript的`document.createElement()`方法创建新的HTML元素。

以下是一个简单的示例代码，用于创建一个新的div元素，并将其添加到页面中：

```js
// 创建一个新的div元素
const newDiv = document.createElement('div');

// 设置新div的属性和内容
newDiv.id = 'myNewDiv';
newDiv.className = 'box';
newDiv.textContent = '这是一个新的div元素。';

// 获取要添加的父级容器，并将新div元素添加到其中
const container = document.querySelector('#container');
container.appendChild(newDiv);
```

在上面的示例代码中，我们首先使用`document.createElement()`方法创建了一个新的div元素，并将其存储在变量`newDiv`中。然后，我们设置了该元素的ID、类名和文本内容。接下来，我们使用`document.querySelector()`方法选择具有ID“container”的元素作为父级容器，并使用`appendChild()`方法将新的div元素添加到该容器中。

请注意，在使用`createElement()`方法创建新元素时，您还必须使用其他方法或属性对其进行进一步设置或操作，例如设置元素的样式、添加事件监听器等。

### 问题四：js用for创建10个id为k的盒子

您可以使用JavaScript的`for`循环和上一题中所示的方法来创建10个具有相同ID的div元素。

以下是一个简单的示例代码，用于创建10个具有ID“k”的div元素，并将它们添加到页面中：

```js
// 获取要添加的父级容器
const container = document.querySelector('#container');

// 创建10个具有相同ID的div元素，并将它们添加到容器中
for (let i = 0; i < 10; i++) {
  const newDiv = document.createElement('div');
  newDiv.id = 'k';
  container.appendChild(newDiv);
}
```

在上面的示例代码中，我们首先使用`document.querySelector()`方法获取具有ID“container”的父级容器。然后，我们使用`for`循环创建10个新的div元素，并为每个元素设置相同的ID“k”。最后，我们将这些新元素逐个添加到父级容器中。

请注意，由于所有新元素都具有相同的ID，“k”，这可能会导致一些问题，例如无法准确选择特定元素或无法应用CSS样式。因此，建议您使用唯一的ID和/或类名来标识每个元素。

### 问题五：js随机top值，left值为10px创建一个宽高为30px的盒子，从上往下运动

以下是一个简单的示例代码，用于创建具有随机位置和运动效果的div元素：

```js
// 获取要添加的父级容器
const container = document.querySelector('#container');

// 创建新的div元素，并设置初始位置和样式
const newDiv = document.createElement('div');
newDiv.style.position = 'absolute';
newDiv.style.width = '30px';
newDiv.style.height = '30px';
newDiv.style.backgroundColor = 'red';
newDiv.style.top = `${Math.random() * 100}px`;
newDiv.style.left = '10px';

// 将新的div元素添加到容器中
container.appendChild(newDiv);

// 定义动画效果，使元素从上方向下移动
newDiv.animate([
  { top: newDiv.style.top },
  { top: '100px' }
], {
  duration: 1000 + Math.random() * 2000, // 随机持续时间
  easing: 'ease-in-out', // 缓动函数
  fill: 'forwards' // 动画结束后保持最后状态
});
```

在上面的示例代码中，我们首先使用`document.querySelector()`方法获取具有ID“container”的父级容器。然后，我们创建一个新的div元素，并通过设置其样式和位置，使其在页面上随机出现在不同的位置，并向下移动。最后，我们使用`animate()`方法定义了一个动画效果，使该元素从初始位置向下移动到页面底部，使用了`Math.random()`方法来随机持续时间。

### 问题六：js利用jq用键盘的上下左右控制一个class=ks的盒子，使其可以在一个宽1000px，高600px的盒子内运动，不能运动到盒子外面。

以下是一个使用jQuery实现控制盒子运动的示例代码：

```html
<!-- HTML代码，包含一个宽1000px，高600px的容器和一个初始位置在左上角、类名为ks的div元素 -->
<div id="container" style="width: 1000px; height: 600px; border: 1px solid black;">
  <div class="ks" style="position: absolute; left: 0px; top: 0px; width: 50px; height: 50px; background-color: red;"></div>
</div>

<!-- 引入jQuery库文件 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>
$(document).ready(function() {
  // 获取要移动的元素
  const ksBox = $('.ks');

  // 监听键盘事件
  $(document).keydown(function(event) {
    const currentPosition = ksBox.position(); // 获取当前位置
    const keyCode = event.keyCode;

    switch (keyCode) {
      case 37: // 左箭头
        ksBox.css('left', Math.max(0, currentPosition.left - 10)); // 左移并限制边界
        break;
      case 38: // 上箭头
        ksBox.css('top', Math.max(0, currentPosition.top - 10)); // 上移并限制边界
        break;
      case 39: // 右箭头
        ksBox.css('left', Math.min(950, currentPosition.left + 10)); // 右移并限制边界
        break;
      case 40: // 下箭头
        ksBox.css('top', Math.min(550, currentPosition.top + 10)); // 下移并限制边界
        break;
    }
  });
});
</script>
```

在上述代码中，我们首先使用jQuery的`$`函数选择具有类名“ks”的div元素，并将其存储在变量`ksBox`中。然后，我们使用`keydown()`方法监听键盘事件，并根据不同的按键响应来更新元素的位置。

在处理不同的按键响应时，我们使用`position()`方法获取元素的当前位置，并结合Math.max()和Math.min()函数对元素的移动范围进行限制。例如，在左箭头（keyCode为37）响应中，我们将元素的left值减去10，并使用Math.max()函数确保其不会移动到容器的左侧边缘之外。另外，为了避免元素移出容器的下面和右边，我们在所有方向上都设置了最大的移动距离。

### 问题七：通过js+jq让两个盒子（A,B）在水平距离上运动，A盒子向右移动B盒子向左移动，当两个盒子碰撞在一起的时候，使A,B两个盒子消失。

以下是一个使用JavaScript和jQuery实现让两个盒子（A,B）在水平距离上运动，碰撞后消失的示例代码：

```html
<!-- HTML代码，包含A、B两个初始位置和样式相同的div元素 -->
<div id="container" style="width: 1000px; height: 600px; border: 1px solid black;">
  <div class="box a" style="position: absolute; left: 50px; top: 50px; width: 50px; height: 50px; background-color: red;"></div>
  <div class="box b" style="position: absolute; left: 900px; top: 50px; width: 50px; height: 50px; background-color: blue;"></div>
</div>

<!-- 引入jQuery库文件 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>
$(document).ready(function() {
  // 获取要移动的元素
  const boxA = $('.a');
  const boxB = $('.b');

  // 定义初始速度和方向
  let speedA = 5;
  let speedB = -5;

  // 定时循环更新元素位置
  setInterval(function() {
    // 更新A盒子位置
    const positionA = boxA.position();
    const newLeftA = positionA.left + speedA;
    if (newLeftA < 0 || newLeftA > 950) {
      speedA = -speedA; // 到达边界则反弹
    }
    boxA.css('left', newLeftA);

    // 更新B盒子位置
    const positionB = boxB.position();
    const newLeftB = positionB.left + speedB;
    if (newLeftB < 0 || newLeftB > 950) {
      speedB = -speedB; // 到达边界则反弹
    }
    boxB.css('left', newLeftB);

    // 检查盒子是否碰撞
    if (isCollide(boxA, boxB)) {
      boxA.remove(); // 碰撞后删除A盒子
      boxB.remove(); // 碰撞后删除B盒子
    }
  }, 50); // 每50毫秒更新一次位置

  // 定义检查碰撞的函数
  function isCollide(obj1, obj2) {
    const offset1 = obj1.offset();
    const offset2 = obj2.offset();

    const left1 = offset1.left;
    const top1 = offset1.top;
    const right1 = left1 + obj1.width();
    const bottom1 = top1 + obj1.height();

    const left2 = offset2.left;
    const top2 = offset2.top;
    const right2 = left2 + obj2.width();
    const bottom2 = top2 + obj2.height();

    // 检查是否有重叠部分
    if (left1 >= right2 || left2 >= right1 || top1 >= bottom2 || top2 >= bottom1) {
      return false;
    } else {
      return true;
    }
  }
});
</script>
```

以上代码实现了让两个盒子在页面上运动，并在碰撞后消失

### 问题八：利用js+jq可以通过键盘的P键使水平运动的盒子停止运动

可以通过以下代码实现：

```javascript
$(document).keydown(function(event) {
  if (event.key === "p" || event.key === "P") {
    $('.horizontal-box').stop();
  }
});
```

首先，使用`$(document).keydown()`绑定键盘按下事件。

然后，在事件处理程序中，检查按下的键是否是大写或小写字母“P”。

如果是，则使用`.stop()`方法停止具有类名`horizontal-box`的元素的运动。该方法将清除由`.animate()`或`.slideDown()`等方法创建的队列并立即停止元素的当前运动。

### 问题九：通过js + jq让两个盒子（ A, B） 在水平距离上运动， A盒子向右移动B盒子向左移动， 当两个盒子碰撞在一起的时候， 使A, B两个盒子消失。利用js+jq可以通过键盘的P键使水平运动的盒子停止运动，再点击P键恢复运动

首先，我们需要在HTML中创建两个盒子，并使用CSS设置它们的样式和初始位置。

```html
<div id="boxA"></div>
<div id="boxB"></div>
```

```css
#boxA {
  width: 50px;
  height: 50px;
  background-color: red;
  position: absolute;
  top: 100px;
  left: 100px;
}

#boxB {
  width: 50px;
  height: 50px;
  background-color: blue;
  position: absolute;
  top: 100px;
  left: 300px;
}
```

接下来，我们可以编写JavaScript代码以实现运动和停止运动的功能。我们将使用jQuery库来操作DOM元素并处理键盘事件。

```javascript
$(document).ready(function() {
  // 定义盒子A和盒子B的初始位置和速度
  var boxAPosition = 100;
  var boxASpeed = 5;
  var boxBPosition = 300;
  var boxBSpeed = -5;

  // 运动函数，每隔10毫秒更新一次位置
  var moveBoxes = setInterval(function() {
    // 获取盒子A和盒子B的位置
    var boxALeft = $('#boxA').offset().left;
    var boxBLeft = $('#boxB').offset().left;

    // 更新盒子A和盒子B的位置
    $('#boxA').css('left', boxAPosition + 'px');
    $('#boxB').css('left', boxBPosition + 'px');

    // 判断盒子A和盒子B是否碰撞
    if (boxALeft + 50 >= boxBLeft) {
      // 停止运动函数
      clearInterval(moveBoxes);
      
      // 隐藏盒子A和盒子B
      $('#boxA').hide();
      $('#boxB').hide();
    }

    // 更新盒子A和盒子B的位置
    boxAPosition += boxASpeed;
    boxBPosition += boxBSpeed;
  }, 10);

  // 处理键盘事件，P键的ASCII码为80
  $(document).keydown(function(event) {
    if (event.which == 80) {
      if (moveBoxes != null) {
        // 停止运动函数
        clearInterval(moveBoxes);
        moveBoxes = null;
      } else {
        // 重新开始运动函数
        moveBoxes = setInterval(function() {
          // 省略与上面相同的代码
        }, 10);
      }
    }
  });
});
```

在这个代码中，我们定义了盒子A和盒子B的初始位置和速度，并使用setInterval函数每10毫秒更新一次它们的位置。我们还使用jQuery库来获取和更新DOM元素的位置和样式，并处理P键的键盘事件以暂停/恢复运动函数。

在运动函数中，我们首先通过offset()方法获取盒子A和盒子B的左侧偏移量（left offset），然后更新它们的位置。接下来，我们检查盒子A和盒子B是否碰撞，如果是，则停止运动函数并隐藏盒子A和盒子B。最后，我们更新盒子A和盒子B的位置，让它们继续移动。

在处理键盘事件时，我们使用keydown()方法监听键盘按下事件，并通过event.which属性获取按下的键的ASCII码。如果按下的是P键（ASCII码为80），则切换运动函数的状态：如果正在运行，则停止；如果已经停止，则恢复。

这样，我们就实现了一个简单的盒子碰撞游戏。您可以尝试修改代码，调整盒子的速度、大小、颜色等参数，以及添加更多的功能，如积分、难度级别等。

### 问题十：利用js+jq，在左边生成一个宽高为50px的方形盒子，通过点击空格键，使其在该盒子前方生成一个宽高为10px的圆形盒子，并且该圆形盒子水平向右运动，如果该圆形盒子超出960px范围则清除该圆形盒子

好的，以下是实现该功能的代码：

先在HTML文件中添加一个空的div，用于生成方形盒子：
```html
<div id="box"></div>
```

然后，在JavaScript文件中使用jQuery监听空格键按下事件，创建圆形盒子并使其水平向右运动，直到超出960px范围被清除：
```javascript
$(document).keydown(function(event){
  if(event.which == 32){ // 监听空格键按下事件
    $("#box").before("<div class='circle'></div>"); // 在方形盒子前插入圆形盒子
    var circle = $(".circle:last"); // 获取最后一个圆形盒子
    var position = 50; // 初始化圆形盒子的位置
    var interval = setInterval(function(){
      position += 10; // 每次移动10px
      circle.css("left", position + "px"); // 修改圆形盒子的左边距
      if(position >= 960){ // 如果超出范围
        clearInterval(interval); // 清除定时器
        circle.remove(); // 移除圆形盒子
      }
    }, 50);
  }
});
```

最后，在CSS文件中定义方形盒子和圆形盒子的样式：
```css
#box {
  width: 50px;
  height: 50px;
  background-color: blue;
}

.circle {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: red;
  position: absolute;
  top: 20px; /* 根据实际情况调整 */
}
```

运行代码后，点击空格键即可在方形盒子前生成圆形盒子并使其水平向右运动。

### 问题十：利用js+jq，在一个宽为960px，高为600px的大盒子里面，生成一个宽高为50px的方形盒子，该方形盒子可以通过方向键控制方向，并且通过点击空格键，使其在该方形盒子前方生成一个宽高为10px的圆形盒子，并且该圆形盒子水平向右运动，如果该圆形盒子超出960px范围则清除该圆形盒子，其中圆形盒子会随着方形盒子运动的不同位置，然后会通过空格键生成

下面是使用js+jq实现该功能的代码：

HTML部分：
```html
<div id="bigBox"></div>
```

CSS部分：
```css
#bigBox {
  width: 960px;
  height: 600px;
  border: 1px solid black;
  position: relative;
}

.squareBox {
  width: 50px;
  height: 50px;
  background-color: red;
  position: absolute;
  left: 0;
  top: 0;
}

.circleBox {
  width: 10px;
  height: 10px;
  background-color: blue;
  position: absolute;
  left: 60px;
  top: 20px;
  border-radius: 5px;
}
```

JavaScript部分：
```javascript
$(document).ready(function() {
  // 创建方形盒子
  var squareBox = $("<div class='squareBox'></div>");
  $("#bigBox").append(squareBox);

  // 定义方形盒子的初始位置
  var squareBoxLeft = 0;
  var squareBoxTop = 0;

  // 监听键盘事件
  $(document).keydown(function(event) {
    switch (event.which) {
      case 37: // 左箭头
        squareBoxLeft -= 10;
        squareBox.css("left", squareBoxLeft);
        break;
      case 38: // 上箭头
        squareBoxTop -= 10;
        squareBox.css("top", squareBoxTop);
        break;
      case 39: // 右箭头
        squareBoxLeft += 10;
        squareBox.css("left", squareBoxLeft);
        break;
      case 40: // 下箭头
        squareBoxTop += 10;
        squareBox.css("top", squareBoxTop);
        break;
      case 32: // 空格键
        // 获取方形盒子的位置
        var circleBoxLeft = squareBoxLeft + 60;
        var circleBoxTop = squareBoxTop + 20;
        // 创建圆形盒子
        var circleBox = $("<div class='circleBox'></div>");
        circleBox.css({
          "left": circleBoxLeft,
          "top": circleBoxTop
        });
        $("#bigBox").append(circleBox);
        
        // 定义圆形盒子的移动函数
        function moveCircleBox() {
          circleBoxLeft += 5;
          circleBox.css("left", circleBoxLeft);

          if (circleBoxLeft > 960) {
            clearInterval(intervalId);
            circleBox.remove();
          }
        }

        // 执行圆形盒子的移动函数，并返回计时器ID
        var intervalId = setInterval(moveCircleBox, 50);
        break;
    }
  });
});
```

利用js+jq，在一个宽为960px，高为600px的大盒子里面，生成一个宽高为50px的方形盒子，该方形盒子可以通过方向键控制方向，并且通过点击空格键，使其在该方形盒子前方生成一个宽高为10px的圆形盒子，并且该圆形盒子水平向右运动，如果该圆形盒子超出960px范围则清除该圆形盒子，其中圆形盒子会随着方形盒子运动的不同位置，然后会通过空格键生成