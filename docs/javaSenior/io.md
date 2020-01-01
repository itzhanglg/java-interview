#### 1.什么是java 序列化，如何实现java 序列化？
序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决在对对象流进行读写操作时所引发的问题；序列化的实现：将需要被序列化的类实现Serializable 接口，该接口没有需实现的方法，implements Serializable 只是为了标注该对象是可被序列化的，然后使用一个输出流(如FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream 对象的writeObject(Object obj)方法就可以将参数为obj 的对象写出(即保存其状态)，要恢复的话则用输入流。

#### 2.IO中分为哪些流
字节流：InputStream、OutputStream
字符流：Reader、Writer

#### 3.为了向文件hello.txt尾加数据，下列哪个是正确创建指向hello.txt的流？
```java
A. try{ OutputStream out = new FileOutputStream(“hello.txt”); 
} 
catch(IOException e){} 

B. try { OutputStream out = new FileOutputStream(“hello.txt”,true); 
} 
catch(IOException e){} 

C.try{ OutputStream out = new FileOutputStream(“hello.txt”,false); 
} 
catch(IOException e){} 

D.try{ OutputStream out = new OutputStream(“hello.txt”,true); 
} 
catch(IOException e){}
```
答案：B
FileOutputStream类是文件字节输出流，它是OutputStream类的子类，所以FileOutputStream的实例方法都是从OutputStream类继承来的，那么在这道题中需要使用上转型对象才能调用重写父类的方法，排除D。ABC在于FileOutpurStream的构造方法的不同，题目要求向文件尾加数据，A选项是具有刷新功能的构造方法，也就是会覆盖掉原来文件的数据，B选项参数为true表示不会刷新所指向的文件，而是从文件的末尾开始向文件写入数据，取值为false，输出流将刷新所指向的文件。