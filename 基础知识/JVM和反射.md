# JVM和反射





# JVM

- **虚拟机**：JVM是一个虚拟计算机，它在实际计算机（如PC、服务器）上模拟一个计算机系统，使得Java跨平台执行
- **解释器**：JVM包含解释器，它负责将Java字节码指令翻译成实际机器代码执行



### 组成

- 类加载器：负责加载类的字节码文件
- 类执行引擎：执行字节码指令
- 内存区域：包括堆、栈、方法区等不同的内存区域
- 垃圾回收器：负责垃圾回收，释放不再使用的内存



### 运行过程

1. 编译：Java源代码经过编译器编译成字节码文件(.class)
2. 类加载：类加载器加载字节码文件并创建对应的Class对象
3. 解释执行：解释器执行字节码指令
4. JIT编译：JVM根据执行情况将热点代码编译成本地机器码提高性能



## 反射

Java反射（Reflection）是指在运行时检查、检索和操作类、接口、字段和方法的能力，通过反射，可以（编译前）运行时获取类的信息并动态调用类的属性和方法



获取到Class实例是反射的基础，可以通过它来获取类的结构信息，JVM创建的 `Class` 实例保存了类的很多信息，因此可以通过该 `Class` 实例来获取该类的信息

> `Class` 是 Java 中的一个类，用于表示类的元数据，支持反射机制；而 `class` 是 Java 的关键字，用于定义类的结构和属性



### 反射步骤

1. 获取Class对象
   使用class属性，每个类都有名为class的属性，可以直接通过该属性获取类的Class对象

   ```java
   Class<Person> clazz = Person.class;
   ```


   调用对象的.getClass()方法

   ```java
   Class<? extends Person> clazz = personObject.getClass();
   ```


   使用Class.forName()方法

   ```java
   Class<?> clazz = Class.forName("com.example.Person");
   ```

   

2. 实例化对象
   使用Class对象的newInstance()方法

   ```java
   Class<?> clazz = Person.class;
   Person person = (Person) clazz.newInstance()
   ```

   使用Constructor.newInstance()方法来实例化对象

   ```java
   Class<?> clazz = Person.class;
   Constructor<?> constructor = clazz.getConstructor();
   Person person = (Person) constructor.newInstance();
   ```

3. 访问字段

   ①`getDeclaredField()` 方法或 `getField()` 方法来获取指定名称的字段对象，前者可以获取所有字段，包括私有字段，后者只能获取公共字段

   ②

   使用 `get()` 方法获取字段的值。

   使用 `set()` 方法设置字段的值

   

4. 调用方法

   ①使用 `getDeclaredMethod()` 方法或 `getMethod()` 方法来获取指定名称和参数类型的方法对象。`getDeclaredMethod()` 可以获取所有方法，包括私有方法，而 `getMethod()` 只能获取公共方法

   ②使用 `invoke()` 方法调用方法



```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}
```

```java
import java.lang.reflect.*;

public class ReflectionExample {
    public static void main(String[] args) throws Exception { //throws Exception对其进行捕获或声明以便抛出
        // 获取 Class 对象
        Class<?> clazz = Person.class;

        // 实例化对象
        Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
        Object personObject = constructor.newInstance("Alice", 30);

        // 访问字段
        Field nameField = clazz.getDeclaredField("name");
        nameField.setAccessible(true);//调用 setAccessible(true) 方法可以允许对私有字段的访问
        nameField.set(personObject, "Bob");

        // 调用方法
        Method displayInfoMethod = clazz.getDeclaredMethod("displayInfo");
        displayInfoMethod.invoke(personObject);
    }
}

// Name: bob, Age: 30
```

先获取了 `Person` 类的 `Class` 对象，然后使用 `getConstructor` 方法获取构造函数，实例化了一个 `Person` 对象

再使用 `getDeclaredField` 方法获取字段 `name`，并通过 `set` 方法修改了字段的值

最后使用 `getDeclaredMethod` 方法获取方法 `displayInfo`，并通过 `invoke` 方法调用了该方法



