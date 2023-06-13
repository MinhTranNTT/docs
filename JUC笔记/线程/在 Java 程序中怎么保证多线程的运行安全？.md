# 在 Java 程序中怎么保证多线程的运行安全？

* 方法一：使用安全类，比如 Java. util. concurrent 下的类。
* 方法二：使用自动锁 synchronized。
* 方法三：使用手动锁 Lock。

手动锁 Java 示例代码如下：

```java
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        lock.lock();
        try {
            System.out.println("获得锁");
        } catch (Exception e) {
            // TODO: handle exception
        } finally {
            System.out.println("释放锁");
            lock.unlock();
        }
    }
```

‍
