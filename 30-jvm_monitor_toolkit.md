# JVM监控及诊断工具

## jps
打印所有正在运行的Java进程的相关信息。

## jstat
打印目标Java进程的性能数据。

## jmap
打印虚拟机堆中的对象信息。

## jinfo
查看目标Java进程的参数，如传递给JVM的-X、-XX参数，以及可在Java层面通过System.getProperty获取的-D参数。

## jstack
打印目标进程中各个线程的栈轨迹，以及这些线程所持有的锁。

## jcmd
可以直接使用jcmd命令来替代前面除了jstat之外的所有命令。
