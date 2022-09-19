---
title: 一万字彻底搞懂 Java 继承（三大特征之一）
shortTitle: 一万字彻底搞懂Java继承
description: Java程序员进阶之路，小白的零基础Java教程，认真聊聊 Java的三大特征：继承
category:
  - Java 核心
tag:
  - 面向对象编程
head:
  - - meta
    - name: keywords
      content: Java,Java SE,Java基础,Java教程,Java程序员进阶之路,Java入门,教程,继承,inheritance
---

## 关于继承

在谈 Java 面向对象的时候，不得不提到面向对象的三大特征：封装、**继承**、多态。三大特征紧密联系而又有区别，本篇文章就来带你学习 Java 的**继承**。合理使用继承就能大大减少重复代码，**提高代码复用性。**

![](https://img-blog.csdnimg.cn/20201021205729775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjkzMTcx,size_1,color_FFFFFF,t_70)

## 什么是继承

**继承**（英语：inheritance）是面向对象软件技术中的一个概念。它使得**复用以前的代码非常容易，能够大大提高开发效率。**

Java 语言是非常典型的面向对象的语言，在 Java 语言中**继承就是子类继承父类的属性和方法，使得子类对象（实例）具有父类的属性和方法，或子类从父类继承方法，使得子类具有父类相同的方法**。

我们来举个例子：动物有很多种，是一个比较大的概念。在动物的种类中，我们熟悉的有猫(Cat)、狗(Dog)等动物，它们都有动物的一般特征（比如能够吃东西，能够发出声音），不过又在细节上有区别（不同动物的吃的不同，叫声不一样）。

在 Java 语言中实现 Cat 和 Dog 等类的时候，就需要继承 Animal 这个类。继承之后 Cat、Dog 等具体动物类就是子类，Animal 类就是父类。

![](https://img-blog.csdnimg.cn/img_convert/1a879fea7899bd17dba2d354763308b3.png)

## 为什么需要继承

你可能会疑问**为什么需要继承**？在具体实现的时候，我们创建 Dog，Cat 等类的时候实现其具体的方法不就可以了嘛，实现这个继承似乎使得这个类的结构不那么清晰。

如果仅仅只有两三个类，每个类的属性和方法很有限的情况下确实没必要实现继承，但事情并非如此，事实上一个系统中往往有很多个类并且有着很多相似之处，比如猫和狗同属动物，或者学生和老师同属人。各个类可能又有很多个相同的属性和方法，这样的话如果每个类都重新写不仅代码显得很乱，代码工作量也很大。

这时继承的优势就出来了：可以直接使用父类的属性和方法，自己也可以有自己新的属性和方法满足拓展，父类的方法如果自己有需求更改也可以重写。这样**使用继承不仅大大的减少了代码量，也使得代码结构更加清晰可见**。

![](https://img-blog.csdnimg.cn/img_convert/3e4e6c0f3027547ca2ea8fb919bdddbe.png)

所以这样从代码的层面上来看我们设计这个完整的 Animal 类是这样的：

```java
class Animal
{
    public int id;
    public String name;
    public int age;
    public int weight;

    public Animal(int id, String name, int age, int weight) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.weight = weight;
    }
    //这里省略get set方法
    public void sayHello()
    {
        System.out.println("hello");
    }
    public void eat()
    {
        System.out.println("I'm eating");
    }
    public void sing()
    {
        System.out.println("sing");
    }
}
```

而 Dog，Cat，Chicken 类可以这样设计：

```java
class Dog extends Animal//继承animal
{
    public Dog(int id, String name, int age, int weight) {
        super(id, name, age, weight);//调用父类构造方法
    }
}
class Cat extends Animal{

    public Cat(int id, String name, int age, int weight) {
        super(id, name, age, weight);//调用父类构造方法
    }
}
class Chicken extends Animal{

    public Chicken(int id, String name, int age, int weight) {
        super(id, name, age, weight);//调用父类构造方法
    }
    //鸡下蛋
    public void layEggs()
    {
        System.out.println("我是老母鸡下蛋啦，咯哒咯！咯哒咯！");
    }
}
```

各自的类继承 Animal 后可以直接使用 Animal 类的属性和方法而不需要重复编写，各个类如果有自己的方法也可很容易地拓展。

## 继承的分类

继承分为单继承和多继承，Java 语言只支持类的单继承，但可以通过实现接口的方式达到多继承的目的。**我们先用一张表概述一下两者的区别，然后再展开讲解。**

继承 | 定义 | 优缺点 |
---| ---- | ------ |
单继承![](https://bigsai.oss-cn-shanghai.aliyuncs.com/img/image-20201104084434252.png)|一个子类只拥有一个父类|优点：在类层次结构上比较清晰<br>缺点：结构的丰富度有时不能满足使用需求|
多继承（Java 不支持，但可以用其它方式满足多继承使用需求）![](https://bigsai.oss-cn-shanghai.aliyuncs.com/img/image-20201105202324446.png)|一个子类拥有多个直接的父类|优点：子类的丰富度很高<br>缺点：容易造成混乱|

**单继承**

单继承，一个子类只有一个父类，如我们上面讲过的 Animal 类和它的子类。**单继承在类层次结构上比较清晰，但缺点是结构的丰富度有时不能满足使用需求**。

**多继承**(Java 不支持，但可以通过实现接口来变通)

多继承，一个子类有多个直接的父类。这样做的好处是子类拥有所有父类的特征，**子类的丰富度很高，但是缺点就是容易造成混乱**。下图为一个混乱的例子。

![](https://img-blog.csdnimg.cn/img_convert/debf7ef41c1fe11bb2ae99139160181c.png)

Java 虽然不支持多继承，但是 Java 有三种实现多继承效果的方式，**分别是**内部类、多层继承和实现接口。

[内部类](https://tobebetterjavaer.com/oo/inner-class.html)可以继承一个与外部类无关的类，保证了内部类的独立性，正是基于这一点，可以达到多继承的效果。

**多层继承：**子类继承父类，父类如果还继承其他的类，那么这就叫**多层继承**。这样子类就会拥有所有被继承类的属性和方法。

![](https://img-blog.csdnimg.cn/img_convert/c32c415c362a651bcbba89d677bcbeba.png)

**实现接口**无疑是满足多继承使用需求的最好方式，一个类可以实现多个接口满足自己在丰富性和复杂环境的使用需求。

类和接口相比，**类就是一个实体，有属性和方法，而接口更倾向于一组方法**。举个例子，就拿斗罗大陆的唐三来看，他存在的继承关系可能是这样的：

![](https://img-blog.csdnimg.cn/img_convert/d396cc5578883619b52bf2a52634ec89.png)

## 如何实现继承

实现继承**除了上面用到的 extends**外，还可以用 implements 这个关键字实现。

### **extends 关键字**

在 Java 中，类的继承是单一继承，也就是说一个子类只能拥有一个父类，所以**extends**只能继承一个类。其使用语法为：

```java
class 子类名 extends 父类名{}
```

例如 Dog 类继承 Animal 类，它是这样的：

```java
class Animal{} //定义Animal类
class Dog extends Animal{} //Dog类继承Animal类
```

子类继承父类后，就拥有父类的非私有的**属性和方法**。如果不明白，请看这个案例，在 IDEA 下创建一个项目，创建一个 test 类做测试，分别创建 Animal 类和 Dog 类，Animal 作为父类写一个 sayHello()方法，Dog 类继承 Animal 类之后就可以调用 sayHello()方法。具体代码为：

```java
class Animal {
    public void  sayHello()//父类的方法
    {
        System.out.println("hello,everybody");
    }
}
class Dog extends Animal//继承animal
{ }
public class test {
    public static void main(String[] args) {
       Dog dog=new Dog();
       dog.sayHello();
    }
}
```

点击运行的时候 Dog 子类可以直接使用 Animal 父类的方法。

![](https://img-blog.csdnimg.cn/img_convert/c368a545587640da16fbe8bb164b3586.png)

### **implements 关键字**

使用 implements 关键字可以变相使 Java 拥有多继承的特性，使用范围为类实现接口的情况，一个类可以实现多个接口(接口与接口之间用逗号分开)。

我们来看一个案例，创建一个 test2 类做测试，分别创建 doA 接口和 doB 接口，doA 接口声明 sayHello()方法，doB 接口声明 eat()方法，创建 Cat2 类实现 doA 和 doB 接口，并且在类中需要重写 sayHello()方法和 eat()方法。具体代码为：

```java
interface doA{
     void sayHello();
}
interface doB{
     void eat();
    //以下会报错 接口中的方法不能具体定义只能声明
    //public void eat(){System.out.println("eating");}
}
class Cat2 implements  doA,doB{
    @Override//必须重写接口内的方法
    public void sayHello() {
        System.out.println("hello!");
    }
    @Override
    public void eat() {
        System.out.println("I'm eating");
    }
}
public class test2 {
    public static void main(String[] args) {
        Cat2 cat=new Cat2();
        cat.sayHello();
        cat.eat();
    }
}
```

Cat 类实现 doA 和 doB 接口的时候，需要实现其声明的方法，点击运行结果如下，这就是一个类实现接口的简单案例：

![](https://img-blog.csdnimg.cn/img_convert/e6d364aa0beed9d41e7f05418db80605.png)

## 继承的特点

继承的主要内容就是子类继承父类，并重写父类的方法。使用子类的属性或方法时候，首先要创建一个对象，而对象通过[构造方法](https://tobebetterjavaer.com/oo/construct.html)去创建，在构造方法中我们可能会调用子父类的一些属性和方法，所以就需要提前掌握 [this 和 super关键字](https://tobebetterjavaer.com/oo/this-super.html)。

创建完这个对象之后，再调用**重写**父类后的方法，并[区别重写和重载的区别](https://tobebetterjavaer.com/basic-extra-meal/override-overload.html)。

### this 和 super 关键字

this 和 super 关键字是继承中**非常重要的知识点**，分别表示当前对象的引用和父类对象的引用，两者有很大相似又有一些区别。

**this 表示当前对象，是指向自己的引用。**

```java
this.属性 // 调用成员变量，要区别成员变量和局部变量
this.() // 调用本类的某个方法
this() // 表示调用本类构造方法
```

**super 表示父类对象，是指向父类的引用。**

```java
super.属性 // 表示父类对象中的成员变量
super.方法() // 表示父类对象中定义的方法
super() // 表示调用父类构造方法
```

此外，this 和 super 关键字只能出现在非 static 修饰的代码中。

this()和 super()都只能在**构造方法**的第一行出现，如果使用 this()表示调用当前类的其他构造方法，使用 super()表示调用父类的某个构造方法，所以两者只能根据自己使用需求选择其一。

写一个小案例，创建 D1 类和子类 D2 如下：

```java
class D1{
    public D1() {}//无参构造
    public void sayHello() {
        System.out.println("hello");
    }
}
class D2 extends D1{
    public String name;
    public D2(){
        super();//调用父类构造方法
        this.name="BigSai";//给当前类成员变量赋值
    }
    @Override
    public void sayHello() {
        System.out.println("hello，我是"+this.name);
    }
    public void test()
    {
        super.sayHello();//调用父类方法
        this.sayHello();//调用当前类其他方法
    }
}
public class test8 {
    public static void main(String[] args) {
        D2 d2=new D2();
        d2.test();
    }
}
```

执行的结果为：

![](https://img-blog.csdnimg.cn/img_convert/67bb2b22b915c7196f2b5ac076469f83.png)

### 构造方法

构造方法是一种特殊的方法，**它是一个与类同名的方法**。对象的创建就通过构造方法来完成，其主要的功能是完成对象的初始化。但在继承中**构造方法是一种比较特殊的方法**（比如不能继承），所以要了解和学习在继承中构造方法的规则和要求。

构造方法可分为有参构造和无参构造，这个可以根据自己的使用需求合理设置构造方法。但继承中的构造方法有以下几点需要注意：

**父类的构造方法不能被继承：**

因为构造方法语法是**与类同名**，而继承则不更改方法名，如果子类继承父类的构造方法，那明显与构造方法的语法冲突了。比如 Father 类的构造方法名为 Father()，Son 类如果继承 Father 类的构造方法 Father()，那就和构造方法定义：**构造方法与类同名**冲突了，所以在子类中不能继承父类的构造方法，但子类会调用父类的构造方法。

**子类的构造过程必须调用其父类的构造方法：**

Java 虚拟机**构造子类对象前会先构造父类对象，父类对象构造完成之后再来构造子类特有的属性，**这被称为**内存叠加**。而 Java 虚拟机构造父类对象会执行父类的构造方法，所以子类构造方法必须调用 super()即父类的构造方法。就比如一个简单的继承案例应该这么写：

```java
class A{
    public String name;
    public A() {//无参构造
    }
    public A (String name){//有参构造
    }
}
class B extends A{
    public B() {//无参构造
       super();
    }
    public B(String name) {//有参构造
      //super();
       super(name);
    }
}
```

**如果子类的构造方法中没有显示地调用父类构造方法，则系统默认调用父类无参数的构造方法。**

你可能有时候在写继承的时候子类并没有使用 super()调用，程序依然没问题，其实这样是为了节省代码，系统执行时会自动添加父类的无参构造方式，如果不信的话我们对上面的类稍作修改执行：

![](https://img-blog.csdnimg.cn/img_convert/333b607acf11d2bb85638164ddaa649e.png)

### 方法重写(Override)

方法重写也就是子类中出现和父类中一模一样的方法(包括返回值类型，方法名，参数列表)，它建立在继承的基础上。你可以理解为方法的**外壳不变，但是核心内容重写**。

在这里提供一个简单易懂的方法重写案例：

```java
class E1{
    public void doA(int a){
        System.out.println("这是父类的方法");
    }
}
class E2 extends E1{
    @Override
    public void doA(int a) {
        System.out.println("我重写父类方法，这是子类的方法");
    }
}
```

其中@Override 注解显示声明该方法为注解方法，可以帮你检查重写方法的语法正确性，当然如果不加也是可以的，但建议加上。

对于重写，你需要注意以下几点：

从重写的要求上看：

- 重写的方法和父类的要一致(包括返回值类型、方法名、参数列表)
- 方法重写只存在于子类和父类之间，同一个类中只能重载

从访问权限上看：

- 子类方法不能缩小父类方法的访问权限
- 子类方法不能抛出比父类方法更多的异常
- 父类的私有方法不能被子类重写

从静态和非静态上看：

- 父类的静态方法不能被子类重写为非静态方法
- 子类可以定义于父类的静态方法同名的静态方法，以便在子类中隐藏父类的静态方法（满足重写约束）
- 父类的非静态方法不能被子类重写为静态方法

从抽象和非抽象来看：

- 父类的抽象方法可以被子类通过两种途径重写（即实现和重写）
- 父类的非抽象方法可以被重写为抽象方法

当然，这些规则可能涉及一些修饰符，在第三关中会详细介绍。

### 方法重载(Overload)

如果有两个方法的**方法名相同**，但参数不一致，那么可以说一个方法是另一个方法的重载。方法重载规则如下：

- 被重载的方法**必须改变参数列表**(参数个数或类型或顺序不一样)
- 被重载的方法可以改变返回类型
- 被重载的方法可以改变访问修饰符
- 被重载的方法可以声明新的或更广的检查异常
- 方法能够在同一个类中或者在一个子类中被重载
- 无法以返回值类型作为重载函数的区分标准

重载可以通常理解为完成同一个事情的方法名相同，但是参数列表不同其他条件也可能不同。一个简单的方法重载的例子，类 E3 中的 add()方法就是一个重载方法。

```java
class E3{
    public int add(int a,int b){
        return a+b;
    }
    public double add(double a,double b) {
        return a+b;
    }
    public int add(int a,int b,int c) {
        return a+b+c;
    }
}
```

**方法重写和方法重载的区别**：

方法重写和方法重载名称上容易混淆，但内容上有很大区别，下面用一个表格列出其中区别：

| 区别点     | 方法重写                                           | 方法重载                     |
| ---------- | -------------------------------------------------- | ---------------------------- |
| 结构上     | 垂直结构，是一种父子类之间的关系                   | 水平结构，是一种同类之间关系 |
| 参数列表   | 不可以修改                                         | 可以修改                     |
| 访问修饰符 | 子类的访问修饰符范围必须大于等于父类访问修饰符范围 | 可以修改                     |
| 抛出异常   | 子类方法异常必须是父类方法异常或父类方法异常子异常 | 可以修改                     |

## 继承与修饰符

Java 修饰符的作用就是对类或类成员进行修饰或限制，每个修饰符都有自己的作用，而在继承中可能有些特殊修饰符使得被修饰的属性或方法不能被继承，或者继承需要一些其他的条件。

Java 语言提供了很多修饰符，修饰符用来定义类、方法或者变量，通常放在语句的最前端。主要分为以下两类：

- [访问权限修饰符](https://tobebetterjavaer.com/oo/access-control.html)，也就是 public、private、protected 等
- 非访问修饰符，也就是 static、final、abstract 等


Java 子类重写继承的方法时，**不可以降低方法的访问权限**，**子类继承父类的访问修饰符作用域不能比父类小**，也就是更加开放，假如父类是 protected 修饰的，其子类只能是 protected 或者 public，绝对不能是 default(默认的访问范围)或者 private。所以在继承中需要重写的方法不能使用 private 修饰词修饰。

如果还是不太清楚可以看几个小案例就很容易搞懂，写一个 A1 类中用四种修饰词实现四个方法，用子类 A2 继承 A1，重写 A1 方法时候你就会发现父类私有方法不能重写，非私有方法重写使用的修饰符作用域不能变小(大于等于)。

![](https://img-blog.csdnimg.cn/img_convert/fb9996503adc1998c5ec948746eaa9bb.png)

正确的案例应该为：

```java
class A1 {
    private void doA(){ }
    void doB(){}//default
    protected void doC(){}
    public void doD(){}
}
class A2 extends A1{

    @Override
    public void doB() { }//继承子类重写的方法访问修饰符权限可扩大

    @Override
    protected void doC() { }//继承子类重写的方法访问修饰符权限可和父类一致

    @Override
    public void doD() { }//不可用protected或者default修饰
}
```

还要注意的是，**继承当中子类抛出的异常必须是父类抛出的异常或父类抛出异常的子异常**。下面的一个案例四种方法测试可以发现子类方法的异常不可大于父类对应方法抛出异常的范围。

![](https://img-blog.csdnimg.cn/img_convert/fd468a60a317caa04a9f370930b4f1ac.png)

正确的案例应该为：

```java
class B1{
    public void doA() throws Exception{}
    public void doB() throws Exception{}
    public void doC() throws IOException{}
    public void doD() throws IOException{}
}
class B2 extends B1{
    //异常范围和父类可以一致
    @Override
    public void doA() throws Exception { }
    //异常范围可以比父类更小
    @Override
    public void doB() throws IOException { }
    //异常范围 不可以比父类范围更大
    @Override
    public void doC() throws IOException { }//不可抛出Exception等比IOException更大的异常
    @Override
    public void doD() throws IOException { }
}
```

### 非访问修饰符

访问修饰符用来控制访问权限，而非访问修饰符每个都有各自的作用，下面针对 static、final、abstract 修饰符进行介绍。

[static 修饰符](https://tobebetterjavaer.com/oo/static.html)

static 翻译为“静态的”，能够与变量，方法和类一起使用，**称为静态变量，静态方法(也称为类变量、类方法)**。如果在一个类中使用 static 修饰变量或者方法的话，它们**可以直接通过类访问，不需要创建一个类的对象来访问成员。**

我们在设计类的时候可能会使用静态方法，有很多工具类比如`Math`，`Arrays`等类里面就写了很多静态方法。static 修饰符的规则很多，这里仅仅介绍和 Java 继承相关用法的规则：

- 构造方法不允许声明为 static 的。
- 静态方法中不存在当前对象，因而不能使用 this，当然也不能使用 super。
- 静态方法不能被非静态方法重写(覆盖)
- 静态方法能被静态方法重写(覆盖)

可以看以下的案例证明上述规则：

![](https://img-blog.csdnimg.cn/img_convert/03034ad1d92e441220f92a34c061796f.png)

源代码为：

```java
class C1{
    public  int a;
    public C1(){}
   // public static C1(){}// 构造方法不允许被声明为static
    public static void doA() {}
    public static void doB() {}
}
class C2 extends C1{
    public static  void doC()//静态方法中不存在当前对象，因而不能使用this和super。
    {
        //System.out.println(super.a);
    }
    public static void doA(){}//静态方法能被静态方法重写
   // public void doB(){}//静态方法不能被非静态方法重写
}
```

[final 修饰符](https://tobebetterjavaer.com/oo/final.html)

final 变量：

- final 表示"最后的、最终的"含义，**变量一旦赋值后，不能被重新赋值**。被 final 修饰的实例变量必须显式指定初始值(即不能只声明)。final 修饰符通常和 static 修饰符一起使用来创建类常量。

final 方法：

- **父类中的 final 方法可以被子类继承，但是不能被子类重写**。声明 final 方法的主要目的是防止该方法的内容被修改。

final 类：

- **final 类不能被继承**，没有类能够继承 final 类的任何特性。

所以无论是变量、方法还是类被 final 修饰之后，都有代表最终、最后的意思。内容无法被修改。

[abstract 修饰符](https://tobebetterjavaer.com/oo/abstract.html)

abstract 英文名为“抽象的”，主要用来修饰类和方法，称为抽象类和抽象方法。

**抽象方法**：有很多不同类的方法是相似的，但是具体内容又不太一样，所以我们只能抽取他的声明，没有具体的方法体，即抽象方法可以表达概念但无法具体实现。

**抽象类**：**有抽象方法的类必须是抽象类**，抽象类可以表达概念但是无法构造实体的类。

抽象类和抽象方法内容和规则比较多。这里只提及一些和继承有关的用法和规则：

- 抽象类也是类，如果一个类继承于抽象类，就不能继承于其他的（类或抽象类）
- 子类可以继承于抽象类，但是一定要实现父类们所有 abstract 的方法。如果不能完全实现，那么子类也必须被定义为抽象类
- 只有实现父类的所有抽象方法，才能是完整类。

![](https://img-blog.csdnimg.cn/img_convert/59544079331d3db7deeadada34606aa7.png)

比如我们可以这样设计一个 People 抽象类以及一个抽象方法，在子类中具体完成：

```java
abstract class People{
    public abstract void sayHello();//抽象方法
}
class Chinese extends People{
    @Override
    public void sayHello() {//实现抽象方法
        System.out.println("你好");
    }
}
class Japanese extends People{
    @Override
    public void sayHello() {//实现抽象方法
        System.out.println("口你七哇");
    }
}
class American extends People{
    @Override
    public void sayHello() {//实现抽象方法
        System.out.println("hello");
    }
}
```

## Object 类和转型

提到 Java 继承，不得不提及所有类的根类：Object(java.lang.Object)类，如果一个类没有显式声明它的父类（即没有写 extends xx），那么默认这个类的父类就是 Object 类，任何类都可以使用 Object 类的方法，创建的类也可和 Object 进行向上、向下转型，所以 Object 类是掌握和理解继承所必须的知识点。

Java 向上和向下转型在 Java 中运用很多，也是建立在继承的基础上，所以 Java 转型也是掌握和理解继承所必须的知识点。

### Object 类概述

1.  Object 是类层次结构的**根类**，所有的类都隐式的继承自 Object 类。
2.  Java 中，所有的对象都拥有 Object 的默认方法。
3.  Object 类有一个[构造方法](https://tobebetterjavaer.com/oo/construct.html)，并且是**无参构造方法**。

Object 是 Java 所有类的父类，是整个类继承结构的顶端，也是最抽象的一个类。

像 toString()、equals()、hashCode()、wait()、notify()、getClass()等都是 Object 的方法。你以后可能会经常碰到，但其中遇到更多的就是 toString()方法和 equals()方法，我们经常需要重写这两种方法满足我们的使用需求。

toString()方法表示返回该对象的字符串，由于各个对象构造不同所以需要重写，如果不重写的话默认返回`类名@hashCode`格式。

**如果重写 toString()方法后**直接调用 toString()方法就可以返回我们自定义的该类转成字符串类型的内容输出，而不需要每次都手动的拼凑成字符串内容输出，大大简化输出操作。

equals()方法主要比较两个对象是否相等，因为对象的相等不一定非要严格要求两个对象地址上的相同，有时内容上的相同我们就会认为它相等，比如 String 类就重写了euqals()方法，通过[字符串的内容比较是否相等](https://tobebetterjavaer.com/string/equals.html)。

![](https://img-blog.csdnimg.cn/img_convert/4d45537a3e8b8e2b07dca4fa38bee41a.png)

### 向上转型

**向上转型** : 通过子类对象(小范围)实例化父类对象(大范围)，这种属于自动转换。用一张图就能很好地表示向上转型的逻辑：

![](https://img-blog.csdnimg.cn/img_convert/ee9a74056658d94078696ebb54e9c5ef.png)

父类引用变量指向子类对象后，只能使用父类已声明的方法，但方法如果被重写会执行子类的方法，如果方法未被重写那么将执行父类的方法。

### 向下转型

**向下转型** : 通过父类对象(大范围)实例化子类对象(小范围)，在书写上父类对象需要加括号`()`强制转换为子类类型。但父类引用变量实际引用必须是子类对象才能成功转型，这里也用一张图就能很好表示向上转型的逻辑：

![](https://img-blog.csdnimg.cn/img_convert/6e1f2e5b4c3cc5d54793a53207764a8e.png)

子类引用变量指向父类引用变量指向的对象后(一个 Son()对象)，就完成向下转型，就可以调用一些子类特有而父类没有的方法 。

在这里写一个向上转型和向下转型的案例：

```java
Object object=new Integer(666);//向上转型

Integer i=(Integer)object;//向下转型Object->Integer，object的实质还是指向Integer

String str=(String)object;//错误的向下转型，虽然编译器不会报错但是运行会报错
```

## 子父类初始化顺序

在 Java 继承中，父子类初始化先后顺序为：

1.  父类中静态成员变量和静态代码块
2.  子类中静态成员变量和静态代码块
3.  父类中普通成员变量和代码块，父类的构造方法
4.  子类中普通成员变量和代码块，子类的构造方法

总的来说，就是**静态>非静态，父类>子类，非构造方法>构造方法**。同一类别（例如普通变量和普通代码块）成员变量和代码块执行从前到后，需要注意逻辑。

这个也不难理解，静态变量也称类变量，可以看成一个全局变量，静态成员变量和静态代码块在类加载的时候就初始化，而非静态变量和代码块在对象创建的时候初始化。所以静态快于非静态初始化。

而在创建子类对象的时候需要先创建父类对象，所以父类优先于子类。

而在调用构造方法的时候，是对成员变量进行一些初始化操作，所以普通成员变量和代码块优于构造方法执行。

至于更深层次为什么这个顺序，就要更深入了解 JVM 执行流程啦。下面一个测试代码为：

```java
class Father{
    public Father() {
        System.out.println(++b1+"父类构造方法");
    }//父类构造方法 第四
    static int a1=0;//父类static 第一 注意顺序
    static {
        System.out.println(++a1+"父类static");
    }
    int b1=a1;//父类成员变量和代码块 第三
    {
        System.out.println(++b1+"父类代码块");
    }
}
class Son extends Father{
    public Son() {
        System.out.println(++b2+"子类构造方法");
    }//子类构造方法 第六
    static {//子类static第二步
        System.out.println(++a1+"子类static");
    }
    int b2=b1;//子类成员变量和代码块 第五
    {
        System.out.println(++b2 + "子类代码块");
    }
}
public class test9 {
    public static void main(String[] args) {
        Son son=new Son();
    }
}
```

执行结果：

![](https://img-blog.csdnimg.cn/img_convert/249406d862216a710074cb7d71f2e284.png)

## 结语

好啦，本次继承就介绍到这里啦，Java 面向对象三大特征之一继承——优秀的你已经掌握。最后再来看一下 Java 面向对象的三大特性：封装、继承、多态。

封装：是对类的封装，封装是对类的属性和方法进行封装，只对外暴露方法而不暴露具体使用细节，所以我们一般设计类成员变量时候大多设为私有而通过一些 get、set 方法去读写。

继承：子类继承父类，即“子承父业”，子类拥有父类除私有的所有属性和方法，自己还能在此基础上拓展自己新的属性和方法。主要目的是**复用代码**。

**多态**：多态是同一个行为具有多个不同表现形式或形态的能力。即一个父类可能有若干子类，各子类实现父类方法有多种多样，调用父类方法时，父类引用变量指向不同子类实例而执行不同方法，这就是所谓父类方法是多态的。

最后送你一张图捋一捋其中的关系吧。

![](https://img-blog.csdnimg.cn/img_convert/351fae294ba1360b061f25a924307040.png)



> 转载链接：[https://bbs.huaweicloud.com/blogs/271358](https://bbs.huaweicloud.com/blogs/271358)，作者：bigsai，整理：沉默王二


----

最近整理了一份牛逼的学习资料，包括但不限于Java基础部分（JVM、Java集合框架、多线程），还囊括了 **数据库、计算机网络、算法与数据结构、设计模式、框架类Spring、Netty、微服务（Dubbo，消息队列） 网关** 等等等等……详情戳：[可以说是2022年全网最全的学习和找工作的PDF资源了](https://tobebetterjavaer.com/pdf/programmer-111.html)

关注二哥的原创公众号 **沉默王二**，回复**111** 即可免费领取。

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/xingbiaogongzhonghao.png)
