# Java字节码

## 操作数栈

Java字节码是JVM所使用的指令集。因此，它与JVM基于栈的计算模型是密不可分的。

在解释执行过程中，每当为Java方法分配栈帧时，JVM往往需要开辟一块额外的空间作为操作数栈，来存放计算的操作数以及返回结果。

具体来说便是：执行每一条指令之前，JVM要求该指令的操作数已被压入操作数栈中。在执行指令时，JVM会将该指令所需的操作数弹出，并且将指令的结果重新压入栈中。

正常情况下，操作数栈的压入弹出都是一条条指令完成的。唯一的例外情况是在抛异常时，JVM会清除操作数栈上的所有内容，而后将异常实例压入操作数栈上。

## 局部变量区

Java方法栈帧的另外一个重要组成部分则是局部变量区，字节码程序可以将计算结果缓存在局部变量区之中。

实际上，JVM将局部变量区当成一个数组，依次存放this指针（仅非静态方法），所传入的参数，以及字节码中的局部变量。

## Java字节码简介

Java相关指令，包括各类具备高层语义的字节码，即new，instanceof，checkcast，athrow，monitorenter和monitorexit。

此外，该类型的指令还包括字段访问指令，即静态字段访问指令getstatic、putstatic，和实例字段访问指令getfield、putfield。这四条指令均附带用以定位目标字段的信息，但所消耗的操作数栈元素皆不同。

方法调用指令，包括invokestatic，invokespecial，invokevirtual，invokeinterface以及invokedynamic。

数组相关指令，包括新建基本类型数组的newarray，新建引用类型数组的anewarray，生成多维数组的multianewarray，以及求数组长度的arraylength。另外，它还包括数组的加载指令以及存储指令。这些指令是区分类型的。

控制流指令，包括无条件跳转goto，条件跳转指令，tableswitch和lookupswitch（前者针对密集的cases，后者针对稀疏的cases），返回指令，以及被废弃的jsr，ret指令。其中返回指令是区分类型的。
