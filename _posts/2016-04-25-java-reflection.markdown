---
title:  "Java 反射"
categories: [Java]
tags: [反射]
---

反射是Java框架常用的技术之一，通过类的全限定名来创建实例，调用实例方法等在运行时对类进行预期的操作，为系统提供了更强的扩展性，但是使用反射机制却会影响系统的性能，并且使用反射还可以绕过属性的访问限制，可以自由的操作`private`属性，所以在选择是否使用反射时要慎重考虑，综合判断。

实例介绍：
```Java
// 实体类定义
package io.github.fengzhongshuang.model;
public class Rectangle{
  private int length;
  private int width;

  public Rectangle(){
    this.length = 0;
    this.width = 0;
  }
  public Rectangle(int length, int width){
    this.length = length;
    this.width = width;
  }

  public int getArea(){
    return this.length * this.width;
  }

  public int getCircle(){
    return (this.length + this.width) * 2;
  }
}
```

```Java
package io.github.fengzhongshuang.test;
public class RectangleTest{
  public static void main(String[] args){
    Class clazz = Class.forName("io.github.fengzhongshuang.model.Rectangle");

    // 获取构造函数
    Constructor constructor = clazz.getConstructor(Integer.TYPE, Integer.TYPE);
    // 通过构造函数创建实例
    Rectangle rectangle = (Rectangle) constructor.newInstance(10, 20);
    System.out.println(rectangle);

    // 获取实例方法
    Method method = clazz.getDeclaredMethod("getArea");    
    // 调用实例方法
    int area = (Integer) method.invoke(rectangle);
    System.out.println("area = " + area);
  }
}
```

除此之外，还可以通过`Class`获取实现的接口、定义的属性、注解等等，获取整个类的完整实现。

使用反射，`Hibernate` 将数据库表与实体类完成映射，`Struts` 根据请求实现 `Controller` 中 `Action` 的调用，`Spring` 创建指定的 `Bean`。

使用反射，你可以实现自己的ORM映射工具，可以实现自己的MVC框架，也可以实现代码的灵活替换。

还是那句话，反射虽然带来了便利，但是也是有牺牲的便利，是否需要反射，一定要综合各种因素考虑衡量后做出决定。
