# JVM如何实现invokedynamic？（下）

## invokedynamic指令

invokedynamic是Java 7引入的一条新指令，用以支持动态语言的方法调用。具体来说，它将调用点（CallSite）抽象成一个Java类，并且将原本由JVM控制的方法调用以及方法链接暴露给了应用程序。在运行过程中，每一条invokedynamic指令将捆绑一个调用点，并且会调用该调用点所链接的方法句柄。

## Java 8的Lambda表达式

在Java 8中，Lambda表达式也是借助invokedynamic来实现的。

具体来说，Java编译器利用invokedynamic指令来生成实现了函数式接口的适配器。这里的函数式接口指的是仅包含一个非default接口方法的接口，一般通过@FunctionalInterface注解。不过就算是没有使用该注解，Java编译器也会将符合条件的接口辨认为函数式接口。

