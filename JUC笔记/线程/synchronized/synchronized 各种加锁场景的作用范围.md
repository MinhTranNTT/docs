# synchronized 各种加锁场景的作用范围

1. 作用于非静态方法，锁住的是对象实例（this），每一个对象实例有一个锁。

    ​`public synchronized void method() {}`​

‍

2. 作用于静态方法，锁住的是类的Class对象，因为Class的相关数据存储在永久代元空间，元空间是全局共享的，因此静态方法锁相当于类的一个全局锁，会锁所有调用该方法的线程。

    ​`public static synchronized void method() {}`​

3. 作用于 Lock.class，锁住的是 Lock 的Class对象，也是全局只有一个。

    ​`synchronized (Lock.class) {}`​

‍

4. 作用于 this，锁住的是对象实例，每一个对象实例有一个锁。

    ​`synchronized (this) {}`​

‍

5. 作用于静态成员变量，锁住的是该静态成员变量对象，由于是静态变量，因此全局只有一个。

    ​`public static Object monitor = new Object(); synchronized (monitor) {}`​

​

‍
