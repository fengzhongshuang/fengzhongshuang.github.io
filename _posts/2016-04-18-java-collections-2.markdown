---
title:  "Java学习之集合一"
summary:  Java集合
categories: [Java]
tags: [集合]
---

本文根据 [The Java Tutorials](http://docs.oracle.com/javase/tutorial/) 整理 Java 集合相关内容。主要对 Aggregate Operations 部分进行介绍。


### 一、聚合函数
要更好的使用聚合函数，需要先对Java的 `Lambda表达式` 和 `方法引用` 有一定的了解。本文将略过这两部分内容，在后面的文章中进行介绍。

**1. 管道和流**
管道指一系列的聚合操作。它包括以下三个部分：
  * 一个数据源，可以是一个集合、数组、生成器或I/O通道。
  * 0个或多个中间操作
  * 1个终止操作

**2. 聚合函数和迭代器的区别**
  * 聚合函数使用内部迭代，使用内部迭代时，应用程序仅决定要迭代的数据集，具体的迭代实现由JDK完成，而使用外部迭代，两个方面都由应用程序决定。外部迭代仅限于集合，而内部迭代没有限制，并且可以充分利用并行计算的优势。
  * 通过流来处理数据
  * 支持函数行为作为参数

### 二、Reduction
Reduction操作是指那些通过组装流中数据来返回一个值的终止函数，例如 `average, sum, min, max, and count`。

**Stream.reduce 方法**

该方法接收两个参数：
  * identity，它既是reduce操作的初始值，也是流中无数据的默认返回值。
  * accumulator，累加器包含两个参数，前面部分的reduce结果和下一个要处理的元素

**Stream.collect 方法**

该方法接收三个参数：
  * supplier，它是一个创建新实例的工厂方法，该实例作为结果的容器。
  * accumulator，将流元素组装到结果容器中的方法。
  * combiner，接收两个结果容器，并合并它们的内容的方法
accumulator 和 combiner 不能有返回值
### 三、Parallelism
并行计算是将一个问题分解为多个子问题，然后同时对每个子问题进行处理，然后将处理的结果合并起来的方式。并行计算需要注意多线程问题。然而聚合操作和并行流能够使非线程安全的集合实现并行性。

在进行并行计算时需要使用 `parallelStream()`，还有专门用于并行的操作。 
