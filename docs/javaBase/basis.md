#### 1.如下代码
```java
 public class Alpha{
         public void foo(){
                System.out.print("Afoo ");
           }
｝
     public class Beta extends Alpha{
          public void foo() {
                 System.out.print("Bfoo" );
           }
    public static void main(String[] args){
           Alpha a = new Beta();
           Beta b=(Beta)a;
           a.foo();		// Bfoo
           b.foo();		// Bfoo
      }
｝
```
考察多态性，编译器认为a是Alpha 类型，b 是Beta 类型，但是虚拟机知道a 和b 的真实类型是Beta，所以调用Beta 类的函数。如果还是不了解，建议大家看看面向对象的多态。

#### 2..下列运算符合法的是（）

```java
A．&& 
B．<>
C．if
D．:=
答案：A
```

解析：
Java 运算符可以分成以下 : 算术运算符，关系运算符，位运算符，逻辑运算符，赋值运算，其他运算符

经常提到的四大类为算数运算符、关系运算符、逻辑运算符、位运算符。

-   算数运算符(9)：+ – * / % ++ —
-   关系运算符(6)：== != > >= < <=
-   逻辑运算符(6)：&& || ! ^ & |
-   位运算符(7)：& | ~ ^ >> << >>>

&&是在编程中使用的一种符号。是逻辑（条件）与。java 中没有<> := 这种运算符，if else不算运算符

#### 3.提供Java存取数据库能力的包是（）

```java
A．java.sql
B．java.awt
C．java.lang
D．java.swing

答案：A
```

解析

-   A存储在数据源（通常是一个关系数据库）中的数据的 API
-   B java.awt是一个软件包，包含用于创建用户界面和绘制图形图像的所有分类
-   C是提供利用 Java 编程语言进行程序设计的基础类
-   D Swing是一个用于开发Java应用程序用户界面的开发工具包

#### 4.下面程序的运行结果是（）

```java
String str1 = “banalan”;
String str2 = “bana” + new String(“lan”);
System.err.println(str1 == str2);

A：true
B：false
C：不确定

答案：B
```

解析

-   对于字符串，==比较引用对象的内存地址，equals比较的是字符串的内容
-   String str1 = “banalan”; //一个对象
-   String str2 = “bana” + new String(“lan”); //new了一个对象，总的就是2个对象 
-   System.err.println(str1 == str2); // ==是比较引用，两个对象的地址能一样么？只有重写equals比较内容（值）才能为TRUE

#### 5.String能被继承吗？为什么？
不可以，因为String类有final修饰符，而final修饰的类是不能被继承的，实现细节不允许改变。平常我们定义的String str=”a”;其实和String str=new String(“a”)还是有差异的。

**前者默认调用的是String.valueOf来返回String实例对象**，至于调用哪个则取决于你的赋值，比如String num=1,调用的是

```java
public static String valueOf(int i) { 
	return Integer.toString(i); 
}
```
**后者是调用相应构造器**，则是调用如下部分：
```java
public String(String original) { 
	this.value = original.value; 
	this.hash = original.hash; 
} 
```
最后我们的变量都存储在一个char数组中 `private final char value[];` 

#### 6.String, Stringbuffer, StringBuilder 的区别?
**String 字符串常量(final修饰，不可被继承)，String是常量，当创建之后即不能更改**。(可以通过StringBuffer和StringBuilder创建String对象(常用的两个字符串操作类)。)

**StringBuffer 字符串变量（线程安全）**,其也是final类别的，不允许被继承，其中的绝大多数方法都进行了同步处理，包括常用的Append方法也做了同步处理(synchronized修饰)。其自jdk1.0起就已经出现。其**toString方法会进行对象缓存，以减少元素复制开销**。

```java
public synchronized String toString() { 
if (toStringCache == null) { 
toStringCache = Arrays.copyOfRange(value, 0, count); 
} 
return new String(toStringCache, true); 
}
```
**StringBuilder 字符串变量（非线程安全）**其自jdk1.5起开始出现。与StringBuffer一样都继承和实现了同样的接口和类，方法除了没使用synch修饰以外基本一致，不同之处在于最后**toString的时候，会直接返回一个新对象**。
```java
public String toString() { 
// Create a copy, don’t share the array 
return new String(value, 0, count); 
}
```

总结：

-   相同点：
    -   都是final类，都不能被继承；
    -   都是采用的char[]存储。
-   异同点：
    -   String是不可变的字符序列(char[] 是final修饰的)，每次对String对象改变，都会返回一个新的String对象；StringBuffer、StringBuilder是可变的字符序列(char[]没有final修饰)；
    -   StringBuffer是线程安全的，效率低；StringBuilder是线程不安全的，效率高；StringBuilder》StringBuffer》String。
    -   StringBuffer和StringBuilder扩容都是value长度的2倍+2。


