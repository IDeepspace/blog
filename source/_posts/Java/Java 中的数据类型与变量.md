---
title: Java 中的数据类型与变量
author: Deepspace
tags:
  - Java
categories: Java
date: 2017-06-29
urlname: java-data-type
---

<!-- ## Java 中的数据类型与变量 -->

### 一、强类型语言

`Java` 是一种强类型语言。这就意味着必须为每一个变量声明一种类型。



### 二、数据类型

在 `Java` 中，把数据分为两大类：基本数据类型、引用数据类型。

#### 1、基本数据类型

基本数据类型有 `8` 种，可以分为 `4` 类：

- 整数类型：`byte`，`short`，`int`，`long`；

- 浮点数类型：`float`，`double`

- 字符类型：`char`

- 布尔类型：`boolean`

不同的数据类型占用的字节数不一样。
<!-- more -->

##### 1.1、整数类型

整型用于表示没有小数部分的数值， 它**允许是负数**。 `Java` 提供了 `4` 种整型。

**int**

- 存储需求：`4` 字节
- 取值范围：`-2147483648 ~ 2147483647` ( 正好超过 `20` 亿 )

**short**

- 存储需求：`2` 字节
- 取值范围：`-32768 ~ 32767`

**long**

- 存储需求：`8` 字节
- 取值范围：`-9223372036854775B08 ~ 9223372036854775807`

**byte**

- 存储需求：`1` 字节
- 取值范围：`-128 ~ 127`



##### 1.2、浮点类型

浮点类型用于表示有小数部分的数值。在 `Java` 中有两种浮点类型。

**float**

- 存储需求：`4` 字节
- 取值范围：大约 `± 3.40282347E+38F` (有效位数为 `6 ~ 7` 位)

**double**

- 存储需求：`8` 字节
- 取值范围：大约 `± 1.79769313486231570E+308` (有效位数为 `15` 位)

`double` 表示这种类型的数值精度是 `float` 类型的两倍（有人称之为双精度数值）。 绝大部分应用程序都采用 `double` 类型。 

`float` 类型的数值有一个后缀 `F` 或 `f`（例如：`3.14F`） 。没有后缀 `F` 的浮点数值（如`3.14`）默 认为 `double` 类型。当然，也可以在浮点数值后面添加后缀 `D` 或 `d` （例如：`3.14D`）。

从我们生活意义上看，整数是浮点数的子集；从计算机底层上来看，整数和浮点数在内存中的保存形式区别很大，他们的操作方法差别也很大，这就是为什么整数可以进行逻辑运算，而浮点数不行。



##### 1.3、布尔型 `boolean`

`boolean`（布尔）类型有两个值：`false` 和 `true`， 用来判定逻辑条件的真假。**注意：整型值和布尔值之间不能进行相互转换。** 

`Java` 语言对布尔类型的存储并没有做规定，因为理论上存储布尔类型只需要一个字节，但是通常 `JVM` 内部会把 `boolean` 表示为 `4` 个字节整数。



##### 1.4、字符型 `char`

字符类型 `char` 表示一个字符。

`Java` 的 `char` 类型除了可表示标准的 `ASCII` 外，还可以表示一个 `Unicode` 字符。

`char` 类型的字面量值要用单引号括起来。 例如：`'A'` 是编码值为 `65` 所对应的字符常量。

```java
public class Test {
  public static void main(String[] args) {
    char a = 'A';
    char zh = '中';
    System.out.println(a); // A
    System.out.println(zh); // 中
  }
}
```

> 注意：它与 `"A"` 不同，`"A"` 是包含一个字符 `A` 的字符串。

```java
String str = "JavaBeginners";
```



#### 2、引用数据类型

引用数据类型和基本数据类型，最大区别就是在**存储形式**上。

在栈中，**只保存有基本类型的变量和引用数据类型对象的引用，引用所指向的对象保存在堆中**（引用可能为 `Null` 值，即不指向任何对象）。

引用类型分为下面几种：

- `null`
- 类、`String`（特殊的类、不可改变）
- 接口
- 数组

后面会一一讲到这些引用类型。



### 三、数据类型转换

`Java` 中，类型转换有两种方式：

- 自动转换
- 强制转换

#### 1、自动转换

自动转换是程序在执行过程中“悄然”进行的转换，不需要用户提前声明。

一般有下面几种情况， `Java` 将会自动做类型转换：

**1.1、由小数据转换为大数据**

这里所说的**小**和**大**并不是指占用字节的多少，而是表示**值的范围的大小**：`(byte，short，char)` < `int` < `long` < `float` < `double`。

