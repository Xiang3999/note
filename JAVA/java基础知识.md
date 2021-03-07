

# JAVA基础

### Java跨平台的原因

- 三种核心机制
  - Java虚拟机(Java Virtual Machine)
  - 代码安全检测(Code Security)
  - 垃圾收集机制(Garbage Collection)

![image-20201014203432469](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201014203432469.png)



![image-20201014204408832](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201014204408832.png)

![image-20201014204421530](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201014204421530.png)

![image-20201014204440123](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201014204440123.png)

![image-20201014204455269](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201014204455269.png)

P 33  3.5 运算符

### 编程规范

JAVA 严格区分大小写

**一个完整的Java。源程序应该包括下列部分：**

-  package语句，该部分至多只有一句，必须放在源程序的第一句。
-  import语句，该部分可以有若干import语句或者没有，必须放在所有的类定义之前。
-  public classDefinition，公共类定义部分，至多只有一个公共类的定义，Java语言规定该Java源程序的文件名必须与该公共类名完全一致。
-  classDefinition，类定义部分，可以有0个或者多个类定义。
- interfaceDefinition，接口定义部分，可以有0个或者多个接口定义。

例如：

```java
package javawork.helloworld;
/*
把编译生成的所有．class文件放到包javawork.helloworld中
package 命名由全部小写的字母组成
*/
import java awt.*;
//告诉编译器本程序中用到系统的AWT包
import javawork.newcentury;
/*告诉编译器本程序中用到用户自定义的包javawork.newcentury*/
 public class HelloWorldApp{...}
/*
公共类HelloWorldApp的定义，名字与文件名相同
class和interface的名字由大写字母开头  鸵鸟命名法
变量的名字用一个小写字母开头，后面的单词用大写字母开头
*/ 
class TheFirstClass{...};
//第一个普通类TheFirstClass的定义 
interface TheFirstInterface{......}
/*定义一个接口TheFirstInterface*/
```

**package语句：**由于Java编译器为每个类生成一个字节码文件，且文件名与类名相同因此同名的类有可能发生冲突。为了解决这一问题，Java提供包来管理类名空间，包实 提供了一种命名机制和可见性限制机制。

### Java 关键字

下面列出了 Java 关键字。这些保留字不能用于常量、变量、和任何标识符的名称。

| 类别                 | 关键字                         | 说明                 |
| :------------------: | :----------------------------: | :------------------: |
| 访问控制(access modifier) | private                        | 私有的               |
|**用于控制程序的其他部分**| protected            | 受保护的                       |
|**对这部分代码的访问级别**| public               | 公共的                         |
|| default              | 默认                           |
| 类、方法和变量修饰符 | abstract                       | 声明抽象             |
|| class                | 类                             |
| |extends              | 扩充,继承                      |
|| final                | 最终值,不可改变的              |
| |implements           | 实现（接口）                   |
| |interface            | 接口                           |
| |native               | 本地，原生方法（非 Java 实现） |
|| new                  | 新,创建                        |
| |static               | 静态                           |
| |strictfp             | 严格,精准                      |
| |synchronized         | 线程,同步                      |
| |transient            | 短暂                           |
| |volatile             | 易失                           |
| 程序控制语句         | break                          | 跳出循环             |
| |case                 | 定义一个值以供 switch 选择     |
| |continue             | 继续                           |
| |default              | 默认                           |
|| do                   | 运行                           |
|| else                 | 否则                           |
|| for                  | 循环                           |
| |if                   | 如果                           |
| |instance of           | 实例                           |
| |return               | 返回                           |
| |switch               | 根据值选择执行                 |
| |while                | 循环                           |
| 错误处理             | assert                         | 断言表达式是否为真   |
| |catch                | 捕捉异常                       |
| |finally              | 有没有异常都执行               |
|| throw                ||
| |throws               | 声明一个异常可能被抛出         |
|| try                  | 捕获异常                       |
|包相关               | import                         | 引入  |
|| package              | 包                             |
| 基本类型             | boolean                        | 布尔型               |
| |byte                 | 字节型                         |
| |char                 | 字符型                         |
| |double               | 双精度浮点                     |
| |float                | 单精度浮点                     |
|| int                  | 整型                           |
| |long                 | 长整型                         |
| |short                | 短整型                         |
| 变量引用             | super                          | 父类,超类            |
|| this                 | 本类                           |
| |void                 | 无返回值                       |
| 保留关键字           | goto                           | 是关键字，但不能使用 |
| |const                | 是关键字，但不能使用           |
| |null                 | 空                             |

