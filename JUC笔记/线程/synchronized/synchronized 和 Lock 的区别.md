# synchronized 和 Lock 的区别

1）Lock 是一个接口；synchronized 是 Java 中的关键字，synchronized 是内置的语言实现；

2）Lock 在发生异常时，如果没有主动通过 unLock() 去释放锁，很可能会造成死锁现象，因此使用 Lock 时需要在 finally 块中释放锁；synchronized 不需要手动获取锁和释放锁，在发生异常时，会自动释放锁，因此不会导致死锁现象发生；

3）Lock 的使用更加灵活，可以有响应中断、有超时时间等；而 synchronized 却不行，使用 synchronized 时，等待的线程会一直等待下去，直到获取到锁；

4）在性能上，随着近些年 synchronized 的不断优化，Lock 和 synchronized 在性能上已经没有很明显的差距了，所以性能不应该成为我们选择两者的主要原因。官方推荐尽量使用 synchronized，除非 synchronized 无法满足需求时，则可以使用 Lock。

synchronized 可以给类、方法、代码块加锁；

lock 只能给代码块加锁。

synchronized 不需要手动获取锁和释放锁，使用简单，发生异常会自动释放锁，不会造成死锁；

lock 需要自己加锁和释放锁，如果使用不当没有 unLock()去释放锁就会造成死锁。通过 Lock 可以知道有没有成功获取锁，而 synchronized 却无法办到。

‍
