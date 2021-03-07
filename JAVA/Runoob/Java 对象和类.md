# Java 对象和类

Java作为一种面向对象语言。支持以下基本概念：

- 多态
- 继承
- 封装
- 抽象
- 类
- 对象
- 实例
- 方法
- 重载

本节我们重点研究对象和类的概念。

- **对象Object**：对象是类的一个实例（**对象不是找个女朋友**），有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
  - 这里理解Object比较好一点，除了翻译为对象还可以翻译为属性。
- **类**：类是一个模板，它描述一类对象的行为和状态。

下图中**汽车**为**类（class）**，而具体的每个人车该类的**对象（object）**，对象包含含来汽车的颜色、品牌、名称等：

![img](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/class-object2020-10-27.png)

```java
public class Dog {
    String breed;//成员变量
    static int a;//类变量
    int size;
    String colour;
    int age;
 
    void eat() {
        int b; //局部变量
    }
 
    void run() {
    }
 
    void sleep(){
    }
 
    void name(){
    }
}
```



一个类可以包含以下类型变量：

- **局部变量**：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
- **成员变量**：==成员变量是定义在类中，方法体之外的变量==。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
- **类变量**：==类变量也声明在类中，方法体之外，但必须声明为 **static** 类型==。

一个类可以拥有多个方法，在上面的例子中：eat()、run()、sleep() 和 name() 都是 Dog 类的方法。

## 构造方法

每个类都有构造方法。如果没有显式地为类定义构造方法，Java 编译器将会为该类提供一个默认构造方法。

在创建一个对象的时候，至少要调用一个构造方法。构造方法的名称必须与类同名，一个类可以有多个构造方法。

下面是一个构造方法示例：

```java
public class Puppy{
    public Puppy(){
    }
 
    public Puppy(String name){
        // 这个构造器仅有一个参数：name
    }
}
```

## 创建对象

对象是根据类创建的。在Java中，使用关键字 new 来创建一个新的对象。创建对象需要以下三步：

- **声明**：声明一个对象，包括对象名称和对象类型。
- **实例化**：使用关键字 new 来创建一个对象。并在堆内存中开辟一块空间。
- **初始化**：使用 new 创建对象时，会调用构造方法初始化对象。

下面是一个创建对象的例子：

```java
public class Puppy{
   public Puppy(String name){
      //这个构造器仅有一个参数：name
      System.out.println("小狗的名字是 : " + name ); 
   }
   public static void main(String[] args){
      // 下面的语句将创建一个Puppy对象
      Puppy myPuppy = new Puppy( "tommy" );
   }
}
/*
* 小狗的名字是 : tommy
*/
```

引用名称是存放对象的地址

引用类型是一个对象类型，它的值是指向内存空间的引用，就是地址，所指向的内存中保存着变量所表示的一个值或一组值。

```java
int a;
a = 250; // 声明变量a的同时，系统给a分配了空间。
```

引用类型就不是了，只给变量分配了引用空间，数据空间没有分配，因为不知道数据是什么。

**错误的例子：**

```java
MyDate today;
today.day = 4; // 发生错误，因为today对象的数据空间未分配。
```

引用类型变量在声明后必须通过实例化开辟数据空间，才能对变量所指向的对象进行访问。

```Java
MyDate today;          //将变量分配一个保存引用的空间
today = new MyDate();     // 这句话是2步，首先执行new MyDate（），给today变量开辟数据空间，然后再执行赋值操作
```

**引用变量赋值：**

```JAVA
MyDate a，b;       // 在内存开辟两个引用空间
a = new MyDate();       // 开辟MyDate对象的数据空间，并把该空间的首地址赋给a
b = a;                   // 将a存储空间中的地址写到b的存储空间中
```



## 访问实例变量和方法

通过已创建的对象来访问成员变量和成员方法，如下所示：

```java
/* 实例化对象 */
Object referenceVariable = new Constructor();
/* 访问类中的变量 */
referenceVariable.variableName;
/* 访问类中的方法 */
referenceVariable.methodName();
/////////////////////////////////////////////////////////
public class Puppy{
   int puppyAge;
   public Puppy(String name){
      // 这个构造器仅有一个参数：name
      System.out.println("小狗的名字是 : " + name ); 
   }
 
   public void setAge( int age ){
       puppyAge = age;
   }
 
   public int getAge( ){
       System.out.println("小狗的年龄为 : " + puppyAge ); 
       return puppyAge;
   }
 
   public static void main(String[] args){
      /* 创建对象 */
      Puppy myPuppy = new Puppy( "tommy" );
      /* 通过方法来设定age */
      myPuppy.setAge( 2 );
      /* 调用另一个方法获取age */
      myPuppy.getAge( );
      /*你也可以像下面这样访问成员变量 */
      System.out.println("变量值 : " + myPuppy.puppyAge ); 
   }
}
```

java因强制要求类名（唯一的public类）和文件名统一，因此在引用其它类时无需显式声明。在编译时，编译器会根据类名去寻找同名文件。

## Package

`package` 的作用就是 c++ 的 `namespace` 的作用，防止名字相同的类产生冲突。Java 编译器在编译时，直接根据 package 指定的信息直接将生成的 class 文件生成到对应目录下。如 **package aaa.bbb.ccc** 编译器就将该 .java 文件下的各个类生成到 **./aaa/bbb/ccc/** 这个目录。

## Import

`import` 是为了简化使用 `package` 之后的实例化的代码。假设 **./aaa/bbb/ccc/** 下的 A 类，假如没有 `import`，实例化A类为：**new aaa.bbb.ccc.A()**，使用 **import aaa.bbb.ccc.A** 后，就可以直接使用 **new A()** 了，也就是编译器匹配并扩展了 **aaa.bbb.ccc.** 这串字符串。

