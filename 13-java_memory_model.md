# Java内存模型

即时编译器的重排序，处理器的乱序执行，以及内存系统的重排序。

即时编译器（和处理器）需要保证能够遵守as-if-serial。通俗地说，就是在单线程情况下，要给程序一个顺序执行的假象。即经过重排序的执行结果要与顺序执行的结果保持一致。

另外，如果两个操作之间存在数据依赖，那么即时编译器（和处理器）不能调整它们的顺序，否则将会造成程序语义的改变。

## Java内存模型与happens-before关系

为了让应用程序能够免于数据竞争的干扰，Java 5引入了明确定义的Java内存模型。其中最为重要的概念便是happens-before关系。happens-before关系是用来描述两个操作的内存可见性的。如果操作X happens-before操作Y，那么X的结果对于Y可见。

在同一个线程中，字节码的先后顺序（program order）也暗含了happens-before关系：在程序控制流路径中靠前的字节码happens-before靠后的字节码。然而，这并不意味着前者一定在后者之前执行。实际上，如果后者没有观测前者的运行结果，即后者没有数据依赖于前者，那么它们可能会被重排序。

除了线程内的 happens-before 关系之外，Java内存模型还定义了下述线程间的happens-before关系。

1. 解锁操作happens-before之后（这里指时钟顺序先后）对同一把锁的加锁操作。
2. volatile字段的写操作happens-before之后（这里指时钟顺序先后）对同一字段的读操作。
3. 线程的启动操作（即Thread.start()）happens-before该线程的第一个操作。
4. 线程的最后一个操作happens-before它的终止事件（即其它线程通过Thread.isAlive()或Thread.join()判断该线程是否终止）。
5. 线程对其它线程的中断操作happens-before被中断线程所收到的中断事件（即被中断线程的InterruptedException异常，或者第三个线程针对被中断线程的Thread.interrupted或者Thread.isInterrupted调用）。
6. 构造器中的最后一个操作happens-before析构器的第一个操作。

happens-before关系还具备传递性。如果操作X happens-before操作Y，而操作Y happens-before操作Z，那么操作X happens-before操作Z。

## Java内存模型的底层实现

Java内存模型是通过内存屏障（memory barrier）来禁止重排序的。

对于即时编译器来说，它会针对前面提到的每一个happens-before关系，向正在编译的目标方法插入相应的读读、读写以及写写内存屏障。

这些内存屏障会限制即时编译器的重排序操作。以volatile字段访问为例，所插入的内存屏障将不允许volatile字段写操作之前的内存访问被重排序至其之后；也将不允许volatile字段读操作之后的内存访问被重排序至其之前。

然后，即时编译器将根据具体的底层体系架构，将这些内存屏障替换成具体的CPU指令。

## 锁，volatile字段，final字段与安全发布

前面提到，锁操作同样具备happens-before关系。具体来说，解锁操作happens-before之后对同一把锁的加锁操作。实际上，在解锁时，JVM同样需要强制刷新缓存，使得当前线程所修改的内存对其它线程可见。

volatile字段可以看成一种轻量级的、不保证原子性的同步，其性能往往优于（至少不亚于）锁操作。然而，频繁地访问volatile字段也会因为不断地强制刷新缓存而严重影响程序的性能。

volatile字段的每次访问均需要直接从内存中读写。

final实例字段则涉及新建对象的发布问题。当一个对象包含final实例字段时，我们希望其它线程只能看到已初始化的final实例字段。因此，即时编译器会在final字段的写操作后插入一个写写屏障，以防止某些优化将新建对象的发布（即将实例对象写入一个共享引用中）重排序至final字段的写操作之前。
