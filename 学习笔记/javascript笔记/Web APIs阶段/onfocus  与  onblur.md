**onfocus  与  onblur**

onfocus 获得焦点

onblur 失去焦点 

```
<input type="text" value="手机">
    <script>
        var text =document.querySelector('input');
        text.onfocus=function(){
            if(this.value==='手机'){
                this.value='';
            }
            this.style.color='#333';
        }
        text.onblur=function(){
            if(this.value===''){
                this.value='手机';
            }
            this.style.color='#999';
        }
    </script>
```

