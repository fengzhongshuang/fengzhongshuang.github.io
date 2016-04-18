---
title:  "Java学习之集合一"
summary:  Java集合
categories: [Java]
tags: [集合]
---

本文根据 [The Java Tutorials](http://docs.oracle.com/javase/tutorial/) 整理 Java 集合相关内容。主要对Introduction 和 Intefaces 部分进行介绍。

### 一、简介

**1. 集合**

集合 指的是由多个元素组成的一个单元，可以用来存储数据、获取数据、维护数据以及数据间的通信。

**2. 集合框架**

集合框架 指用于表示和操作集合的统一结构。包含以下三个方面：
  * 接口: 表示集合的 **抽象数据类型** 。
  * 实现: 集合接口的具体实现，即可重用的 **数据结构**
  * 算法: 对实现集合接口的对象进行计算的方法。即可重用的 **函数** 。

**3. Java 集合框架的优势**
* 提高编程效率
* 增加程序的速度和质量
* 允许非关联API的互操作
* 降低学习和使用新API的成本
* 降低设计新API的成本
* 提高软件的可重用性

### 二、接口
核心集合接口封装了不同的集合类型。它们是 Java 集合框架的基础。核心接口主要分为 **Collection** 和 **Map** 两大类所有的接口都是泛型的。
![核心集合接口](/images/posts/java-core-collection-interfaces.png)
* Collection: 集合层次的根。Collection 接口是所有集合实现的通用版本， 且用于传递和操作最大泛型数据的需要。该接口没有具体的实现。
* Set: 一个不能存储重复元素的集合。
* List: 一个有序集合（或序列）。可以存储重复的元素。
* Queue: 在 Collection 集合的基础上有新增的操作。通常以 FIFO 的方式组织元素。
* Deque: 在 Collection 集合的基础上有新增的操作。可以通过 FIFO 或 LIFO 的方式组织元素。
* Map: 对象的键值映射。不能包含重复的键。一个键只能对应一个值。
* SortedSet: Set 接口的有序版本。
* SortedMap: Map 接口的有序版本。

**1. Collection 接口**

* 基本操作的方法。
```Java
int size()  获取集合大小
boolean isEmpty()  判断集合是否为空
boolean contains(Object element)  判断集合是否包含element元素
boolean add(E element)   向集合中添加一个元素
boolean remove(Object element)   从集合中删除元素element
Iterator<E> iterator()  获取集合的迭代器
```
* 操作整个集合的方法。
```Java
boolean containsAll(Collection<?> c)  判断当前集合时候包含集合c中的所有元素，可用于判断集合之间时候是超集和子集的关系
boolean addAll(Collection<? extends E> c)   将集合c中的元素全部添加到当前集合中，可用于计算两个集合的并集
boolean removeAll(Collection<?> c)  移除当前集合中与集合c相同的元素，可用于计算两个集合的差集
boolean retainAll(Collection<?> c)  保留当前集合中与集合c相同的元素，可用于计算两个集合的交集
void clear()  清空整个集合
```
* 操作数组的方法。
```Java
Object[] toArray()  将当前集合转成数组
<T> T[] toArray(T[] a)  将当前集合中的元素存入指定类型的数组a中。
```
* 遍历集合
  * 使用聚合函数  在JDK 8 或以后版本中，推荐使用该方法。通过 stream 和相应的聚合函数来操作集合。
  ```Java
  myShapesCollection.stream()
                    .filter(e -> e.getColor() == Color.RED)
                    .forEach(e -> System.out.println(e.getName()));
  ```
  * 使用 for-each
  ```Java
  for (Object o : collection)
    System.out.println(o);
  ```
  * 使用 Iterators
  ```Java
  for (Iterator<?> it = c.iterator(); it.hasNext(); )
          if (!cond(it.next()))
              it.remove();
  ```

**2. Set 接口**
* 三个实现：
  * HashSet  元素存储在一个hash表中，无序的，性能最优。
  * TreeSet  元素存储在一个红黑树种，根据值进行排序，慢于HashSet。
  * LinkedHashSet  元素存储在一个使用链表的hash表中，依据元素插入的顺序排序。
* 求集合的对称差集（元素仅存在于一个集合中）
  ```Java
    Set<Type> symmetricDiff = new HashSet<Type>(s1);
    symmetricDiff.addAll(s2);
    Set<Type> tmp = new HashSet<Type>(s1);
    tmp.retainAll(s2);
    symmetricDiff.removeAll(tmp);
  ```

**3. List 接口**
* 位置存取
```Java
E get(int index)   获取指定位置的元素
E set(int index, T element)  在指定位置设置元素
boolean add()   添加元素
E remove()  删除元素
```
* 查询
```Java
indexOf   获取元素首次出现的位置
lastIndexOf   获取元素末次出现的位置
```
* 迭代
```Java
iterator
listIterator  返回一个 ListIterator 类型的迭代器，可以从任何方向遍历 List，可以在迭代是修改 List 或者获取当前的位置。
for (ListIterator<Type> it = list.listIterator(list.size()); it.hasPrevious(); ) {
    Type t = it.previous();
    ...
}
```
* 范围视图
```Java
subList(int fromIndex, int toIndex)  获取一个左闭右开的子列表。
```

**4. Quque 接口**

元素从头部获得，从尾部插入。每个 Queue 接口的实现都要指定 ordering 属性。
```Java
public interface Queue<E> extends Collection<E> {
    E element();
    boolean offer(E e);
    E peek();
    E poll();
    E remove();
}
```
**5. Deque 接口**

是一个双端 Queue。
* 插入
```Java
addfirst
offerFirst
addLast
offerLast
```
* 删除
```Java
removeFirst
pollFirst
removeLast
pollLast
```
* 读取
```Java
getFirst
peekFirst
getLast
peekLast
```
* 特殊方法
```Java
removeFirstOccurence  移除特定元素第一次的实例
removeLastOccurence  移除特定元素最后一次的实例
```
**6. Map 接口**
* 基本操作
```Java
put
get
containsKey
containsValue
size
isEmpty
```
* 批量操作
```Java
clear
putAll
```
* 集合视图
```Java
keySet
values
entrySet
```

**7. SortedSet 接口**
SortedSet 是一个根据元素升序排序的集合。
```Java
public interface SortedSet<E> extends Set<E> {
    // Range-view
    SortedSet<E> subSet(E fromElement, E toElement);
    SortedSet<E> headSet(E toElement);
    SortedSet<E> tailSet(E fromElement);

    // Endpoints
    E first();
    E last();

    // Comparator access
    Comparator<? super E> comparator();
}
```
* 范围视图操作
```Java
SortedSet<E> subSet(E fromElement, E toElement);  创建一个左闭右开的新的有序集合
SortedSet<E> headSet(E toElement);   从头元素到指定元素的新集合，不包含指定元素
SortedSet<E> tailSet(E fromElement);  从指定元素到为元素的新集合，包含指定元素
```
例如：
```Java
SortedSet<String> volume1 = dictionary.headSet("n");   返回 a-m 的集合，不含n
SortedSet<String> volume2 = dictionary.tailSet("n");   返回 n-z 的集合，包含n
```
* 端点操作
```Java
E first();  获取第一个元素
E last();   获取最后一个元素
```
* Comparator 访问器
```Java
Comparator<? super E> comparator();
```
**8. SortedMap 接口**
SortedMap 是一个按升序排列的 Map。操作方法含义与 SortedSet 接口基本一致。
```Java
public interface SortedMap<K, V> extends Map<K, V>{
    Comparator<? super K> comparator();
    SortedMap<K, V> subMap(K fromKey, K toKey);
    SortedMap<K, V> headMap(K toKey);
    SortedMap<K, V> tailMap(K fromKey);
    K firstKey();
    K lastKey();
}
```
