---
title:  "JAVA基础"
summary: 本文是JDBC系列文章的目录，该系列文章将从7个方面对JDBC进行介绍。
categories: [Java]
tags: [J2SE]
---

## 1、使用`strictfp`关键字标记的方法必须使用严格的浮点计算来产生理想的结果。  
主要是在不同的处理器上对于浮点数的运算处理的差异导致，为了平衡性能和理想结果做出的选择。

## 2、`&&`、`||` 和 `&`、`|`的异同
 * `&&`、`||` 是逻辑运算符，`&`、`|`是位运算符
 * `&&`、`||`和`&`、`|`都可以用来做逻辑判断，只是前者是** 短路方式 **计算，后者不是 ** 短路方式 **
 * `&&`、`||`比 `&`、`|`的运算符优先级 ** 低 **

## 3、`>>`、`>>>` 右移运算符、`<<`左移运算符
`>>`和`<<`是使用 ** 符号位 **来填充高位
`>>>`是使用 ** 0 **来填充高位
例如：
```java
int num1 = 4 >> 2;  // num1 = 2
int num2 = 4 >>> 2; // num2 = 2

int num3 = -4 >> 2;  // num3 = -2 (0b11111111111111111111111111111110)
int num4 = -4 >>> 2; // num4 = 2147483646 (0b01111111111111111111111111111110)
```
整数的二进制表示  
正数的二进制 = 二进制原码  
负数的二进制 = abs(负数)的二进制的反码 + 1   
例如：  
5的二进制

0b00000000000000000000000000000101

-5的二进制
* 得到5的二进制 0b00000000000000000000000000000101
* 取反 0b11111111111111111111111111111010
* 再加1 0b11111111111111111111111111111011

## 4、`StrictMath`与`Math`
前者提供的方法在任何平台上保证得到相同的结果，后者则追求更高的运算效率。

## 5、`Code Unit(代码单元)`和`Code Point(代码点)` ?????

## 6、字符串格式化 ????
![格式说明符](/images/posts/String_Format.png)

## 7、数组的交集、并集、差集、补集？？？

## 8、NumberFormat、DecimalFormat、MessageFormat、ChoiceFormat ？？

## 9、对用对象类型的getter方法，要返回对象的clone

## 10、初始化块、静态初始化块
* 初始化块在每次创建对象的时候都会调用  
* 静态初始化块只在创建第一个对象的时候调用，再次创建对象是不会再次调用  
* 创建对象时，先执行静态初始化块（仅执行一次），在执行普通初始化块，然后执行构造函数。
* 如果类存在继承关系，那么执行顺序需要特别注意  
```Java
/*
 *Parent Class
 */
public class Parent {
    static {
        System.out.println("Parent static block");
    }

    {
        System.out.println("Parent common block");
    }

    public Parent() {
        System.out.println("Parent Constructor");
    }
}
```
```Java
/*
 *Child Class
 */
public class Child extends Parent {
    static {
        System.out.println("Child static block");
    }

    {
        System.out.println("Child common block");
    }

    public Child() {
        super();
        System.out.println("Child Constructor");
    }
}
```
```Java
public class Application {
    public static void main(String[] args) {
        Child c = new Child();
    }
}
/**
 * Output:
 * Parent static block
 * Child static block
 * Parent common block
 * Parent Constructor
 * Child common block
 * Child Constructor
 */
```

## 11、静态绑定（前期绑定）和动态绑定（后期绑定）区别对比
1. 静态绑定发生在编译时期。   
动态绑定发生在运行时。
2. 使用private或static或final修饰的变量或者方法，使用静态绑定。  
而虚方法（可以被子类重写的方法）则会根据运行时的对象进行动态绑定。
3. 静态绑定使用类信息来完成。  
而动态绑定则需要使用对象信息来完成。
4. 重载(Overload)的方法使用静态绑定完成  
 重写(Override)的方法则使用动态绑定完成。
5. 动态绑定比静态绑定更耗时。

## 12、编写equals方法
* 显示参数命名位OtherObject，稍后需要将它转换成另一个叫做other的变量
* 检测this与otherObject是否引用同一个对象
```Java
if (this == otherObject) return true;
```
* 检测otherObject是否为null。
*
```Java
if (null == otherObject) return false;
```
* 比较this与otherObject是否属于同一个类，如果equals的语义在每个子类中有所改变，就使用getClass()检测,如果所有的子类都拥有统一的语义，就使用instanceof检测。
    ```Java
    if (getClass != otherObject.getClass()) return false;

    if(!(otherObject instanceof ClassName)) return false;
    ```
* 将otherObject转换位相应的类类型变量
```Java
ClassName other = (ClassName) otherObject;
```
* 对需要比较的域进行比较，使用==比较基本类型域，使用equals比较对象域，如果所有域都匹配，返回true，否则返回false。
* 如果在子类中重新定义equals，就要调用super.equals(other)。

## 13、内部类？？


## 14、代理？？
