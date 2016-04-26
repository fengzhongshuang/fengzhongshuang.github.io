---
title:  "Java 动态代理"
categories: [Java]
tags: [Java]
---

代理，可以解耦客户类与委托类，使客户类与代理类进行交互，代理类对委托类进行封装，可以对客户的请求进行预处理、转发以及过滤等操作。代理的创建可以分为两类，**静态代理** 和 **动态代理**。静态代理就是由开发者自行实现代理类或通过代码生成工具生成代理类，动态代理是指在运行时生成代理类，针对不同的需求，动态代理又有两种实现。

### 代理模式
![代理模式](/images/posts/java-proxy.png)
一张丑陋的图类简单说明下代理模式。代理类将委托类进行封装，客户类对委托类的所有操作都不再直接调用委托类的逻辑，而是通过代理类进行转发，并且在代理类中可以对客户类的调用进行前期处理和后期处理。
![代理模式UML](/images/posts/java-proxy-uml.png)
```Java
// 定义接口
public interface Subject {
    void sayHello(String name);
}

// 委托类
public class RealSubject implements Subject {
    @Override
    public void sayHello(String name) {
        System.out.println("Hello " + name);
    }
}

//代理类
public class ProxySubject implements Subject {
    private Subject subject;
    public ProxySubject(Subject subject){
        this.subject = subject;
    }

    @Override
    public void sayHello(String name) {
        System.out.println("Before....");
        subject.sayHello(name);
        System.out.println("After....");
    }
}

// 客户类
public class Main {
    public static void main(String[] args) {
        Subject subject = new ProxySubject(new RealSubject());
        subject.sayHello("Michel");
    }
}

// 运行结果
Before....
Hello Michel
After....
```

### 静态代理 与 动态代理
**静态代理** 就是由用户自行实现或者通过代码生成工具创建来实现代理类的方式，这种方式最显著的特点就是在类文件中已经明确的实现了代理类的所有代码。

**动态代理** 则是在代码运行时才真正的创建出代理类，在代码编写的过程中是不存在代理类的。动态代理可以通过两种方式实现，第一种是使用JDK提供的工具实现，使用这种方式要求委托类必须实现了某个接口；第二种是使用CGLIB类库，使用这种方式就不需要委托类实现接口了。

### JDK动态代理
```Java
// 定义接口
public interface Subject {
    void sayHello(String name);
}

// 委托类
public class RealSubject implements Subject {
    @Override
    public void sayHello(String name) {
        System.out.println("Hello " + name);
    }
}

// 动态代理工具类
public class ProxyUtil<T> implements InvocationHandler {
    private T object;

    public ProxyUtil(T object) {
        this.object = object;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before...");
        T obj = (T) method.invoke(object, args);
        System.out.println("After...");
        return obj;
    }

    public T getInstance() {
        Class[] interfaces = object.getClass().getInterfaces();
        ClassLoader classLoader = object.getClass().getClassLoader();
        return (T)Proxy.newProxyInstance(classLoader, interfaces, this);
    }
}

// 客户类
public class Main {

    public static void main(String[] args) {
        Subject subject = new ProxyUtil<Subject>(new RealSubject()).getInstance();
        subject.sayHello("Michel");
    }
}
```

通过动态代理工具类就可以在运行期间创建代理类，这种实现方式主要运用的技术就是反射。

### CGLIB动态代理
待补充
