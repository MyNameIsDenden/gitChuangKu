DOM事件流

​	事件流描述的是从页面中接收的顺序

​	事件发生时会在元素节点之间按照特定的顺序传播，这个过程及DOM事件流

​	1.捕获阶段

​	2.目标阶段

​	3.冒泡阶段

​	**注意**

​	1.js代码中只能执行捕获或者冒泡其中的一个阶段。

​	2.onclick和attachEvent只能得到冒泡阶段。

​	3.addEventList(type,listener,[useCapture])第三个参数如果是true，表示在事件捕获阶段调用事件处理程序；如果是false(不写默认就是false),表示在事件冒泡阶段调用事件处理程序。

​	有些事件没有冒泡，比如onblur,onfocus,onmousenter,onmouseleave

```
 <div class="dad">
        <div class="son"></div>
    </div>
    <script>
        var son=document.querySelector('.son');
        var d=document.querySelector('.dad');
        son.addEventListener('click',function(){
            alert('a');
        },true);
        d.addEventListener('click',function(){
            alert('b');
        },true);
        //先 b 后 a
        //true 改为 false 先 a 后 b
    </script>
```

