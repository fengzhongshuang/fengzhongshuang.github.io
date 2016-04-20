---
title:  "Java学习之集合三"
summary:  Java集合
categories: [Java]
tags: [集合]
---

本文根据 [The Java Tutorials](http://docs.oracle.com/javase/tutorial/) 整理 Java 集合相关内容。主要对 Implementations 部分进行介绍。

### 一、实现
可以把实现分为以下几类：
  * 通用目的的实现，指最常用的实现方式，如图所示
  ![通用实现](/images/posts/java-core-collections-implementations.png)
  * 特殊目的的实现，用于特殊场景的实现方式
  * 并发实现，用于高并发的实现方式，参见 `java.util.concurrent`
  * 包装器实现，用于组合其他类型的实现
  * 便利实现，最简单的实现方式，提供便利高效的操作
  * 抽象实现，自定义实现方式的骨架结构

  > 对于 Set 接口， HashSet 是最常用的实现  
  > 对于 List 接口， ArrayList 是最常用的实现  
  > 对于 Map 接口， HashMap 是最常用的实现  
  > 对于 Queue 接口， LinkedList 是最常用的实现  
  > 对于 Deque 接口， ArrayDeque 是最常用的实现  

### 二、Set实现
* 通用实现

Set 有三个通用版本的实现， `HashSet`，`TreeSet`，`LinkedHashSet`。HashSet 比 TreeSet 要快的多，但是不能保证有序。如果需要按值排序遍历，则使用 TreeSet。LinkedHashSet 是折中方案，由哈希表和链表构成，它的速度接近 HashSet，又是按值插入顺序排序的。

* 特殊实现

Set 有两个特殊版本的实现，`EnumSet` 和 `CopyOnWriteArraySet`。

EnumSet 是一个为枚举类型设计的高性能实现，所有的元素必须是相同的枚举类型。

CopyOnWriteArraySet 是一个用 copy-on-write 数组实现的 Set 实现。适用于经常遍历却很少修改的情况。

### 三、List实现
* 通用实现

有两个通用版本实现，`ArrayList`，`LinkedList`。多数情况下使用ArrayList，它提供了常量时间的访问操作。LinkedList适用于添加或删除操作较频繁的场景，

* 特殊实现

CopyOnWriteArrayList 是一个用 copy-on-write 数组实现的 List 实现。
### 四、Map实现
* 通用实现

有三个通用版本的实现， `HashMap`，`TreeMap`，`LinkedHashMap`。

* 特殊实现

有三个通用版本的实现， `numMap, WeakHashMap and IdentityHashMap`。

* 并发实现

` ConcurrentHashMap` 是一个基于哈希表的高并发，高性能实现。

### 五、Queue实现
* 通用实现

有两个通用版本的实现， `LinkedList`，`PriorityQueue`。PriorityQueue是一个基于堆结构的优先级队列，

* 并发实现

  * BlockingQueue
  * LinkedBlockingQueue — 基于链表实现的阻塞队列
  * ArrayBlockingQueue — 基于数组实现的阻塞队列
  * PriorityBlockingQueue — 基于堆实现的优先级阻塞队列
  * DelayQueue — 基于堆实现的时间调度队列
  * SynchronousQueue — 使用BlockingQueue的简易渲染机制。

### 六、Deque实现

* 通用实现

有两个通用版本的实现， `LinkedList`，`ArrayDeque`。  ArrayDeque 是一个基于可变数组的实现。

* 并发实现

  * LinkedBlockingQueue — 基于链表实现的阻塞队列

### 七、Wrapper实现
这些实现都在 `Collections` 类中，使用了装饰器模式来实现。

* 同步包装器

为集合添加了线程安全的保证，对应着每个接口都有一个方法实现。
```Java
public static <T> Collection<T> synchronizedCollection(Collection<T> c);
public static <T> Set<T> synchronizedSet(Set<T> s);
public static <T> List<T> synchronizedList(List<T> list);
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m);
public static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s);
public static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m);
```

* 不可变包装器

去除集合的修改操作，当执行修改时会抛出异常。主要用于两个场景：
  * 保证集合一旦创建就不可变
  * 使客户端以只读的方式访问设定的数据结构

```Java
public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c);
public static <T> Set<T> unmodifiableSet(Set<? extends T> s);
public static <T> List<T> unmodifiableList(List<? extends T> list);
public static <K,V> Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> m);
public static <T> SortedSet<T> unmodifiableSortedSet(SortedSet<? extends T> s);
public static <K,V> SortedMap<K, V> unmodifiableSortedMap(SortedMap<K, ? extends V> m);
```
* 检查接口包装器

使用Collections.checked来保证泛型集合的类型安全

### 八、便利实现
* 数组的列表视图  Arrays.asList
* 不可变的Multiple-Copy列表   Collections.nCopies，有两个使用场景
  * 用来初始化一个新创建的列表
  ```Java
  List<Type> list = new ArrayList<Type>(Collections.nCopies(1000, (Type)null);
  ```
  * 用来扩展一个已有列表
  ```Java
  lovablePets.addAll(Collections.nCopies(69, "fruit bat"));
  ```
* 不可变的单例Set  Collections.singleton，使Set只包含一个元素，两个使用场景：
  * 可以用来清除集合中的特定元素。
  ```Java
  c.removeAll(Collections.singleton(e));
  ```
  * 为接收集合作为参数的方法传递仅包含一个元素的集合。