JAVA 的类（外部类）有 2 种访问权限: public、default。

而方法和变量有 4 种：public、default、protected、private。

其中默认访问权限和 protected 很相似，有着细微的差别。

-  public 意味着任何地方的其他类都能访问。
-  default 则是同一个包的类可以访问。
-  protected 表示同一个包的类可以访问，其他的包的该类的子类也可以访问。
-  private 表示只有自己类能访问。
- synchronize：
  - synchronize关键字修饰在方法上，在多线程中使用，该方法同一时间只能被一个线程访问，锁就是this

- transient:
  - 修饰在包含定义变量的语句中将不会被序列化存储在硬盘
- volatile
  - 修饰在成员变量上，在多线程中访问该变量，都会重新从线程中获取，使真实数据可见。

**修饰符：abstract、static、final**

-  abstract: 表示是抽象类。 使用对象：类、接口、方法
-  static: 可以当做普通类使用，而不用先实例化一个外部类。（用他修饰后，就成了静态内部类了）。 使用对象：类、变量、方法、初始化函数（注意：修饰类时只能修饰 内部类 ）
-  final: 表示类不可以被继承。 使用对象：类、变量、方法\

**final、static、abstract** 之间不能同时使用的问题：

1、final 不能同时和 abstract 使用，例子：

```
abstract final void m();
```

**原因：**因为 abstract 是需要被子类继承覆盖的，否则毫无意义，而 final 作用是禁止继承的，两者相互排斥，所以不呢能 共用。

2：static 和 abstract 也是不能连用的，例子：

```
abstract static void m(){}
```

**原因：**因为 static 是类级别的不能被子类覆盖，而 abstract 需要被继承实现，两者相互矛盾。

### Java 源程序与编译型运行区别

如下图所示：

![img](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/ZSSDMld.png)

### TIPS

在JAVA中，变量的声明尽量靠近变量第一次用的地方，这是一个良好的程序编写风格。

## 与C++的异同

无直接指针操作
自动内存管理
数据类型长度固定
不用头文件
不包含结构和联合
不支持宏
不用多重继承
无类外全局变量
无Goto

### 类

Java中的所有函数都属于某一个类的方法（标准术语称其为方法，而不是成员函数）

Java中的main方法必须要有一个外壳类，而且main方法一般是public的。

### 数据类型

JAVA中的数据类型有4种整型(int 4 short 2 long  8 byte 1) 2种浮点类型(float 4 double 8) 1种Unicode编码的字符类型char  1种真值boolean类型。其中**JAVA没有无符号(unsigned)类型的int  short long  byte** 

同C++都是强制式语言，每用一个变量都要声明一种类型。

JAVA中int都是4字节，而C++中是随系统变化而变化的，有可能是4字节、2字节、8字节。

整理一下脑子里的知识，也算温故知新吧。

**一、Java四大数据类型分类**

**1、整型** byte 、short 、int 、long

**2、浮点型** float 、 double

**3、字符型** char

**4、布尔型** boolean

**二、八种基本数据类型**

**![img](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/v2-a4de2da2942089375382858919e3ae63_1440w.png)**

**三、数据类型详细介绍**

**整型（byte、short、int、long）**

虽然byte、short、int、long 数据类型都是表示整数的，但是它们的取值范围可不一样。

byte 的取值范围：-128～127（-2的7次方到2的7次方-1）

short 的取值范围：-32768～32767（-2的15次方到2的15次方-1）

int 的取值范围：-2147483648～2147483647（-2的31次方到2的31次方-1）

long 的取值范围：-9223372036854774808～9223372036854774807（-2的63次方到2的63次方-1）

