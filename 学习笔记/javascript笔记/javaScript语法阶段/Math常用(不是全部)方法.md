Math**常用**(不是全部)方法

1. 绝对值方法 Math.abs();

2. 三个取整方法

   Math.floor() 往小了取值

   Math.ceil()往大了取值

   Math.round()四舍五入 其他数字都是四舍五入，但是.5特殊 它往大了取;

   ```javascript
   console.log(Math.floor(1.1));//1
   console.log(Math.floor(1.9));//1
   console.log(Math.ceil(1.1));//2
   console.log(Math.ceil(1.9));//2
   console.log(Math.round(1.1));//1
   console.log(Math.round(1.9));//2
   console.log(Math.round(-1.1));//-1
   console.log(Math.round(-1.5));//-1
   ```

3. `Math.random()` 函数返回一个浮点数,  伪随机数在范围从**0到**小于**1**，也就是说，从0（包括0）往上，但是不包括1（排除1）.

   ### [得到一个两数之间的随机数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random#得到一个两数之间的随机数)

   这个例子返回了一个在指定值之间的随机数。这个值不小于 `min`（有可能等于），并且小于（不等于）`max`。

   ```javascript
   function getRandomArbitrary(min, max) {
     return Math.random() * (max - min) + min;
   }
   ```

   ### [得到一个两数之间的随机整数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random#得到一个两数之间的随机整数)

   这个例子返回了一个在指定值之间的随机整数。这个值不小于 `min` （如果 `min` 不是整数，则不小于 `min` 的向上取整数），且小于（不等于）`max`。

   ```js
   function getRandomInt(min, max) {
     min = Math.ceil(min);
     max = Math.floor(max);
     return Math.floor(Math.random() * (max - min)) + min; //不含最大值，含最小值
   }
   ```

   也许很容易想到用 `Math.round()` 来实现，但是这会导致你的随机数处于一个不均匀的分布，这可能不符合你的需求。

### [得到一个两数之间的随机整数，包括两个数在内](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random#得到一个两数之间的随机整数，包括两个数在内)

上一个例子提到的函数 `getRandomInt()` 结果范围包含了最小值，但不含最大值。如果你的随机结果需要同时包含最小值和最大值，怎么办呢? `getRandomIntInclusive()` 函数可以实现。

```js
function getRandomIntInclusive(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值 
}
```

