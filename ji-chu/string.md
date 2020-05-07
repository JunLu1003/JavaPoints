---
description: 基于JDK 1.8
---

# Object

## 定义

> Class Object is the root of the class hierarchy. Every class has Object as a superclass. All objects, including arrays, implement the methods of this class.

`Object`类是Java层级关系中的根结点，`Object`类是所有类的基类。所有类，包括数组，都实现类`Object`的方法。

诞生自Java 1.0时代。

{% hint style="info" %}
接口不是`Object`的子集。
{% endhint %}



## 主要方法

### 1. hashcode\(\) & equals\(\)

```java
public native int hashCode();
```

方法中使用了`native`关键字修饰了`hashCode()`方法，是通过JNI接口访问了底层实现，目的是获取当前对象的hash值，单独使用没有什么意义，通常是配合`equals()`方法一起使用。

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

方法实现使用了`==`操作符进行两个对象的比较，如果在自定义对象中没有重写`equals()`方法，那么实际上比较的是两个对象的内存地址，而不是值的比较。



我们经常会听到，重写`equals()`方法时，需要重写`hashcode()`方法，真的需要吗？举个例子：

```java
public class Foo {
    public int        a;
    public String     b;
    
    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Foo)) {
            return false;
        }
        Foo foo = (Foo) obj;
        return this.a == foo.a && (this.b.equals(foo.b)); 
    }
}
```