#### 7.代码如下
```java
public class Test {
    public final String a;
        public Test() {
            a = "10";
        }
    public void setA1(){
               a = "20";
        }
     public static void main(String[] args) {
            Test t = new Test();
            t.setA1();
            System.out.println(t.a);
        }
}
```
上述代码会编译错误。 final修饰**成员变量**必须在声明时或构造器或代码块中显示赋值，然后才能使用，且只能赋值一次。

#### 8.访问修饰符
对象对**该类成员**的访问权限：

-   public 的作用域有当前类，同一个package，子孙类和其他的package
-   protected 的作用域有当前类，同一个package，和子孙类
-   default 的作用域有当前类和同一个package
-   private 的作用域只有当前类

对于**class的权限修饰**只可以用public和default(缺省)。 

**关键字：**

-   static 静态成员变量(在定义这个变量的类的实例中,这个变量是唯一的,即多个实例间这个变量为共享的)
-   final 成员常量(必须在定义时或在构造方法中赋初值)
-   final static 静态成员常量,必须在定义这个常量时给定其初始化值

#### 9.下面float的赋值语句错误的是？()

```java
A:float f=3.4;
B:float f=3.4F;
C:float f=1.0;
D:float f=1;

答:AC
```

**3.4是双精度数，将双精度型（double）赋值给浮点型（float）属于下转型（down-casting，也称为窄化）**会造成精度损失，因此需要强制类型转换float f =(float)3.4; 或者写成float f =3.4F;

-   float a = 1; 这里的**1是整形，当赋值给一个float时，存在一个隐式转换，不损失精度。**
-   float a = 1f; 1f就是float类型的。

如果你这样定义 float f = 1.0; 肯定会出错。 因为1.0默认是double类型的。double范围比float大。为了不损失精度，不会自动转换。这是必须这样写 float f = 1.0f;

#### 10.JAVA所定义的版本中不包括：（ ）

```java
A、 JAVA2 EE
B、 JAVA2 Card
C、 JAVA2 ME
D、 JAVA2HE
E、 JAVA2 SE

答案：D
```

-   Java企业版：JEE 适用于创建服务器应用程序和服务的Java 平台企业版
-   Java标准版: JSE 适用于桌面系统的Java 平台标准版
-   JME:适用于小型设备和智能卡的Java 平台Micro版
-   Java Card技术主要是让智能卡或与智能卡相近的设备上，以具有安全防护性的方式来运行小型的Java Applet，此技术也被广泛运用在SIM卡、提款卡上。

#### 11.java标识符
-   Java标识符只能由数字、字母、下划线“”或“$”符号以及Unicode字符集组成
-   Java标识符不能以数字开头
-   Java标识符不可以是Java关键字、保留字（const、goto）和字面量（true、false、null）
-   Java标识符区分大小写，是大小写敏感的

#### 12.java中栈和堆得区别？
-   **功能不同**

    栈内存用来存储局部变量和方法调用。
    而堆内存用来存储Java中的对象。无论是成员变量，局部变量，还是类变量，它们指向的对象都存储在堆内存中。

-   **共享性不同**

    栈内存是线程私有的。
    堆内存是所有线程共有的。

-   **异常错误不同**

    如果栈内存或者堆内存不足都会抛出异常。
    栈空间不足：java.lang.StackOverFlowError。
    堆空间不足：java.lang.OutOfMemoryError。

-   **空间大小**

    栈的空间大小远远小于堆的。

#### 13.下列代码
```java
public class Boxer1{
    Integer i;
    int x;
    public Boxer1(int y) {
        x = i+y;    // java.lang.NullPointerException
        System.out.println(x);
    }
    public static void main(String[] args) {
        new Boxer1(new Integer(4));
    }
}
```
NullPointerException在运行时发生： x = i+y;，还没有为i赋值（此时i为null），无法运算

#### 14.为什么构造方法里 this 或者 super 函数调用必须放在第一行且无法共存？
答：super 方法在构造函数的第一行原因是子类有可能访问了父类对象，比如在构造函数中使用父类对象的成员函数和变量，在成员初始化使用了父类，在代码块中使用了父类等，所以放在第一行可以保证在子类可以访问父类对象之前完成对父类对象的初始化。

this 方法在构造函数的第一行原因是为保证父类对象初始化的唯一性，因为假设类 B 是类 A 的子类，如果 this 方法可以在构造函数的任意行使用则首先程序运行到构造函数 B() 的第一行发现没有调用 this() 和 super()，就自动在第一行补齐了 super() 方法（这是 java 默认的机制），以此完成了对父类对象的初始化，然后返回子类的构造函数继续执行，当运行到构造函数 B() 的 this(参数) 调用行时, 调用 B 类对象的另一个构造方法 B(参数)，在 B(参数) 中还会对父类对象再次初始化，这就造成了对资源的浪费，也有可能造成某些意想不到的结果，所以 this 方法不能出现在构造方法除第一行以外的其他行。

这也就解释了为啥在构造方法里面 this 与 super 方法不能同时存在的原因。