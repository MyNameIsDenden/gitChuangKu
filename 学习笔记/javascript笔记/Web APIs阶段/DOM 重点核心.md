**DOM 重点核心**

**文档对象模型**我们获取过来的DOM元素是一个对象(object)，所以称为文档对象模型

1.创建元素

​	document.write                    innerHTML          createElement

2.增

​	appendChild                           insertBefore

3.删

​    removeChild

4.改

​    1.修改元素属性：src、href、title等 直接修改

​	2.修改普通元素的内容 ：innerHTML    innerText

​	3.修改表单元素：value  、type    、disabled等

​	4.修改元素样式：style .className

5.查

​	1.DOM提供的API方法：getElementById、getElementsByTagName 古老用法 不太推荐

​	2.H5提供的新方法：querySelector、querySelectorAll提倡

​	3.利用节点操作获取元素:父(parentNode)、子(children)、兄(previousElementSibling、nextElementSibling)提倡

6.属性操作

​	1.setAttribute:设置dom的属性值

​	2.getAttribute:得到dom的属性值

​	3.removeAttribute移除属性值

7.事件操作

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过         |
| onmouseput  | 鼠标离开         |
| onfocus     | 获得鼠标焦点     |
| onblur      | 失去焦点         |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

