## 继承

继承的判断：

“IS-A”规则：它表明子类的每个对象也是超类的对象，例如每个经理都是雇员，因此将`Manager`类设计成为	`Employee`的子类是显而易见的。

"is-a”规则的另一种表述法是置换法则。它表明程序中出现超类对象的任何地方都可以用子类对象置换。
例如，可以将一个子类的对象赋给超类变量。

```java
Employee e;
e = new Employee(. . .); //Employee object expected
e = new Manager(. . .);// OK，Manager can be used as wel1
```

![image-20210214165632514](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20210214165632514.png)

在Java程序设计语言中，对象变量是多态的。



在Java中，子类的构造过程中，必须调用其父类的构造函数，是因为有继承关系存在时，子类要把父类的内容继承下来，通过什么手段做到的？  

答案如下：  

　　　　当你new一个子类对象的时候，必须首先要new一个父类的对像出来，这个父类对象位于子类对象的内部，所以说，子类对象比父类对象大，子类对象里面包含了一个父类的对象，这是内存中真实的情况.构造方法是new一个对象的时候，必须要调的方法，这是规定，要new父类对象出来，那么肯定要调用其构造方法，所以：  

　　　　 第一个规则：子类的构造过程中，必须调用其父类的构造方法。一个类，如果我们不写构造方法，那么编译器会帮我们加上一个默认的构造方法，所谓默认的构造方法，就是没有参数的构造方法，但是如果你自己写了构造方法，那么编译器就不会给你添加了，所以有时候当你new一个子类对象的时候，肯定调用了子类的构造方法，但是在子类构造方法中我们并没有显示的调用基类的构造方法，就是没写，如：super(); 并没有这样写，但是这样就会调用父类没有参数的构造方法，如果父类中没有没有参数的构造方法就会出错。  

　　　　 第二个规则：如果子类的构造方法中没有显示的调用基类构造方法，则系统默认调用基类无参数的构造方法注意：如果子类的构造方法中既没有显示的调用基类构造方法，而基类中又没有默认无参的构造方法，则编译出错，所以，通常我们需要显示的：super(参数列表)，来调用父类有参数的构造函数。

```java
//当你没有使用父类默认的构造方法时，此时在子类的构造方法中就需要显示的调用父类定义的构造方法。
class Animal{
  private String name;
  
  //如果你定义一个新的构造方法
  public Animal(String name) {
    this.name = name;
  }
}
public Dog extends Animal{
  
  //这时你就要显示的调用父类的构造方法，因为子类默认调用的是父类的
  //无参构造方法Animal()
  public Dog(){
    super("小狗");  //显示调用父类的有参构造方法

    ....  //子类的构造方法处理
  }
}
//当然，如果你在父类里面把无参的构造方法，显示的写出来了，比如：
class Animal{
  private String name;

  //无参的构造方法
  public Animal() {
    .....  //处理
  }
  /*
  如果你定义一个新的构造方法,那么在子类的构造方法中，就可以不用显示的调用父类的构造方法，因为子类有个无参的构造方法，
  子类在构造方法中会自动调用父类已经定义的无参构造方法。
  */
  public Animal(String name) {
    this.name = name;
  }
}
```





## 抽象类

![image-20210215163927701](D:\OneDrive_PAO\OneDrive - std.uestc.edu.cn\note\img\image-20210215163927701.png)

抽象方法

```java
public  abstract String getDescription()
```

抽象类

```java
public  abstract class Person
{
    private String name;
    public  abstract String getDescription()
}
```

含有抽象方法的类一定是抽象类

抽象类里不一定含有抽象方法

抽象类不能被实列化

```java
new Person("XX")  //是错误的
```

但是可以定义一个抽象类的对象变量，而且只能引用非抽象子类的对象。

```java
Person p=new Studengt("XX");
```

## Object：所有类的超类