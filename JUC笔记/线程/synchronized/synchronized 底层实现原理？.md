# synchronized 底层实现原理？

[(33条消息) synchronized底层实现原理（保证看懂）_负债程序猿的博客-CSDN博客_synchronized底层实现原理](https://blog.csdn.net/qq_33709582/article/details/113572368)

‍

synchronized 是由一对 monitorenter（监视器）/monitorexit（监控退出） 指令实现的

基于对象的监视器（ObjectMonitor），我们在字节码文件里面可以看到，在同步方法执行前后，有两个指令，进入同步方法前monitorenter，方法执行完成后 monitorexit；

我的理解是对象都有一个监视器ObjectMonitor，这个监视器内部有很多属性，比如当前等待线程数、计数器、当前所属线程等；其中计数器属性就是用来记录是否已被线程占有，方法执行到monitorenter时，计数器+1，执行到monitorexit时，计数器-1，线程就是通过这个计数器来判断当前锁对象是否已被占用（0为未占用，此时可以获取锁）；

补充：一个synchronized锁会有两个monitorexit，这是保证synchronized能一定释放锁的机制，一个是方法正常执行完释放，一个是执行过程发生异常时虚拟机释放  

‍