小数据类型的数值表示范围小于大数据类型的数值表示范围，即精度小于大数据类型。

所以，如果大数据向小数据转换，会丢失数据精度。比如：`long` 转为 `int`，则超出 `int` 表示范围的数据将会丢失，导致结果的不确定性。

反之，小数据向大数据转换，则不会存在数据丢失情况。



**1.2、转换前后的数据类型兼容**

由于 `boolean` 类型只能存放 `true` 或 `false`，这与整数或字符是不兼容的，因此不可以做类型转换。



**1.3、整型类型和浮点型进行计算后，结果会转为浮点类型**

如：

```java
public class Test {
  public static void main(String[] args) {
    long x = 30;
    float y = 14.3f;
    System.out.println("x/y = " + x/y); // x/y = 2.097902
  }
}
```

`long` 虽然精度大于 `float` 类型，但是结果为浮点数类型。



#### 2、强制转换

在不符合自动转换条件或者有需要时，可以对数据类型做强制的转换。强制类型转换必须在代码中声明。

**强制转换使用括号 `()` ：**

```java
public class Test {
  public static void main(String[] args) {
    float f = 25.5f;
    int x = (int)f;
    System.out.println("x = " + x); // x = 25
  }
}
```



#### 3、引用类型之间的转换

引用类型之间的转换有个先提条件：两个类型之间必须**具有继承关系**；也就是说一个类型必须是另一个类型的子类类型。不具备继承关系的两个引用类型变量是不能进行强制类型转换的。

举个例子，假设系统中存在 `Father`、`Son` 两个对象。

**3.1、向上转型**

把子类对象直接赋给父类引用叫向上转型（`upcasting`），向上转型是自动的，不用强制转型。

```java
Father father = new Son();  
```

在这里 `Son` 对象实例被向上转型为 `father` 了。但是需要注意：这个 `Son` 对象实例（`father`）在内存中的本质还是 `Son` 类型。



**3.2、向下转型 — 引用类型的强制转换**

把**指向子类对象的父类引用**赋给子类引用叫向下转型（`downcasting`），要使用强制转型。**也就是说要向下转型，必须先向上转型**（本身是子类才能从父类引用转为子类，直接父类转子类不行）。

```java
Father father = new Son(); 
Son son =(Son) father;
```

父类引用并不是指向子类对象时，就会报错：

```java
Father father = new Father(); 
Son son =(Son) father; // Error: Father cannot be cast to Son
```

因此，为了安全，可以用 `instanceof` 先做一个判断。

**完整示例代码：**

```java
public class Father {
  public static void main(String[] args) {
    // 测试向上造型
    Father father = new Son();
    System.out.println(father instanceof Son); // true
    father.test();  // Father的test方法
    father.test1(); // Son的test1方法
    //father.test2(); //编译出错

    // 测试强制类型转化
    Son son = (Son) father;
    son.test();  // Father的test方法
    son.test2();  // Son的test2方法

    // 测试错误的强制类型转化
    Father father2 = new Father();
    son = (Son) father2;
    /*
     * 上面这条编译不报错，但是运行异常，下面是错误报告
     * Exception in thread "main" java.lang.ClassCastException:
     * test.Father cannot be cast to test.Son
     */
  }

  public void test() {
    System.out.println("Father的test方法");
  }

  public void test1() {
    System.out.println("Father的test1方法");
  }
}

class Son extends Father {
  public Son() {
  }

  public void test1() {
    System.out.println("Son的test1方法");
  }

  public void test2() {
    System.out.println("Son的test2方法");
  }
}
```



### 四、变量

在 `Java` 中， 每个变量都有一个类型(`type`)。在使用变量之前，要声明变量的类型，变量的类型位于变量名之前。 并且不像 `JavaScript` 里面存在变量提升。

```java
public class Test {
  public static void main(String[] args) {
    System.out.println("Declare in the middle:");
    int a;
    a = 5;
    System.out.println(a);  // print an integer

    boolean done = true;
    System.out.println(done); // true

    b = 3.14; // error: cannot find symbol
    double b;
  }
}
```



### 五、内置包装类、装箱和拆箱

#### 1、内置包装类

在 `Java` 中，一切皆对象。但是从数据类型的划分中，我们知道 `Java` 中的数据类型分为基本数据类型和引用数据类型，那基本数据类型怎么能够称为对象呢？于是 `Java` 为每种基本数据类型分别设计了对应的类，能将基本类型视为对象处理，这些类称之为**包装类**（`Wrapper Classes`），也有地方称为外覆类或数据类型类。如 `int` 型数值的包装类 `Integer`，`boolean` 型数值的包装类 `Boolean` 等。

