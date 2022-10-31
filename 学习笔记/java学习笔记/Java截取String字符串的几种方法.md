# Java截取String字符串的几种方法

## 方法一，指定字符，截取字符串，返回字符串数组：

```java
String str = "abcd,123,123abc,fij23";
String[]  strs=str.split(",");
```

## 方法二，指定索引号，截取字符串：

将字符串从索引号为5开始截取，一直到字符串末尾。（索引值从0开始）:

```java
String str = "abcdefghijklmnopqrstuvwxyz";
str.substring(5);
```


从索引号2开始到索引好4结束（并且不包含索引4截取在内，也就是说实际截取的是2和3号字符）:

```java
String sb = "abcdefghijklmnopqrstuvwxyz";
sb.substring(2, 4);
```

## 方法三，通过StringUtils截取

```
StringUtils.substringBefore("abcdefgefge", "e"); 
```


结果是：abcd
以第一个”e”，为标准。

```java
StringUtils.substringBeforeLast("abcdefgefge", "e") 
```

结果为：abcdefgefg
以最后一个“e”为准。
