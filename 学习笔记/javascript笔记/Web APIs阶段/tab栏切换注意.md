**注意其中K的值避免出错**

```
for(var i=0;i<l.length;i++){
    l[i].setAttribute('index',i);
    var k=i;
    l[i].onclick=function(){
        for(var i=0;i<l.length;i++){
            l[i].removeAttribute('class');
            c[i].style.display='none';
        }
        var index=this.getAttribute('index');
        this.className='current';
        console.log(k);//k=l.length-1;
        c[index].style.display='block';
        //c[k].style.display='block';做不到效果;
    }
}
```

