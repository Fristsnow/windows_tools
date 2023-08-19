game

基类

```javascript
this.elements = new Map()

this.player = null

this.pause = true

this.regcollisionObj = {
	enemy:{
        orginal:*,
        now:*,
        class:*
    }
}
init(){
    this.setEvent()
}
setEvent(){
    $(window).keydown(()=>{
        
    })
}
addElement(){
    if(!this.elements.has(type)) this.element.set(type,[])
    this.element.set(type,[...this.elements.get(type,element)])
}
createElement(){
    for(const key in this.regcollisionObj){
        const item = this.regcollisionObj[key]
        if(--item.now <=0){
            new item.class()
            item.now = item.orginal
        }
    }
}
updata(){
   if(this.pause){
       if(this.player = null) this.player = new Player()
       this.createElement()
       this.elements.forEach((item,type)=>{
           for(let i = 0;i<= item.length;i++){
               const element = item[i]
               if(element.die){
                   element.dom.remove()
                   this.elements.get(type).splic(i,1)
               }
               element.collision()
               element.updata()
           }
       })
       $(window).requestA...(this.elements.bind(this))
   }
}
```