> 虽然 `Java` 可以直接处理基本类型，但是在有些情况下需要将其作为对象来处理，这时就需要将其转换为包装类。

<p style="text-align: center;">基本数据类型及对应的包装类：</p>

| 序号 | 基本数据类型 | 包装类    |
| ---- | ------------ | --------- |
| 1    | byte         | Byte      |
| 2    | short        | Short     |
| 3    | int          | Integer   |
| 4    | long         | Long      |
| 5    | char         | Character |
| 6    | float        | Float     |
| 7    | double       | Double    |
| 8    | boolean      | Boolean   |



#### 2、装箱和拆箱

- 基本数据类型转换为包装类的过程称为**装箱（`Autoboxing`）**，例如把 `int` 包装成 `Integer` 类的对象；
- 包装类变为基本数据类型的过程称为**拆箱（`Unboxing`）**，例如把 `Integer` 类的对象重新简化为 `int`。

`Java 1.5` 版本之后，可以自动拆箱装箱：

```java
public class Demo {
  public static void main(String[] args) {
    int m = 500;
    Integer obj = m;  // 自动装箱
    int n = obj;  // 自动拆箱
    System.out.println("n = " + n); // n = 500

    Integer obj1 = 500;
    System.out.println("obj等价于obj1返回结果为" + obj.equals(obj1)); // true
  }
}
```

也就是在进行基本数据类型和对应的包装类转换时，系统将自动进行装箱及拆箱操作，不用在进行手工操作，为开发者提供了更多的方便。



#### 3、应用

- 实现 `int` 和 `Integer` 的相互转换：

```java
public class Demo {
  public static void main(String[] args) {
    int m = 500;
    Integer obj = m;  // 自动装箱
    int n = obj;  // 自动拆箱
    System.out.println("n = " + n); // n = 500

    Integer obj1 = 500;
    System.out.println("obj等价于obj1返回结果为" + obj.equals(obj1)); // true
  }
}
```

- 将字符串转换为数值类型

```java
public class Demo {
  public static void main(String[] args) {
    String str1 = "30";
    String str2 = "30.3";
    // 将字符串变为int型
    int x = Integer.parseInt(str1);
    // 将字符串变为float型
    float f = Float.parseFloat(str2);
    System.out.println("x = " + x + ", f = " + f); // x = 30, f = 30.3
  }
}
```

- 将整数转换为字符串

```java
public class Demo {
  public static void main(String[] args) {
    int m = 500;
    String s = Integer.toString(m);
    System.out.println("s = " + s); // s = 500
  }
}
```



### 五、常量

定义变量的时候，如果加上 `final` 修饰符，这个变量就变成了常量：

```java
public class Test {
  public static void main(String[] args) {
    final double PI = 3.14; // PI是一个常量
    double r = 5.0;
    double area = PI * r * r;
    PI = 300; // compile error!
  }
}
```



### 六、变量的作用范围

在 `Java` 中，多行语句用 `{ }` 括起来。很多控制语句，例如条件判断和循环，都以 `{ }` 作为它们自身的范围。只要正确地嵌套这些 `{ }`，编译器就能识别出语句块的开始和结束。

而在语句块中定义的变量，它有一个作用域，就是从定义处开始，到语句块结束。超出了作用域引用这些变量，编译器会报错。

```java
public class Test {
  public static void main(String[] args) {
    int i = 0;
    if (2 > 1){ // if开始
      int x = 1;
    }
    System.out.println(i);
    System.out.println(x); // error
  }
}
```

**定义变量时，要遵循作用域最小化原则，尽量将变量定义在尽可能小的作用域。并且，不要重复使用变量名。**



### 七、变量的类型

在  `Java` 中，每个变量都有一个特定的类型，这个类型决定了变量在内存中的存储大小和存储方式，变量的类型不同，操作方式也会不同。

`Java` 中的变量类型有三种：

- 静态（类）变量 `Static/Class variables`
- 成员（实例）变量 `Member/Instance variables`
- 局部变量 `Local variables`

```java
public class Variable {
  static int m = 0;    // 静态变量

  String str = "hello world";  // 成员变量

  public void method() {
    int i = 0;  // 局部变量
  }
}
```



#### 1、局部变量

