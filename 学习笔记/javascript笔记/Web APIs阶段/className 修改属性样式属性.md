className 修改属性样式属性

1. 使用element.style 获得修改元素样式 如果样式比较少 或者 功能简单的情况下使用

2. 修改元素的className 更改元素的样式 适合于样式较多或者功能复杂的情况

   ```
   var test=document.querySelector('div');
           test.onclick =function(){
           this.className='change';
           this.className='first change';//以前的样式也保留
   }
   ```

   