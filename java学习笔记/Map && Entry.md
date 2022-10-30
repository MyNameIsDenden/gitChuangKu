# 一.Java Entry类详解

## 1.Entry类概述

Java的entry是一个静态[内部类](https://so.csdn.net/so/search?q=内部类&spm=1001.2101.3001.7020)，实现Map.Entry< K ,V> 这个接口,通过entry类可以构成一个单向链表。

## 2.java中Map及Map.Entry

（1）.Map是java中的接口，Map.Entry是Map的一个内部接口。
（2）.Map提供了一些常用方法，如keySet()、entrySet()等方法。
（3）.keySet()方法返回值是Map中key值的集合；entrySet()的返回值也是返回一个Set集合，此集合的类型为Map.Entry。
（4）.Map.Entry是Map声明的一个内部接口，此接口为泛型，定义为Entry<K,V>。它表示Map中的一个实体（一个key-value对）。接口中有getKey(),getValue方法。


```java
Iterator<Map.Entry<Integer, Integer>> it=map.entrySet().iterator();
    while(it.hasNext()) {
        Map.Entry<Integer,Integer> entry=it.next();
        int key=entry.getKey();
        int value=entry.getValue();
        System.out.println(key+" "+value);
    }
```

## 3.**Entry类的实现源码**

```java
//源码
private static class Entry<K,V> implements Map.Entry<K,V> {
        int hash;
        final K key;
        V value;
        //next 可构成单向链表
        Entry<K,V> next;

        protected Entry(int hash, K key, V value, Entry<K,V> next) {
            this.hash = hash;
            this.key =  key;
            this.value = value;
            this.next = next;
        }

        protected Object clone() {
            return new Entry<>(hash, key, value,(next==null ? null : (Entry<K,V>) next.clone()));
        }

        // Map.Entry Ops
        public K getKey() {
            return key;
        }

        public V getValue() {
            return value;
        }

        public V setValue(V value) {
            if (value == null)
                throw new NullPointerException();

            V oldValue = this.value;
            this.value = value;
            return oldValue;
        }

        public boolean equals(Object o) {
            if (!(o instanceof Map.Entry))
                return false;
            Map.Entry<?,?> e = (Map.Entry)o;

            return key.equals(e.getKey()) && value.equals(e.getValue());
        }

        public int hashCode() {
            return hash ^ value.hashCode();
        }

        public String toString() {
            return key.toString()+"="+value.toString();
        }
    }

```



## 4.entrySet

entrySet是 java中 键-值 对的集合，Set里面的类型是Map.Entry，一般可以通过map.entrySet()得到。

entrySet实现了Set接口，里面存放的是键值对。一个K对应一个V。
用来遍历map的一种方法。

```java
Set<Map.Entry<String, String>> entryseSet=map.entrySet();

for (Map.Entry<String, String> entry:entryseSet) {

    System.out.println(entry.getKey()+","+entry.getValue());

}
```

即通过getKey（）得到K，getValue得到V。

## 5.keySet

还有一种是keySet, keySet是键的集合，Set里面的类型即key的类型

```java
Set<String> set = map.keySet();

for (String s:set) {

    System.out.println(s+","+map.get(s));

}
```

# 2.四种遍历Map方式:



```java
public static void main(String[] args) {	
    Map<String, String> map = new HashMap<String, String>();
    map.put("1", "value1");
    map.put("2", "value2");
    map.put("3", "value3");

    //第一种：普遍使用，二次取值
    System.out.println("通过Map.keySet遍历key和value：");
    for (String key : map.keySet()) {
        System.out.println("key= "+ key + " and value= " + map.get(key));
    }

    //第二种
    System.out.println("通过Map.entrySet使用iterator遍历key和value：");
    Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
    while (it.hasNext()) {
        Map.Entry<String, String> entry = it.next();
        System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
    }

    //第三种：推荐，尤其是容量大时
    System.out.println("通过Map.entrySet遍历key和value");
    for (Map.Entry<String, String> entry : map.entrySet()) {
        System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
    }

    //第四种
    System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
    for (String v : map.values()) {
        System.out.println("value= " + v);
    }
}
```


