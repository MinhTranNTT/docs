# 在 hibernate 中 getCurrentSession 和 openSession 的区别是什么？

* getCurrentSession 会绑定当前线程，而 openSession 则不会。
* getCurrentSession 事务是 Spring 控制的，并且不需要手动关闭，而 openSession 需要我们自己手动开启和提交事务。

‍
