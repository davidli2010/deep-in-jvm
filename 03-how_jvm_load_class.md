# JVM如何加载Java类？

从class文件到内存中的类，按先后顺序需要经过加载、链接以及初始化三大步骤。其中，链接过程同样需要验证；而内存中的类没有经过初始化，同样不能使用。

Java语言中的类型可以分为两大类：基本类型和引用类型。

Java的基本类型是由JVM预先定义好的。至于引用类型，Java将其细分为四种：类、接口、数组类和范型参数。由于范型参数会在编译过程中被擦除，因此JVM中实际上只有前三种。在类、接口和数组类中，数组类是由JVM直接生成的，其它两种则有对应的字节流。

字节流最常见的形式是由Java编译器生成的class文件。除此之外，也可以在程序内部直接生成，或者从网络中获取字节流。这些不同形式的字节流，都会被加载到JVM中，成为类或接口。

## 加载

加载，是指查找字节流，并且据此创建类的过程。

启动类加载器（boot class loader）是由C++实现的，没有对应的Java对象，因此在Java中只能用null指代。

除了启动类加载器之外，其它的类加载器都是java.lang.ClassLoader的子类，因此有对应的Java对象。这些类加载器需要先由另一个类加载器，比如说启动类加载器，加载至JVM中，方能执行类加载。

双亲委派模型：每当一个类加载器接收到加载请求时，它会先将请求转发给父类加载器。在父类加载器没有找到所请求的类的情况下，该类加载器才会尝试去加载。

除了由Java核心类库提供的类加载器外，我们还可以加入自定义的类加载器，来实现特殊的加载方式。举例来说，我们可以对class文件进行加密，加载时再利用自定义的类加载器对其解密。

在JVM中，类的唯一性是由类加载器实例以及类的全名一同确定的。即便是同一串字节流，经由不同的类加载器加载，也会得到两个不同的类。

## 链接

链接，是指将创建成的类合并至JVM中，使之能够执行的过程。它可分为验证、准备以及解析三个阶段。其中，解析阶段为非必须的。

## 初始化

直接赋值操作，以及所有静态代码块中的代码，会被Java编译器置于同一方法中，并把它命名为\<clinit\>。

类加载的最后一步是初始化，便是为标记为常量值的字段赋值，以及执行\<clinit\>方法的过程。JVM会通过加锁来确保类的\<clinit\>方法仅被执行一次。
