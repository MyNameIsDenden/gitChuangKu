***innerText 和 innerHTML的区别***

​    *//1.innerText 不识别html标签 非标准 去除空格和换行的*

​    

```
var div=document.querySelector('div');
div.innerText = '<strong> 今天是: </strong>2021';*
//div中  <strong> 今天是: </strong>2021
```

​    *//2.innerHTML 识别html标签 W3C标准 保留空格和换行*

```
 div.innerHTML='<strong>今天是:</strong>2021';
 //div中   今天是: 2021
```

​    *//这两个属性是可读写的 可以获取元素里面的内容*

```
var p=document.querySelector('p');

​    console.log(p.innerText);不识别html标签 非标准 去除空格和换行的

​    console.log(p.innerHTML);识别html标签 W3C标准 保留空格和换行
```

