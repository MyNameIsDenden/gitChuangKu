```
//1.因为文档页面从上往下加载，所以先有标签 所以我们script写到标签的下面*

//参数 id是大小写敏感的字符串

var timer=document.getElementById('time');

console.log(timer);//返回的是一个元素对象 

console.log(typeof timer);*//Object;*

*//console.dir 打印我们返回的元素对象  更好的查看里面的属性和方法*

console.dir(timer);
```



```
//返回的是 获取过来元素对象的集合 以伪数组的形式储存的
//只有一个li返回的还是伪数组的形式 没有li 返回空的伪数组
var lis=document.getElementsByTagName('li');
console.log(lis);
console.log(lis[0]);
//遍历
for(var i=0;i<lis.length;i++){
console.log(lis[i]);
}
//element.getElementsByTagName('ol');
// var ol =document.getElementsByTagName('ol');//[ol]
// console.log(ol[0].getElementsByTagName('li'));
var gg=document.getElementById('pp');
console.log(gg.getElementsByTagName('li'));
```

*//1.getElementsByClassName 根据类名获得某些**元素集合***

```


​    var boxs=document.getElementsByClassName('box');

​    console.log(boxs);

​    *//2.quertSelector 返回指定选择器的第一个元素对象 切记 里面的选择器需要加符号 .box #nav*

​    var firstBox =document.querySelector('.box');*//class*

​    console.log(firstBox);

​    var nav =document.querySelector('#nav');*//id*

​    console.log(nav);

​    var li=document.querySelector('li');*//标签*

​    console.log(li);

​    *//3.querySelectorAll()返回指定选择器的所有元素对象集合*

​    var allBox = document.querySelectorAll('.box');

​    console.log(allBox);

​    var lis =document.querySelectorAll('li');

​    console.log(lis);
```

```
//1.获取body 元素
      var bodyEle =document.body;
      console.log(bodyEle);//返回是对象
      console.dir(bodyEle);//打印元素对象
      //2.获取html 元素
      //var htmlEle = document.html; 错
      var htmlEle =document.documentElement;
      console.log(htmlEle);
```

