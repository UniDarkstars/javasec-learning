# **java的类，面向对象{继承|封装|多态}**



## 类

在Java类中，属性（fields）、方法（methods）、和构造器（constructors）是类的三个主要成员，它们共同构成了类的结构。



1. **属性**是类的成员变量，用于存储对象的状态或数据
2. **方法**是类定义的行为或操作
3. **构造器**是一个特殊的方法，用于在创建对象时初始化对象的状态，包括属性的赋值和其他初始化操作



打个比方：

> **一个车是一个类，车的属性可以是车厂、车龄等，车的方法可以是跑等这些行为，而车的构造器可以看成是对车进行出厂设置**

```java
public class car {
    // 属性
    private String brand;
    private String color;

    // 构造器
    public car(String brand, String color) {
        this.name = name;
        this.color = color;
    }

    // 方法
    public void init_car() {
        System.out.println("car: " + name + ", color: " + color);
    }

    public static void main(String[] args) {
        car car1 = new car("华为", "white"); 
        car1.init_car(); 
    }
}
```



### 继承

在Java中，继承是面向对象编程的重要概念之一，它允许一个类（称为子类或派生类）基于另一个类（称为父类或基类）创建新类，并继承父类的属性和方法

**extends关键字**建立类之间的继承关系，子类通过extends关键字来继承父类



**父类（基类）**：定义了通用的属性和行为，子类可以继承这些属性和行为

**子类（派生类）**：通过继承父类，可以获得父类的属性和方法，并可以添加自己的特定属性和方法



#### 继承的特点



- **单继承**：Java不支持多重继承（一个子类只能有一个直接父类），但支持多层继承（一个类可以有多层父类） 

  > 接口有多继承

- **重写**：子类可以重写父类的方法以提供特定实现。

- **访问修饰符**：子类可以访问父类的公共和受保护成员，但不能访问私有成员。

- **构造器的继承**：子类会默认调用父类的无参构造器，但如果父类没有无参构造器，子类必须显式调用父类的构造器



代码示例：

```java
// 父类
class Vehicle{
    String brand;
    int year;

    public Vehicle(String brand, int year){
        this.brand = brand;
        this.year = year;
    }
    public void displayInfo(){
        System.out.println("Brand: " + brand+ " Year: " + year);
    }
}

//子类Car 继承 父类 Vehicle
class Car extends Vehicle{

    int wheels;
    public Car(String brand, int year, int wheels){
        super(brand, year); // 调用父类的构造器
        this.wheels = wheels; // 这里要赋值wheels哦
    }

    public void displayCarInfo(){
        System.out.println("Brand: " + brand+ " Year: " + year + " Wheels: " + wheels);
    }
}

public class Main {
    public static void main(String[] args) {
        Car myCar = new Car("huawei", 2024,4);
        myCar.displayInfo(); // 调用父类方法，虽然说新建的对象是Car类，但是Car继承了Vehicle的方法，所以可以调用
        myCar.displayCarInfo(); //调用子类方法
    }
}
```

父类 `Vehicle`，它包含车辆的属性和方法，以及一个子类 `Car`，继承了 `Vehicle` 类，并添加了特定于汽车的属性和方法



### 封装

指的是将数据（属性）和方法（行为）捆绑在一起，并对外部隐藏对象的内部实现细节。

封装可以帮助确保数据的安全性和完整性，并提供简洁的接口供其他对象进行交互

概括就是：**想给你用的给你用，不想给你用的不给你用**



**实现封装--访问修饰符**：

- **private**：私有的属性和方法只能在声明它们的类内部访问，外部类无法直接访问
- **default**：受保护的属性只有同一个类或同一个包中可以使用
- **protected**：受保护的属性和方法可以被当前类、同一包内的类以及子类访问
- **public**：公共的属性和方法，可以被任何类访



下列给出一个运行不了的代码，因为System.out.println(a.testPrivate)不被允许

```java
class A {
    private String testPrivate;
    String testDefault; //这个是default，即默认
    protected String testProtected;
    public String testPublic;
}

class B {

    public void test(){
        A a = new A();
        System.out.println(a.testProtected);
        System.out.println(a.testDefault);
        System.out.println(a.testPublic);
        System.out.println(a.testPrivate);//程序报错

    }
}
```



### 多态

允许不同类的对象对同一消息作出响应，表现出不同的行为。在多态性中，同样的方法调用可以在不同的对象上产生不同的行为，**即同一个方法调用，不同对象的行为不同**



**使用多态的方法:**

1. **多态的实现**：多态性可以通过继承和重写实现。在父类中定义一个方法，子类可以对该方法进行重写，从而实现多态性。
2. **方法重写**：子类可以重写（覆盖）父类的方法，以实现特定的行为。在运行时，根据对象的实际类型调用相应的方法。
3. **动态绑定（动态多态）**：在运行时确定调用的方法，而不是在编译时。通过动态绑定，可以使程序更加灵活，根据对象的实际类型来决定调用哪个方法



**实现多态的条件**：

- 继承关系：子类继承自父类。
- 方法重写：子类重写父类的方法。
- 父类引用指向子类对象：通过父类的引用指向子类的对象，可以实现多态性



以下代码重写了makeSound()，实现了不同子类对象的多态

```java
// 父类 Animal
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// 子类 Dog 继承自 Animal
class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

// 子类 Cat 继承自 Animal
class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();

        myDog.makeSound(); // 输出 "Dog barks"
        myCat.makeSound(); // 输出 "Cat meows"
    }
}
```
