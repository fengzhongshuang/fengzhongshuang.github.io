---
title:  "Java学习之集合四"
summary:  Java集合
categories: [Java]
tags: [集合]
---

## 算法
这些算法来自于 `Collections` 类的静态方法。

**排序**

使用 `sort` 函数，使列表的元素以升序排列。该函数使用优化的归并排序实现，效率高并且是在位的。

**洗牌**

使用 `shuffle` 函数来完成 `sort` 函数的逆操作，

**常规数据操作**
  * reverse，获取列表的逆序
  * fill，使用给定的值来对列表的每个元素重新赋值完成列表的重新初始化
  * copy，将数据从源列表拷贝到目的列表，目的列表的长度必须大于等于源列表长度，对于超过源列表长度部分的元素值不发生变化。
  * swap，交换列表中指定位置的元素
  * addAll，添加数据到列表中，数据可以是独立的也可以是数组

**查询**

使用 `binarySearch` 算法在已经排好序的列表中查找指定元素。

**组合**

frequency — 统计元素在集合中出现的次数
disjoint — 判断两个集合是否是互斥的

**寻找极值**

使用 `min` 和 `max` 函数来获取集合的最大值和最小值。

## 自定义实现
自定义的实现的几个理由：
* 需要对数据进行持久化
* 应用程序的特定需要
* 高性能的特殊目的
* 高性能的一般目的
* 功能性增强
* 便利性
* 适配

如何编写一个自定义实现：
* 选定抽象实现类
```Java
AbstractCollection — 既不是Set也不是List的集合，需要自定义size方法和迭代器。
AbstractSet — 抽象Set类。
AbstractList — 基于随机访问的List抽象类，需要实现get，set，remove，add 和 size方法。抽象类已实现listIterator。
AbstractSequentialList — 基于顺序访问的List抽象类，需要实现listIterator 和 size 方法。与 AbstractList 相反。
AbstractQueue — 需要实现 offer, peek, poll, size 和支持 remove操作的 iterator。
AbstractMap — 需要实现 entrySet。 通常使用 AbstractSet 类实现。如果Map是可修改的，那必须实现put方法。
```
* 为类中的抽象方法提供实现。
* 测试新的实现。
* 如果关心性能，则改善之。

## 互操作性
* 兼容性

  遗留类型 Vector, Hashtable, array, 和 Enumeration

  * 向上兼容
    * 使用 `Arrays.asList` 将 array 转成集合或列表
    * 使用 `Collections.list` 将 Enumeration 转成列表
    * Vector 和 Hashtable 不需要做任何转换即可直接使用
  * 向下兼容
    * 使用 `toArray` 将集合转成数组
    * 使用构造函数将集合转成 Vector 和 Hashtable
    * 使用 `Collections.enumeration` 将集合转成 Enumeration
* API设计
  * 如果使用集合作为API的参数，则应该选择接口而不应该选择具体实现类。
  * 返回值也要尽量使用接口或者特定目的的实现类。
  * 对使用遗留集合类型的接口进行兼容性更新。