局部变量在方法、构造函数或块中声明，没有声明提升；并且一旦变量退出方法、构造函数或块，就会被销毁（内存回收），所以它是有作用域的。访问修饰符（后面会讲到）不能用于局部变量。

**这里要注意：局部变量没有默认值，因此声明局部变量时，需要在第一次使用之前分配初始值。**看下面的例子：

```java
public class Variable {
  public static void main(String[] args) {
    Variable var = new Variable();
    var.sayAge();
  }

  public void sayAge() {
    int age = 0;
    age = age + 7;
    System.out.println("My age is : " + age); // My age is : 7
  }
}
```

如果不对局部变量进行初始化，就会编译报错：

```java
public class Variable {
  public static void main(String[] args) {
    Variable var = new Variable();
    var.sayAge();
  }

  public void sayAge() {
    int age;
    age = age + 7; // error: variable age might not have been initialized
    System.out.println("My age is : " + age);
  }
}
```



#### 2、成员变量

- 成员变量随着对象的创建而存在，随着对象的回收而释放；
- 成员变量在类中但在方法、构造函数或任何块之外声明；
- 成员变量可以在使用变量的代码之后声明，并不会报错；
- 成员变量对于类中的所有方法、构造函数和块都是可见的，所以在类中可以直接使用变量名来使用成员变量；
- **成员变量只能被对象调用**；

- 可以为成员变量指定访问修饰符，控制可见性；
- 成员变量具有默认值。 数字类型默认值为 `0`；布尔类型默认值为 `false`；对象引用默认值为 `null`。 

```java
public class Employee {

  // this instance variable is visible for any child class.
  public String name;

  // age variable
  int age;

  // salary variable is visible in Employee class only.
  private double salary;

  // The name variable is assigned in the constructor.
  public Employee(String empName, int empAge) {
    name = empName;
    age = empAge;
  }

  public static void main(String[] args) {
    Employee empOne = new Employee("Deepspace", 23);
    empOne.setSalary(1000);
    empOne.printEmp();
  }

  // The salary variable is assigned a value.
  public void setSalary(double empSal) {
    salary = empSal;
  }

  // This method prints the employee details.
  public void printEmp() {
    System.out.println("name: " + name + ". age: " + age); // name: Deepspace. age: 23
    System.out.println("salary: " + salary); // salary: 1000.0
  }
}
```



#### 3、静态变量

- 声明实例变量时，在前面加上 `static` 关键字，该变量就变成了静态变量；
- 静态变量可以被对象调用，还可以被类名（`ClassName.VariableName`）调用；
- 静态变量随着类的加载而存在，随着类的消失而消失；
- 静态变量的可见性类似于实例变量，也可以指定访问修饰符；
- 静态变量的默认值同实例变量。

```java
public class Employee {

  private static double salary;

  Employee(double mySalary) {
    salary = mySalary;
  }

  public static void main(String[] args) {
    salary = 1000;
    System.out.println(salary); // 1000.0

    Employee epmOne = new Employee(1000.0);

    System.out.println(epmOne.salary); // 1000.0
    System.out.println(Employee.salary); // 1000.0
  }
}
```

那为什么要多出一个 `static` 这个静态修饰符来修饰实例变量让其成为静态变量呢？有什么作用呢？



#### 4、静态变量的作用

我们知道，在程序中，任何变量或者代码都是在编译时是由系统自动分配内存来存储的。而所谓静态就是指在**编译后所分配的内存会一直存在**，直到程序退出内存才会释放这个空间。也就是说只要程序在运行，那么这块内存就会一直被占用着。

在 `Java` 程序里面，如果要使用类的成员，那么普通情况下必须先通过类实例化一个对象，通过对象的引用来访问这些成员；但是用 `static` 修饰的成员是可以通过类名（`ClassName.VariableName`）进行直接访问的，使用起来比成员变量**更加方便**。

> 当然，你也可以通过对象来访问，但是这样就把静态变量的优势给丢弃了，不推荐这样使用。

也就是说，被 `static` 修饰的成员变量（和成员方法）是独立于该类的任何对象的，它不依赖类特定的实例，被类的所有实例共享，只要这个类被加载，`Java` 虚拟机就能根据类名找到他们。对于成员变量，每创建一个实例，就会为实例变量分配一次内存，成员变量可以在内存中有多个拷贝；**而静态变量在内存中只有一个拷贝，节省内存。**

所以，如果类的所有实例都包含一个相同的常量属性，则可以把这个属性定义为静态常量类型，从而节省内存空间。例如，在类中定义一个静态常量 `PI`。
