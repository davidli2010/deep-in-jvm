# JVM如何执行方法调用(上)

## 重载与重写

重载的方法在编译过程中即可完成识别。具体到每一个方法调用，Java编译器会根据所传入参数的声明类型来选取重载方法。选举过程分为3个阶段：

1. 在不考虑基本类型自动装箱拆箱，以及可变长参数的情况下选取重载方法；
2. 第一阶段没有找到匹配方法时，那么在允许自动装箱拆箱，但不允许可变长参数的情况下选取重载方法；
3. 第二阶段没有找到匹配方法时，那么在允许自动装箱拆箱以及可变长参数的情况下选取重载方法。

重写调用会根据调用者的动态类型来选取实际的目标方法。

## JVM的静态绑定和动态绑定

JVM识别方法的关键在于类名、方法名以及方法描述符。方法描述符由方法的参数类型以及返回类型所构成。

由于对重载方法的区分在编译阶段已完成，可以认为JVM不存在重载这一概念。因此，在某些文章中重载也被称为静态绑定，或者编译时多态；而重写则被称为动态绑定。

这个说法在JVM语境中并非完全正确。确切的说，JVM中的静态绑定指的是在解析时便能够直接识别目标方法的情况，而动态绑定则指的是需要在运行过程中根据调用者的动态类型来识别目标方法的情况。

具体来说，Java字节码中与调用有关的指令共有五种。

1. invokestatic：用于调用静态方法。
2. invokespecial：用于调用私有实例方法、构造器，以及使用super关键字调用父类的实例方法或构造器，和所实现接口的默认方法。
3. invokevirtual：用于调用非私有实例方法。
4. invokeinterface：用于调用接口。
5. invokedynamic：用于调用动态方法。

## 调用指令的符号引用

在编译过程中，并不知道目标方法的具体内存地址。因此，Java编译器会暂时用符号引用来表示该目标方法。这一符号引用包括目标方法所在的类或接口的名字，以及目标方法的方法名和方法描述符。

符号引用存储在class文件的常量池之中。根据目标方法是否为接口方法，这些引用可分为接口符号引用和非接口符号引用。
