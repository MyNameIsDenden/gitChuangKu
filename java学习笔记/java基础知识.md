# 一.反射

## 1.getclass()

getClass() 返回此 Object 的运行时类。 **返回Class类型的对象。**

**getClass()方法的作用**

获得了Person这个(类)Class，进而通过返回的Class对象获取Person的相关信息，比如：获取Person的构造方法，方法，属性有哪些等等信息。

[getClass()相关理解](https://blog.csdn.net/expect521/article/details/79139829)

```java
public class Test {
    public static void main(String[] args) {
        Person p = new Person(1,"刘德华");
        System.out.println(p.getClass());  
        System.out.println(p.getClass().getName()); 
    }
}

class Person{
    int id;
    String name;
    public Person(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }
}
```

![运行结果](https://img-blog.csdn.net/20180123145612504?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXhwZWN0NTIx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 2.Class.getClassLoader（）

### 一、[ClassLoader](https://so.csdn.net/so/search?q=ClassLoader&spm=1001.2101.3001.7020) 的作用

我们都知道java程序写好以后是以`.java`（文本文件）的文件存在磁盘上，然后，我们通过(bin/javac.exe)编译命令把`.java`文件编译成`.class`文件（[字节码](https://so.csdn.net/so/search?q=字节码&spm=1001.2101.3001.7020)文件），并存在磁盘上。
但是程序要运行，首先一定要把`.class`文件加载到JVM内存中才能使用的，我们所讲的classLoader,就是负责把磁盘上的`.class`文件加载到JVM内存中，如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426172636107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25hbmh1YWliZWlhbg==,size_16,color_FFFFFF,t_70)
你可以认为每一个Class对象拥有磁盘上的那个`.class`字节码内容,每一个class对象都有一个`getClassLoader()`方法，得到是谁把我从`.class`文件加载到内存中变成Class对象的

### 二、ClassLoader 层次结构

![img](https://img-blog.csdnimg.cn/20200426175940483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25hbmh1YWliZWlhbg==,size_16,color_FFFFFF,t_70)

（1）根[类加载](https://so.csdn.net/so/search?q=类加载&spm=1001.2101.3001.7020)器（null）
它是由本地代码(c/c++)实现的，你根本拿不到他的引用，但是他实际存在，并且加载一些重要的类，它加载`(%JAVA_HOME%\jre\lib)`,如`rt.jar(runtime)、i18n.jar`等，这些是Java的核心类。
（2）平台类加载器（PlatformClassLoader）（jdk1.8之后的版本，之前的称为扩展类加载器 ExtClassLoader）
虽说能拿到，但是我们在实践中很少用到它，它主要加载扩展目录下的jar包， `%JAVA_HOME%\lib\ext`
（3）应用类加载器（appClassLoader）
它主要加载我们应用程序中的类，如Test,或者用到的第三方包,如jdbc驱动包等。这里的父类加载器与类中继承概念要区分，它们在class定义上是没有父子关系的。

### 三、Class 加载时调用类加载器的顺序

当一个类要被加载时，有一个启动类加载器和实际类加载器的概念，这个概念请看如下分析：

如上面的`Test.class`要进行加载时，它将会启动应用类加载器进行加载`Test`类，但是这个应用类加载器不会真正去加载它，而是会调用看是否有父加载器，结果有，是扩展类加载器，扩展类加载器也不会直接去加载，它看自己是否有父加载器没，结果它还是有的，是根类加载器。

所以这个时候根类加载器就去加载这个类，可在`%JAVA_HOME%\jre\lib`下，它找不到`dir_b.Test`这个类，所以他告诉他的子类加载器，我找不到，你去加载吧，子类扩展类加载器去`%JAVA_HOME%\lib\ext`去找，也找不着，它告诉它的子类加载器 AppClassLoader，我找不到这个类，你去加载吧，结果AppClassLoader找到了，就加到内存中，并生成Class对象。
这个时间时候启动类加载器（应用类加载器）和实际类加载器（应用类加载器）是同一个.

这也是 Java 中著名的委托加载机制：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426185027299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25hbmh1YWliZWlhbg==,size_16,color_FFFFFF,t_70)







## **3.getResourceAsStream**（）



```java
public InputStream getResourceAsStream(String name);
```



**首先，Java中的getResourceAsStream有以下几种：** 
\1. Class.getResourceAsStream(String path) ： path 不以’/'开头时默认是从此类所在的包下取资源，以’/'开头则是从ClassPath根下获取。其只是通过path构造一个绝对路径，最终还是由ClassLoader获取资源。 

\2. Class.getClassLoader.getResourceAsStream(String path) ：默认则是从ClassPath根下获取，path不能以’/'开头，最终是由ClassLoader获取资源。 

\3. ServletContext. getResourceAsStream(String path)：默认从WebAPP根目录下取资源，Tomcat下path是否以’/'开头无所谓，当然这和具体的容器实现有关。 

\4. Jsp下的application内置对象就是上面的ServletContext的一种实现。 

**其次，getResourceAsStream 用法大致有以下几种：** 

第一： 要加载的文件和.class文件在同一目录下，例如：com.x.y 下有类me.class ,同时有资源文件myfile.xml 

那么，应该有如下代码： 

```java
me.class.getResourceAsStream("myfile.xml"); 
```

第二：在me.class目录的子目录下，例如：com.x.y 下有类me.class ,同时在 com.x.y.file 目录下有资源文件myfile.xml 

那么，应该有如下代码： 

```java
me.class.getResourceAsStream("file/myfile.xml"); 
```

第三：不在me.class目录下，也不在子目录下，例如：com.x.y 下有类me.class ,同时在 com.x.file 目录下有资源文件myfile.xml 

那么，应该有如下代码： 

```java
me.class.getResourceAsStream("/com/x/file/myfile.xml"); 
```

总结一下，可能只是两种写法 

第一：前面有 “  / ” 

“ / ”代表了工程的根目录，例如工程名叫做myproject，“ / ”代表了myproject 

```java
me.class.getResourceAsStream("/com/x/file/myfile.xml"); 
```

第二：前面没有 “  / ” 

代表当前类的目录 

```java
me.class.getResourceAsStream("myfile.xml"); 

me.class.getResourceAsStream("file/myfile.xml"); 
```

**最后，自己的理解：** 
getResourceAsStream读取的文件路径只局限与工程的源文件夹中，包括在工程src根目录下，以及类包里面任何位置，但是如果配置文件路径是在除了源文件夹之外的其他文件夹中时，该方法是用不了的。

## 2.Class.forName()

Class.forName()主要功能

Class.forName(xxx.xx.xx)返回的是一个类，

Class.forName(xxx.xx.xx)的作用是要求JVM查找并加载指定的类，也就是说JVM会执行该类的静态代码段。

下面，通过解答以下三个问题的来详细讲解下Class.forName()的用法。

### 一.什么时候用Class.forName()？

给你一个字符串变量，它代表一个类的包名和类名，你怎么实例化它？你第一想到的肯定是new,但是注意一点：

A a = (A)Class.forName(“pacage.A”).newInstance();

这和你 A a = new A()； 是一样的效果。

现在言归正传。

动态加载和创建Class 对象，比如想根据用户输入的字符串来创建对象时需要用到：

String str = “用户输入的字符串” ;

Class t = Class.forName(str);

t.newInstance();

在初始化一个类，生成一个实例的时候，newInstance()方法和new关键字除了一个是方法，一个是关键字外，最主要有什么区别？它们的区别在于创建对象的方式不一样，前者是使用类加载机制，后者是创建一个新类。那么为什么会有两种创建对象方式？这主要考虑到软件的可伸缩、可扩展和可重用等软件设计思想。

Java中工厂模式经常使用newInstance()方法来创建对象，因此从为什么要使用工厂模式上可以找到具体答案。 例如：

class c = Class.forName(“Example”);

factory = (ExampleInterface)c.newInstance();

其中ExampleInterface是Example的接口，可以写成如下形式：

String className = “Example”;

class c = Class.forName(className);

factory = (ExampleInterface)c.newInstance();

进一步可以写成如下形式：

String className = readfromXMlConfig;//从xml 配置文件中获得字符串

class c = Class.forName(className);

factory = (ExampleInterface)c.newInstance();

上面代码已经不存在Example的类名称，它的优点是，无论Example类怎么变化，上述代码不变，甚至可以更换Example的兄弟类Example2 , Example3 , Example4……，只要他们继承ExampleInterface就可以。

从JVM的角度看
我们使用关键字new创建一个类的时候，这个类可以没有被加载。但是使用newInstance()方法的时候，就必须保证：

1、这个类已经加载；

2、这个类已经连接了。

而完成上面两个步骤的正是Class的静态方法forName()所完成的，这个静态方法调用了启动类加载器，即加载 java API的那个加载器。

现在可以看出，newInstance()实际上是把new这个方式分解为两步，即首先调用Class加载方法加载某个类，然后实例化。 这样分步的好处是显而易见的。我们可以在调用class的静态加载方法forName时获得更好的灵活性，提供给了一种降耦的手段。

### 二.new 和Class.forName（）有什么区别？

其实上面已经说到一些了，这里来做个总结：

首先，newInstance( )是一个方法，而new是一个关键字；

其次，Class下的newInstance()的使用有局限，因为它生成对象只能调用无参的构造函数，而使用 new关键字生成对象没有这个限制。

简言之：

newInstance(): 弱类型,低效率,只能调用无参构造。

new: 强类型,相对高效,能调用任何public构造。

Class.forName(“”)返回的是类。

Class.forName(“”).newInstance()返回的是object 。

### 三.为什么在加载数据库驱动包的时候有用的是Class.forName( )，却没有调用newInstance( )？

在Java开发特别是数据库开发中，经常会用到Class.forName( )这个方法。

通过查询Java Documentation我们会发现使用Class.forName( )静态方法的目的是为了动态加载类。

通常编码过程中，在加载完成后，一般还要调用Class下的newInstance( )静态方法来实例化对象以便操作。因此，单单使用Class.forName( )动态加载类是没有用的，其最终目的是为了实例化对象。

有数据库开发经验朋友会发现，为什么在我们加载数据库驱动包的时候有的却没有调用newInstance( )方法呢？

即有的jdbc连接数据库的写法里是Class.forName(xxx.xx.xx);而有一 些：Class.forName(xxx.xx.xx).newInstance()，为什么会有这两种写法呢？

刚才提到，Class.forName(“”);的作用是要求JVM查找并加载指定的类，首先要明白，java里面任何class都要装载在虚拟机上才能运行，而静态代码是和class绑定的，class装载成功就表示执行了你的静态代码了，而且以后不会再走这段静态代码了。

而我们前面也说了，Class.forName(xxx.xx.xx)的作用就是要求JVM查找并加载指定的类，如果在类中有静态初始化器的话，JVM必然会执行该类的静态代码段。

而在JDBC规范中明确要求这个Driver类必须向DriverManager注册自己，即任何一个JDBC Driver的 Driver类的代码都必须类似如下：

```java
public class MyJDBCDriver implements Driver {

static {

DriverManager.registerDriver(new MyJDBCDriver());

}

}
```

既然在静态初始化器的中已经进行了注册，所以我们在使用JDBC时只需要Class.forName(XXX.XXX);就可以了。


## 4.getDeclaredField()

获取 Field（类或接口的成员变量的类型） 

```java
public Field getDeclaredField(String name) throws NoSuchFieldException,SecurityException
```

## 5.Field.set();

```java
set(Object obj, Object value)
```

​	**field.set(a,b); 中 a表示所需要改变的实例化对象 b表示改变之后的值 field表示要改变的对象（a中成员变量的类型）**

​	**改变之后 a 中的 field 的值就为  b;**



## 6.getDeclaredMethod()

注：方法返回一个Method对象，它反映此Class对象所表示的类或接口的指定已声明方法。

**name 参数是一个字符串，指定所需的方法的简单名称，**

**parameterTypes 参数是一个数组的Class对象识别方法的形参类型，在声明的顺序**

```java
public Method getDeclaredMethod(String name, Class... parameterTypes) throws NoSuchMethodException,SecurityException
```





### getDeclaredMethods()

getDeclaredMethods(),该方法是获取本类中的所有方法，包括私有的(private、protected、默认以及public)的方法。



## 7.Method的getParameters()

获取该方法的参数

如果jdk是1.8一下或者没有设置带参数名编译，那么Parameter的getName函数只会返回arg0，arg1，arg2......

需设置：

​	JDK8以后才可以 IDEA中File->Settings->Java Compiler的Addintional command line parameters 的下面加上 **-parameters**参数即可

### 	getType() 获取参数类型

### 	getClass() 获取参数名称



# 二排序

## [compareTo返回值为-1 、 1 、 0 的排序问题](https://www.cnblogs.com/gengaixue/p/6928654.html)

1.什么是Comparable接口

此接口强行对实现它的每个类的对象进行整体排序。此排序被称为该类的*自然排序* ，类的 `compareTo `方法被称为它的*自然比较方法* 。实现此接口的对象列表（和数组）可以通过 `Collections.sort `（和 `Arrays.sort `）进行自动排序。实现此接口的对象可以用作有序映射表中的键或有序集合中的元素，无需指定比较器。 强烈推荐（虽然不是必需的）使自然排序与 equals 一致。所谓与equals一致是指对于类 `C `的每一个 `e1`和 `e2 `来说，当且仅当 `(e1.compareTo((Object)e2) == 0) `与`e1.equals((Object)e2) `具有相同的布尔值时，类 `C `的自然排序才叫做*与 equals 一致* 。

2.实现什么方法

```
int compareTo(T o)
比较此对象与指定对象的顺序。如果该对象小于、等于或大于指定对象，则分别返回负整数、零或正整数。
强烈推荐 (x.compareTo(y)==0) == (x.equals(y)) 这种做法，但不是 严格要求这样做。一般来说，任何实现 Comparable 接口和违背此条件的类都应该清楚地指出这一事实。推荐如此阐述：“注意：此类具有与 equals 不一致的自然排序。”
参数：
       o - 要比较的对象。 
返回：
        负整数、零或正整数，根据此对象是小于、等于还是大于指定对象。 
抛出：
        ClassCastException - 如果指定对象的类型不允许它与此对象进行比较。
```



3.实例

```java
package test1;

public class Note<T> implements Comparable<Note<T>> {

    private T data; //数据
    private int weight; //权值
    private Note<T> left; //左孩子
    private Note<T> right; //右孩子
    
    public Note(T data,int weight){
        this.data=data;
        this.weight=weight;
    }
    
    @Override
    public String toString(){
        return "data="+this.data+",weitht="+this.weight;
    }    
    
    public T getData() {
        return data;
    }
    public void setData(T data) {
        this.data = data;
    }

    public int getWeight() {
        return weight;
    }
    public void setWeight(int weight) {
        this.weight = weight;
    }

    public Note<T> getLeft() {
        return left;
    }
    public void setLeft(Note<T> left) {
        this.left = left;
    }

    public Note<T> getRight() {
        return right;
    }
    public void setRight(Note<T> right) {
        this.right = right;
    }
    
    
    /**
     * 倒序排列。
     */
    @Override
    public int compareTo(Note<T> o) {
        if(o.weight>this.weight){
            return 1;
        }else if(o.weight<this.weight){
            return -1;
        }
        return 0;
    }
    /**
     * 升序排列
     */
//    @Override
//    public int compareTo(Note<T> o){
//        if(this.weight>o.weight){
//            return 1;
//        }else if(this.weight<o.weigh){
//            return -1;
//        }
//        return 0;
//    }
```

**Ps：（快速记忆法）当前对象与后一个对象进行比较，如果比较结果为1进行交换，其他不进行交换。**

**当后一个对象比当前对象大，返回结果值为1时，前后交换，说明是倒序排列。**

**当后一个对象比当前对象小，返回结果值为1时，前后交换，说明是升序排列。**





## 自定义排序

### PriorityQueue

```java
 PriorityQueue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>(){
            public int compare(Integer x,Integer y){
                return y - x;
            }
        });
```



### 实现 Comparator 接口的复写 compare() 方法，代码如下:

```java

public class Test {
    public static void main(String[] args) {
        /*
         * 注意，要想改变默认的排列顺序，不能使用基本类型（int,double,char）而要使用它们对应的类
         */
        Integer[] a = { 9, 8, 7, 2, 3, 4, 1, 0, 6, 5 };
        // 定义一个自定义类MyComparator的对象
        Comparator cmp = new MyComparator();
        Arrays.sort(a, cmp);
        for (int arr : a) {
            System.out.print(arr + " ");
        }
    }
}
// 实现Comparator接口
class MyComparator implements Comparator<Integer> {
    @Override
    public int compare(Integer o1, Integer o2) {
        /*
         * 如果o1小于o2，我们就返回正值，如果o1大于o2我们就返回负值， 这样颠倒一下，就可以实现降序排序了,反之即可自定义升序排序了
         */
        return o2 - o1;
    }
}
```

# 三数组

### Arrays.copyOfRange

**Arrays.copyOfRange**支持：**boolean[]， byte[] ，char[]，double[]，float[]，int[]，long[]\**以及\**泛型的 T[]**

平常我们说copyOfRange(original,int from,int to)该方法是从original数组的下标from开始赋值，到下标to结束，不包括下标为to的元素。实际上看完代码应该这样理解copyOfRange(original,int from,int to)该方法返回一个长度为to-from的数组，其中from~min(original.length,to)之间的元素（不包括min(original.length,to)）是从数组original复制的元素，剩下的值为0。

## Java中ArrayList转成二维数组以及int[]数组和ArrayList＜Integer＞转换



```java
package com.xunfang.epay.util;
 
import java.util.ArrayList;
 
public class TwoArray {
	// 数组转换问题
	public static void main(String[] args) {
		String str[][] = { { "a1", "a2", "a3" }, { "b1", "b2" },
				{ "c1", "c2", "c3", "c4" } };
 
		String arr1[] = { "a1", "a2", "a3" };
		String arr2[] = { "b1", "b2" };
		String arr3[] = { "c1", "c2", "c3", "c4" };
		String strTwo[][] = new String[3][];
		String strone1[];
 
		ArrayList<String> list1, list2, list3;
		list1 = new ArrayList<String>();
		list1.add("a1");
		list1.add("a2");
		list1.add("a3");
 
		list2 = new ArrayList<String>();
		list2.add("b1");
		list2.add("b2");
 
		list3 = new ArrayList<String>();
		list3.add("c1");
		list3.add("c2");
		list3.add("c3");
		list3.add("c4");
 
		ArrayList<ArrayList<String>> listTwo = new ArrayList<ArrayList<String>>();
		listTwo.add(list1);
		listTwo.add(list2);
		listTwo.add(list3);
 
		// 转成一维数组
		strone1 = list1.toArray(new String[list1.size()]);
		String strone2[] = list2.toArray(new String[list2.size()]);
		String strone3[] = list3.toArray(new String[list3.size()]);
 
		// printOne(strone1);
 
		// 转成二维数组
		strTwo[0] = strone1;
		strTwo[1] = strone2;
		strTwo[2] = strone3;
		printTwo(strTwo);
	}
 
	private static void printOne(String arr[]) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + ",");
		}
	}
 
	private static void printTwo(String arr[][]) {
		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[i].length; j++) {
				System.out.print(arr[i][j] + ",");
			}
			System.out.println();
		}
	}
}

```

下面方法会更简单

```JAVA
List<int[]> list = new ArrayList<>();
list.toArray(new int[0][]);
```

为什么写0？看ArrayList的toArray源码

```JAVA
public <T> T[] toArray(T[] a) {
    if (a.length < size)
        // Make a new array of a's runtime type, but my contents:
        return (T[]) Arrays.copyOf(elementData, size, a.getClass());
    System.arraycopy(elementData, 0, a, 0, size);
    if (a.length > size)
        a[size] = null;
    return a;
}
```

当T泛型的长度没有list的size大时，会以size长度返回T；否则，使用传入的T。所以使用时不管写0还是其它数字，都没关系，习惯写0。

关于List和int[]相互转换,int是Java中基本类型，Integer是包装类型，转换会出现问题。
//判断两数组交集。

```JAVA
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        List<Integer> res = new ArrayList<>();
        for(int n1: nums1){
            set.add(n1);
        }
        for(int n2: nums2){
            if(set.contains(n2))
                res.add(n2);
        }
        //出错。这是因为ArrayList中规定的泛型是Integer，不能直接转化到int[]
        return (int[])res.toArray(new int[res.size()]);
    }
}
//		  Integer转成Integer数组没有问题。
//        List<Integer> res = new ArrayList<>();
//        res.add(1);
//        res.add(2);
//        res.add(3);
//        Integer[] ans = res.toArray(new Integer[res.size()]);
```

该怎么转换，使用流操作





```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        int[] data = {4, 5, 3, 6, 2, 5, 1};    
		// int[] 转 List<Integer>
        List<Integer> list1 = Arrays.stream(data).boxed().collect(Collectors.toList());
        // Arrays.stream(arr) 可以替换成IntStream.of(arr)。
        // 1.使用Arrays.stream将int[]转换成IntStream。
        // 2.使用IntStream中的boxed()装箱。将IntStream转换成Stream<Integer>。
        // 3.使用Stream的collect()，将Stream<T>转换成List<T>，因此正是List<Integer>。

        // int[] 转 Integer[]
        Integer[] integers1 = Arrays.stream(data).boxed().toArray(Integer[]::new);
        // 前两步同上，此时是Stream<Integer>。
        // 然后使用Stream的toArray，传入IntFunction<A[]> generator。
        // 这样就可以返回Integer数组。
        // 不然默认是Object[]。

        // List<Integer> 转 Integer[]
        Integer[] integers2 = list1.toArray(new Integer[0]);
        //  调用toArray。传入参数T[] a。这种用法是目前推荐的。
        // List<String>转String[]也同理。

        // List<Integer> 转 int[]
        int[] arr1 = list1.stream().mapToInt(Integer::valueOf).toArray();
        // 想要转换成int[]类型，就得先转成IntStream。
        // 这里就通过mapToInt()把Stream<Integer>调用Integer::valueOf来转成IntStream
        // 而IntStream中默认toArray()转成int[]。

        // Integer[] 转 int[]
        int[] arr2 = Arrays.stream(integers1).mapToInt(Integer::valueOf).toArray();
        // 思路同上。先将Integer[]转成Stream<Integer>，再转成IntStream。

        // Integer[] 转 List<Integer>
        List<Integer> list2 = Arrays.asList(integers1);
        // 最简单的方式。String[]转List<String>也同理。

        // 同理
        String[] strings1 = {"a", "b", "c"};
        // String[] 转 List<String>
        List<String> list3 = Arrays.asList(strings1);
        // List<String> 转 String[]
        String[] strings2 = list3.toArray(new String[0]);
    }
}
```




