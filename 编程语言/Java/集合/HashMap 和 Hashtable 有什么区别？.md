# HashMap 和 Hashtable 有什么区别？

1. 存储：HashMap 运行 key 和 value 为 null，而 Hashtable 不允许。
2. 线程安全：Hashtable 是线程安全的，而 HashMap 是非线程安全的。  

    在 Hashtable 的类注释可以看到，Hashtable 是保留类不建议使用，推荐在单线程环境下使用 HashMap 替代，如果需要多线程使用则用 ConcurrentHashMap 替代。

‍
