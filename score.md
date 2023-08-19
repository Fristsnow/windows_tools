# score

### 1.定义类方法（config）：

1. 获取id值。
2. 获取id值的宽高，
3. 定义npc和玩家的速度
4. 利用window.config = new Config()，定义全局变量

### 2.定义实体类(Element)：

1. #### 定义基础方法

   1. 定义类型--->this.type = type
   2. 定义速度--->this.speed = config.npcSpeed
   3. 将类型添加game后面
   4. 定义死亡；this.die = false
   5. 定义生成数组; this.regCollistion = []

2. #### 定义getInfo方法[getInfo(dom = this.dom)]

   1. 声明元素，宽，高，左，上的值：
      1. 宽：let width = dom.width()
      2. 高：let height = dom.height()
      3. 左：let left = dom.position().left
      4. 上：let top = dom.position().top
      5. 返回这四个值：通过return返回

3. ### 定义collision()方法---->碰撞检测

```javascript
collision(){
    //循环生成类型的数组
    for(const item of this.regCollision){
        const targets = game.element.get(item.type)
        if(targets){
            for(const target of targets){
                //获取玩家飞机
                let{
                    left:thisLeft,
                    top:thisTop,
                    width:thisWidth,
                    height:thisHeight,
                } = this.getInfo()
                //获取其他生成物
                let{
                    left:targetLeft,
                    top:targetTop,
                    width:targetWidth,
                    height:targetHeight,
                } = this.getInfo(target.dom)
                //碰撞检测核心逻辑
                if(
                    thisLeft + thisWidth >= targetLeft
                    &&
                    targetLeft + targetWidth >= thisLeft
                    &&
                    thisTop + thisHeight >= targetTop
                    &&
                    targetTop + targetHeight >= thisTop
                ){
                    item.callback(this,target)
                }
            }
        }
    }
}
```

#### 1.玩家飞机（继承Element）

1. 定义基础类

   ```javascript
   constructor(){
       //接收父类方法，传入子类值----->	
       super('player')
      	//定义玩家飞机主体----> 
   	this.dom = $('<div class='player'></div>')
       //定义初始左位置
       this.left = 20
       //定义初始top值
       this.top = config.HEIGHT / 2 - this.dom.height() / 2
       //定义初始速度
       this.speed = 5
       //放到页面
       config.WRAPPER.append(this.dom)
       
       
       //定义键盘事件
       //1.将键盘事件封装在Map()里面
       this.dir = new Map()
       //存入键盘键位和触发事件
       this.dir.set(37,0)
       this.dir.set(38,0)
       this.dir.set(39,0)
       this.dir.set(40,0)
       
       //2.调用执行函数
       this.setEvent()
       
       //3.定义飞机碰撞数组
       this.regCollision = [
           //燃油,定义类型，定义函数callback，如果触发了，则将die改为true，即死亡
           {
               type:'fuel',
               callback(_this,target){
                   target.die = true
               }
           }，
           //敌军
           {
           	type:'enemy',
           	callback(_this,target){
                   target.die = true
               }
           },
           //友军
           {
               type:'friendly',
               callback(_this,target){
                   target.die = true
               }
           },
           //行星
           {
               type:'score',
               callback(_this,target){
                   target.die = true
               }
           }
       ]
       
   }
   
   ```

2.键盘函数

```javascript
setEvent(){
    $(window)
    	.keydown((e)=>{
        if(this.dir.has(e.keyCode) === true){
            this.dir.set(e.keyCode,1)
        }
    })
    	.keydown((e)=>{
        if(this.dir.has(e.keyCode) === true){
            this.dir.set(e.keyCode,0)
        }
    })
}
```

3,键盘事件主要逻辑

```javascript
update(){
	//可以使键盘同时点击两个键，实现不同方向的移动
	this.left += (this.dir.get(39) - this.dir.get(37)) * this.speed
    this.top += (this.dir.get(40) - this.dir.get(38)) * this.speed
    
    //定义移动的距离范围
    this.left = Math.max(
        0,
        Math.min(this.left,config.WIDTH - this.dom.width())
    )
    this.top = Math.max(
        0,
        Math.min(this.top,config.HEIGHT - this.dom.height())
    )
    //将移动值渲染到css中
    this.dom.css(
        left: this.left,
        top: this.top,
    )
}
```

#### 2.敌军飞船

1，定义基础类

```javascript
//接受并传入参数
super('emeny')
//定义实体
this.dom = $('<div class='enemy'></div>')
//定义初始位置
this.left = config.WIDTH
this.top = Math.max(0,Math.random()*config.HEIGHT - this.dom.height())
config.WRAPPER.append(this.dom)
```

2.定义生成方法

```javascript
update(){
    //定义移动速度
    this.left -= this.speed
    //超出屏幕消失
    if(this.left <= 0 -this.dom.width()){
        this.die = true
    }
    this.dom.css(
        left: this.left,
        top: this.top,
    )
}
```

#### 3.友军飞船

1.定义基础类

```javascript
//接收方法并定义类型
super('friendly')
//定义实体类
this.dom = $('<div class='friendly'></div>')
//定义位置
this.left = config.WIDTH
this.top = Math.max(0,Math.random()*config.HEIGHT - this.dom.height())
//放到页面
config.WRAPPER.append(this.dom)
```

2.定义生成方法

