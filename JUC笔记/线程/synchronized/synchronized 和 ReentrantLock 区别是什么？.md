# synchronized 和 ReentrantLock 区别是什么？

  
synchronized 早期的实现比较低效，对比 ReentrantLock，大多数场景性能都相差较大，但是在 Java 6 中对 synchronized 进行了非常多的改进。

主要区别如下：

* ReentrantLock 使用起来比较灵活，但是必须有释放锁的配合动作；
* ReentrantLock 必须手动获取与释放锁，而 synchronized 不需要手动释放和开启锁；
* ReentrantLock 只适用于代码块锁，而 synchronized 可用于修饰方法、代码块等。

‍
