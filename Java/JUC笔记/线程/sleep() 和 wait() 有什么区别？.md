# sleep() 和 wait() 有什么区别？

来源不同：

sleep() 来自 Thread 类，wait() 来自 Object 类。

‍

对于同步锁的影响不同：

sleep() 不会该表同步锁的行为，如果当前线程持有同步锁，那么 sleep 是不会让线程释放同步锁的。

wait() 会释放同步锁，让其他线程进入 synchronized 代码块执行。

‍

使用范围不同：

sleep() 可以在任何地方使用。

wait() 只能在同步控制方法或者同步控制块里面使用，否则会抛 `IllegalMonitorStateException`​ 异常。

‍

恢复方式不同：

两者会暂停当前线程，但是在恢复上不太一样。

sleep() 在时间到了之后会重新恢复；

wait() 则需要其他线程调用同一对象的 `notify() | nofityAll()`​ 才能重新恢复。

‍