由上可以看出 byte、short 的取值范围比较小，而long的取值范围时最大的，所以占用的空间也是最多的。int 取值范围基本上可以满足我们的日常计算需求了，所以 int 也是我们使用的最多的一个整型类型。

**浮点型（float、double）**

float 和 double 都是表示浮点型的数据类型，它们之间的区别在于精确度的不同。

float（单精度浮点型）取值范围：3.402823e+38～1.401298e-45（e+38 表示乘以10的38次方，而e-45 表示乘以10的负45次方）

double（双精度浮点型）取值范围：1.797693e+308～4.9000000e-324（同上）

double 类型比float 类型存储范围更大，精度更高。

通常的浮点型数据在不声明的情况下都是double型的，如果要表示一个数据时float 型的，可以在数据后面加上 "F" 。

浮点型的数据是不能完全精确的，有时候在计算时可能出现小数点最后几位出现浮动，这时正常的。

**字符型（char）**

char 有以下的初始化方式：

char ch = 'a'; // 可以是汉字，因为是Unicode编码

char ch = 1010; // 可以是十进制数、八进制数、十六进制数等等。

char ch = '\0'; // 可以用字符编码来初始化，如：'\0' 表示结束符，它的ascll码是0，这句话的意思和 ch = 0 是一个意思。

Java是用unicode 来表示字符，“中” 这个中文字符的unicode 就是两个字节。

String.getBytes(encoding) 方法获取的是指定编码的byte数组表示。

通常gbk / gb2312 是两个字节，utf-8 是3个字节。

如果不指定encoding 则获取系统默认encoding 。

**布尔型（boolean）**

boolean 没有什么好说的，它的取值就两个：true 、false 。

**四、基本类型之间的转换**

将一种类型的值赋值给另一种类型是很常见的。==在Java中，boolean 类型与其他7中类型的数据都不能进行转换==，这一点很明确。但对于其他7种数据类型，它们之间都可以进行转换，只是可能会存在精度损失或其他一些变化。

转换分为自动转换和强制转换：

自动转换（隐式）：无需任何操作。

强制转换（显式）：需使用转换操作符（type）。

将6种数据类型按下面顺序排列一下：

double > float > long > int > short > byte

如果从小转换到大，那么可以直接转换，而从大到小，或char 和其他6种数据类型转换，则必须使用强制转换。

**1、自动转换**

自动转换时发生扩宽（widening conversion）。因为较大的类型（如int）要保存较小的类型（如byte），内存总是足够的，不需要强制转换。如果将字面值保存到byte、short、char、long的时候，也会自动进行类型转换。注意区别，此时从int（没有带L的整型字面值为int）到byte/short/char也是自动完成的，虽然它们都比int小。在自动类型转化中，除了以下几种情况可能会导致精度损失以外，其他的转换都不能出现精度损失。

》int--> float

》long--> float

》long--> double

》float -->double without strictfp

除了可能的精度损失外，自动转换不会出现任何运行时（run-time）异常。

**2、强制类型转换**

如果要把大的转成小的，或者在short与char之间进行转换，就必须强制转换，也被称作缩小转换（narrowing conversion）,因为必须显式地使数值更小以适应目标类型。强制转换采用转换操作符（）。严格地说，将byte转为char不属于narrowing conversion），因为从byte到char的过程其实是byte-->int-->char，所以widening和narrowing都有。强制转换除了可能的精度损失外，还可能使模（overall magnitude）发生变化。强制转换格式如下：

```java
(target-type) value;
```

如果整数的值超出了byte所能表示的范围，结果将对byte类型的范围取余数。例如a=256超出了byte的[-128,127]的范围，所以将257除以byte的范围（256）取余数得到b=1；需要注意的是，当a=200时，此时除了256取余数应该为-56，而不是200。

将浮点类型赋给整数类型的时候，会发生截尾（truncation）。也就是把小数的部分去掉，只留下整数部分。此时如果整数超出目标类型范围，一样将对目标类型的范围取余数。

7种基本类型转换总结如下图：

