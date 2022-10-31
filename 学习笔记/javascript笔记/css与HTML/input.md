input 点击时出现的边框可以用          outline:none;     消除

```
<input checked='checked'>//表示被选中
```

```
z.onclick=function(){
       console.log(this.checked);//选中为true;未选中为false;
       for(var i=0;i<a.length;i++){
       a[i].checked=this.checked;
    }
}
```

