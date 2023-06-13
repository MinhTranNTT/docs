# 线程池中 submit() 和 execute() 方法有什么区别？

* execute()：只能执行 Runnable 类型的任务。
* submit()：可以执行 Runnable 和 Callable 类型的任务。

Callable 类型的任务可以获取执行的返回值，而 Runnable 执行无返回值。

‍