![img](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/v2-669dda47fc54412fce7a0985bc5272b0_1440w.png)

**3、赋值及表达式中的类型转换**

**3.1、字面值赋值**

在使用字面值对整数赋值的过程中，可以将int literal赋值给byte short char int，只要不超出范围。这个过程中的类型转换时自动完成的，但是如果你试图将long literal赋给byte，即使没有超出范围，也必须进行强制类型转换。例如 byte b = 10L；是错的，要进行强制转换。

**3.2、表达式中的自动类型提升**

除了赋值以外，表达式计算过程中也可能发生一些类型转换。在表达式中，类型提升规则如下：

· 所有byte/short/char都被提升为int。

· 如果有一个操作数为long，整个表达式提升为long。float和double情况也一样。

**拓展知识点：**Java是面向对象语言，其概念为一切皆为对象，但基本数据类型算是个例外哦，基本数据类型大多是面向机器底层的类型，它是 **“值”** 而不是一个对象，它存放于**“栈”**中而不是存放于**“堆”**中，但Java一切皆为对象的概念不是说说而已，它为每一个基本数据类型都做了相应的**包装类**，我们日常使用中大多情况下都会使用着这些包装类：

boolean Boolean
char Character
byte Byte
short Short
int Integer
long Long
float Float
double Double
String（字符串）
包装类就是一个对象，它存放于**“堆”**中。

### 真值

在C++中值０相当于布尔值false,非0值相当于布尔值True。

在Java中则不是这样的。

```
if(x=0)// oops... means x==0
```

在C++中测试可以编译运行，其结果总是false.而在JAVA中，这个测试将不能通过编译，其原因是整数表达式x=0不能转换为布尔值。

### 定义&声明

变量的定义和声明在JAVA中是没有区分的

在C/C++ 中区分这两者:声明是将一个名称引入程序。定义提供了一个实体在程序中的唯一描述，涉及到内存空间的分配以及初始值的设定。声明和定义有时是同时存在的。[C++ 声明与定义的区别](https://blog.csdn.net/cloud323/article/details/75646379#:~:text=%E5%A3%B0%E6%98%8E%E4%BB%85%E4%BB%85%E6%98%AF%E5%B0%86%E4%B8%80%E4%B8%AA,%E5%87%A0%E4%B9%8E%E9%83%BD%E6%98%AF%E7%BC%96%E7%A8%8B%E9%94%99%E8%AF%AF%E3%80%82)

```C
int i=10;//这是变量的定义
extern int i;//这是变量的声明
注意：如果使用extern关键字时，对变量进行了初始化，那就是定义。
extern int b = 20;  //是定义
```

**子类是父类的类型，但父类不是子类的类型。**

**子类的实例可以声明为父类型，但父类的实例不能声明为子类型。**

```JAVA
class Vehicle {}

public class Car extends Vehicle {
    public static void main(String args[]){
        Vehicle v1 = new Vehicle(); //父类型
        Vehicle v2 = new Car(); //子类的实例可以声明为父类型
        Car c1 = new Car();    // 子类型
        Car c2 = new Vehicle(); //这句会报错，父类型的实例不能声明为子类型

        //Car（子类）是Vehicle（父类）类型, Vehicle（父类）不是Car（子类）类型
        boolean result1 =  c1 instanceof Vehicle;    // true
        boolean result2 =  c1 instanceof Car;        // true
        boolean result3 =  v1 instanceof Vehicle;    // true
        boolean result4 =  v1 instanceof Car;          // false
        boolean result5 =  v2 instanceof Vehicle;    // true
        boolean result6 =  v2 instanceof Car;          // true

        System.out.println(result1);
        System.out.println(result2);
        System.out.println(result3);
        System.out.println(result4);
        System.out.println(result5);
        System.out.println(result6);
   }
}
```

从执行结果来看，虽然 v2 被声明为了 Vehicle（父类），但它既是 instanceof Vehicle，又是 instanceof Car，所以 v2 其实是 Car（子类），否则 v2 instanceof Car 应该为 false。

常量的定义：
JAVA      final

C++        const 