StringBuilder是一个可变的字符串类，我们可以把它看成是一个容器，这里的可变指的是StringBuilder对象中的内容是可变的

## 1. StringBuilder常用方法

```java
StringBuilder sb = new StringBuilder();
// 对象名.length() 序列长度
System.out.println(sb.length());  // 0
// 对象名.append() 追加到序列 可追字符
sb.append("hello");
System.out.println(sb);  // hello
// 97代表的是是'a'
sb.appendCodePoint(97);
System.out.println(sb);  // helloa
// 链式编程
sb.append(1).append("world").append(2);
System.out.println(sb);  // helloa1world2
// 索引是5的位置替换成空格
sb.setCharAt(5, ' ');
System.out.println(sb);  // hello 1world2
// 指定位置0前插入0
sb.insert(0, 0);
System.out.println(sb);  // 0hello 1world2
// 删除索引6(包含)至索引14(不包含)的字符串
sb.delete(6, 14);
System.out.println(sb);  // 0hello
// StringBuilder类型转换成String类型
String s = sb.toString();
System.out.println(s);  // 0hello

// 以及还有截取、反转、替换等方法
StringBuilder sb = new StringBuilder("猪头大一来过上海");
sb.reverse();
System.out.println(sb); // 输出结果为：海上过来一大头猪

//void replace(int start, int end, String str)
//根据索引把某部分替换成其它的。
StringBuilder sb = new StringBuilder("春眠不觉晓，处处闻啼鸟。");
sb.replace(8, 11, "蚊子咬");
System.out.println(sb); // 输出结果为：春眠不觉晓，处处蚊子咬。
```

## 2. String和StringBuilder的区别

1、String内容是不可变的，StringBuilder内容是可变的

2、StringBuilder处理字符串性能比String好

```java
public static void main(String[] args) {
    run1();
    run2();
}

public static void run1() {
    long start = System.currentTimeMillis();
    String result = "";
    for (int i = 0; i < 10000; i++) {
        result += i;
    }
    System.out.println(System.currentTimeMillis() - start);  // 145
}

public static void run2() {
    long start = System.currentTimeMillis();
    StringBuilder builder = new StringBuilder();
    for (int i = 0; i < 10000; i++) {
        builder.append(i);
    }
    System.out.println(System.currentTimeMillis() - start);  // 4
}

```

## 3. StringBuilder和String相互转换

```java
// string转换成StringBuilder
String str = "hello";
StringBuilder sb = new StringBuilder(str);
System.out.println(sb);  // hello
// StringBuilder转换成String
String s = sb.toString();
System.out.println(s);  // hello

```