```javascript
update(){
    //定义移动速度
    this.left -= this.speed
    //定义最大值，超出屏幕消失
    if(this.left <= 0 - this.dom.width()){
        this.die = true
    }
    this.dom.css(
        left: this.left,
        top: this.top,
    )
}
```

#### 4.燃油

1，定义基础类

```javascript
//接受并传参数
super('fuel')
//定义实体类
this.dom = $('<div class='fuel'></div>')
//定义位置
this.left = Math.max(0,Math.random()*config.WIDTH - this.dom.width())
this.top = 0-this.dom.width()
config.WRAPPER.append(this.dom)
```

2.定义生成方法

```javascript
update(){
    //定义速度
    this.top += this.speed
    //超出隐藏
    if(this.top >= config.HEIGHT){
        this.die = true
    }
    //添加到css
    this.dom.css(
        left:this.left,
        top:this.top,
    )
}
```

#### 5.行星

1.定义基础类

```javascript
//接收并传参
super('planet')
//定义实体类
this.dom = $('<div class='planet'></div>')
//定义初始位置和初始血量
this.speed = 4
this.life = 2
this.left = config.WIDTH
this.top = Math.max(0,Math.random()*config.HEIGHT - this.dom.height())
config.WRAPPER.append(this.dom)
```

2.定义生成方法

```javascript
update(){
    this.left -= this.speed
    if(this.left >= 0 - config.WIDTH){
        this.die = true
    }
    this.dom.css(
        left:this.left,
        top:this.top,
    )
}
```

#### 6.子弹

1.定义基础类

```javascript
conetructor(ship){
    //接收并定义类
    super('bullet')
    //定义ship
    this.ship = ship
    //定义速度
    this.speed = 7
    //定义实体类
    this.dom = $('<div class='bullet'></div>')
    //定义位置
    this.left = left + width
    this.top = top + height/2
    config.WRAPPER.append(this.dom)
    
    //调用函数方法
    this.regCollisionFunc()
}
```

2,定义碰撞目标函数处理

```javascript
regCollisionFunc(){
    this.regCollision = [
        //碰到敌军
        {
            type:'enemy',
            callback(_this,target){
                target.die = true
                _this.die = true
            }
        },
        //友军
        {
            type:'friendly',
            collback(_this,target){
                target.die = true
                _this.die = true
            }
        },
        //行星
        {
            type:'planet',
            collback(_this,target){
                if(--target.life <=0){
                    target.die = true
                }
                _this.die = true
            }
        }
    ]
}
```

3，定义生成方法

```javascript
updata(){
    this.left += this.speed
    if(this.left >= config.WIDTH){
        this.die = true
    }
    this.dom.css(
        left:this.left,
        top:this.top,
    )
}
```

### 3.游戏主类

1，定义基础类

```javascript
constructor(){
    //定义实体类函数方法
    this.element = new Map()
    //定义玩家飞机
    this.player = null
    //定义页面正常运动
    this.pause = true
    
    //定义实体生成时间
    //记忆方法	create	Element	Obj
    this.createElementObj = {
        enemy:{
            orginal:200,
            now:200,
            class:Enemy,
        },
        friendly:{
            orginal:200,
            now:200,
            class:Friendly,
        },
        fuel:{
            orginal:200,
            now:200,
            class:Fuel,
        },
        planet:{
            orginal:200,
            now:200,
            class:Planet,
        },
    }
    
    //调用初始化函数
    this.init()
}
```

2.定义初始化函数

```javascript
init(){
    window.requestAnimationFrame(this.update.bind(this))
    //调用P键，空格键事件函数
    this.setEvent()
}
```

3.定义P键，空格键事件

```javascript
setEvent(){
    //初始化子弹发射频率
    let lastShot = 0
    $(window).keydown((e)=>{
        if(e.keyCode == 80){
            this.pause = !this.pause
        }
        if(e.keyCode == 32 && this.pause && new Date()-lastShot >= 400){
            new Bullet(this.player)
            lastShot = new Date()
        }
    })
}
```

4.定义页面类添加事件

```javascript
addElement(type,element){
    if(!this.element.has(type)){
        this.element.set(type,[])
    }
    this.element.set(type,[...this.element.get(type),element])
}
```

5,创建元素事件

```javascript
createElement(){
    for(const key in this.createElementObj){
        const item = this.createElementObj[key]
        if(--item.now <=0){
            //生成调用class继承类，如果有类诞生，则调用生成
            new item.class()
            item.now = item.orginal
        }
    }
}
```

6.定义生成事件

```javascript
update(){
    if(this.pause){
        //如果初始化页面上没有玩家的飞机，则创建一个
        if(this.player == null){
            this.player = new Player
        }
       	
        //调用创建交互类
        this.createElement()
        
        //页面主逻辑
        this.element.forEach((item,type) =>{
            for(let i = 0;i < item.length;i++){
                const element = item[i]
                //如果元素die（死掉），则移除该元素
                if(element,die){
                    element.dom.remove()
                    this.element.get(type).splice(i,1)
                }
                
                //重复执行碰撞检测
                element.collision()
                //重复执行这个update()
                element.updata()
            }
        })
    }
    
    //重复执行这个update()
    window.requestAnimationFrame(this.update.bind(this))
}

//开始游戏必须调用这个
window.game = new Game()
```

