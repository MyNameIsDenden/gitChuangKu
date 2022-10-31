**Java PriorityQueue**类是一种队列数据结构实现，其中根据**优先级**处理对象。它与遵循[FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics))（先进先出）算法的标准队列不同。

在优先级队列中，添加的对象根据其优先级。默认情况下，优先级由对象的自然顺序决定。队列构建时提供的[比较](https://howtodoinjava.com/java/collections/java-comparator/)器可以覆盖默认优先级。

## 1. PriorityQueue功能

让我们记下PriorityQueue上的几个要点。

- PriorityQueue是一个无限制的队列，并且动态增长。默认初始容量`'11'`可以使用相应构造函数中的**initialCapacity**参数覆盖。
- 它不允许NULL对象。
- 添加到PriorityQueue的对象必须具有可比性。
- **默认情况下，**优先级队列的对象**按自然顺序排序**。
- 比较器可用于队列中对象的自定义排序。
- 优先级队列的**头部**是基于自然排序或基于比较器的排序的**最小**元素。当我们轮询队列时，它从队列中返回头对象。
- 如果存在多个具有相同优先级的对象，则它可以随机轮询其中任何一个。
- PriorityQueue **不是线程安全的**。`PriorityBlockingQueue`在并发环境中使用。
- 它为add和poll方法提供了**O（log（n））**时间。

## 2. Java PriorityQueue示例

让我们看看对象的排序如何影响PriorityQueue中的添加和删除操作。在给定的示例中，对象是类型的`Employee`。Employee类实现**Comparable**接口`'id'`，默认情况下，该对象使employee对象可以与对象进行比较。

```java
public class Employee implements Comparable<Employee> {

    private Long id;
    private String name;
    private LocalDate dob;
    public Employee(Long id, String name, LocalDate dob) {
        super();
        this.id = id;
        this.name = name;
        this.dob = dob;
    }

    @Override
    public int compareTo(Employee emp) {
        return this.getId().compareTo(emp.getId());
    }


    //Getters and setters
    @Override
    public String toString() {
        return "Employee [id=" + id + ", name=" + name + ", dob=" + dob + "]";
    }

}
```

2.1。自然排序

Java PriorityQueue示例，用于添加和轮询基于其自然顺序进行比较的元素。

```java
PriorityQueue<Employee> priorityQueue = new PriorityQueue<>();
priorityQueue.add(new Employee(1l, "AAA", LocalDate.now()));
priorityQueue.add(new Employee(4l, "CCC", LocalDate.now()));
priorityQueue.add(new Employee(5l, "BBB", LocalDate.now()));
priorityQueue.add(new Employee(2l, "FFF", LocalDate.now()));
priorityQueue.add(new Employee(3l, "DDD", LocalDate.now()));
priorityQueue.add(new Employee(6l, "EEE", LocalDate.now()));
while(true)
{
    Employee e = priorityQueue.poll();
    System.out.println(e);
    if(e == null) break;
}
```

节目输出。

```java
Employee [id=1, name=AAA, dob=2018-10-31]
Employee [id=2, name=FFF, dob=2018-10-31]
Employee [id=5, name=BBB, dob=2018-10-31]
Employee [id=4, name=CCC, dob=2018-10-31]
Employee [id=3, name=DDD, dob=2018-10-31]
Employee [id=6, name=EEE, dob=2018-10-31]
```

2.2。使用比较器进行自定义排序

让我们使用**[基于Java 8 lambda的比较器](https://howtodoinjava.com/java8/using-comparator-becomes-easier-with-lambda-expressions-java-8/)**语法重新定义自定义排序并验证结果。

```java
//Comparator for name field
Comparator<Employee> nameSorter = Comparator.comparing(Employee::getName);
PriorityQueue<Employee> priorityQueue = new PriorityQueue<>( nameSorter );
priorityQueue.add(new Employee(1l, "AAA", LocalDate.now()));
priorityQueue.add(new Employee(4l, "CCC", LocalDate.now()));
priorityQueue.add(new Employee(5l, "BBB", LocalDate.now()));
priorityQueue.add(new Employee(2l, "FFF", LocalDate.now()));
priorityQueue.add(new Employee(3l, "DDD", LocalDate.now()));
priorityQueue.add(new Employee(6l, "EEE", LocalDate.now()));
while(true)
{
    Employee e = priorityQueue.poll();
    System.out.println(e);
    if(e == null) break;
}
```

节目输出。

```java

Employee [id=1, name=AAA, dob=2018-10-31]
Employee [id=5, name=BBB, dob=2018-10-31]
Employee [id=4, name=CCC, dob=2018-10-31]
Employee [id=3, name=DDD, dob=2018-10-31]
Employee [id=6, name=EEE, dob=2018-10-31]
Employee [id=2, name=FFF, dob=2018-10-31]
```

## 3. Java PriorityQueue构造函数

PriorityQueue类提供了6种在Java中构造优先级队列的方法。

- **PriorityQueue（）**：使用默认初始容量（11）构造空队列，该容量根据其自然顺序对其元素进行排序。
- **PriorityQueue（Collection c）**：构造包含指定集合中元素的空队列。
- **PriorityQueue（int initialCapacity）**：构造具有指定初始容量的空队列，该容量根据其自然顺序对其元素进行排序。
- **PriorityQueue（int initialCapacity，Comparator comparator）**：构造具有指定初始容量的空队列，该容量根据指定的比较器对其元素进行排序。
- **PriorityQueue（PriorityQueue c）**：构造包含指定优先级队列中元素的空队列。
- **PriorityQueue（SortedSet c）**：构造包含指定有序集合中元素的空队列。

## 4. Java PriorityQueue方法

PriorityQueue类下面给出了重要的方法，你应该知道。

- **boolean add（object）**：将指定的元素插入此优先级队列。
- **boolean offer（object）**：将指定的元素插入此优先级队列。
- **boolean remove（object）**：从此队列中删除指定元素的单个实例（如果存在）。
- **Object poll（）**：检索并删除此队列的头部，如果此队列为空，则返回null。
- **Object element（）**：检索但不删除此队列的头部，如果此队列为空，则返回null。
- **Object peek（）**：检索但不删除此队列的头部，如果此队列为空，则返回null。
- **void clear（）**：从此优先级队列中删除所有元素。
- **Comparator comparator（）**：返回用于对此队列中的元素进行排序的比较器，如果此队列根据其元素的自然顺序排序，则返回null。
- **boolean contains（Object o）**：如果此队列包含指定的元素，则返回true。
- **Iterator iterator（）**：返回此队列中元素的迭代器。
- **int size（）**：返回此队列中的元素数。
- **Object [] toArray（）**：返回包含此队列中所有元素的数组。

## 5.结论

在这个**Java队列教程中**，我们学会了使用**PriorityQueue类**，它能够通过默认的自然顺序或自定义排序来指定比较器来存储元素。

我们还了解了PriorityQueue类的一些重要方法和[构造函数](https://howtodoinjava.com/oops/java-constructors/)。