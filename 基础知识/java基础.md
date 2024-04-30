# **java的类、对象、继承、多态、封装**



## 类



在Java类中，属性（fields）、方法（methods）、和构造器（constructors）是类的三个主要成员，它们共同构成了类的结构。



属性是类的成员变量，用于存储对象的**状态或数据**

方法是类定义的**行为或操作**

构造器是一个特殊的方法，用于在创建对象时初始化对象的状态，包括**属性的赋值和其他初始化操作**



打个比方：

**一个车是一个类，车的属性可以是车厂、车龄等，车的方法可以是跑等这些行为，而车的构造器可以看成是对车进行出厂设置**



```java
public class car {
    // 属性
    private String name;
    private String color;

    // 构造器
    public car(String name, String color) {
        this.name = name;
        this.color = color;
    }

    // 方法
    public void init_car() {
        System.out.println("car: " + name + ", color: " + color);
    }

    public static void main(String[] args) {
        car car1 = new car("比亚迪", "black"); 
        car1.init_car(); 
    }
}
```
