# Java语法糖

## 自动装箱与自动拆箱

Java语言有8个基本类型，每个基本类型都有对应的包装（wrapper）类型。

之所以需要包装类型，是因为许多Java核心类库的API都是面向对象的。

## 范型与类型擦除

Java程序里的范型信息，在JVM里全部都丢失了。这么做主要是为了兼容引入范型之前的代码。

当然，并不是每一个范型参数被擦除类型后都会变成Object类。对于限定了继承类的范型参数，经过类型擦出后，所有的范型参数都将变成所限定的继承类。也就是说，Java编译器将选取该范型所能指代的所有类中层次最高的那个，作为替换范型的类。

## 桥接方法

范型的类型擦除带来了不少问题，其中一个便是方法重写。

为了保证编译而成的Java字节码能够保留重写的语义，Java编译器额外添加了一个桥接方法。该桥接方法在字节码层面重写了父类的方法，并将调用子类的方法。

如果子类定义了一个与父类参数类型相同的方法，其返回类型为父类方法返回类型的子类，那么Java编译器也会为其生成桥接方法。

## 其它语法糖

foreach循环

字符串switch

var类型推导