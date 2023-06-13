# synchronized 和 volatile 的区别是什么？

  
volatile 是变量修饰符；synchronized 是修饰类、方法、代码段。  
volatile 仅能实现变量的修改可见性，不能保证原子性；而 synchronized 则可以保证变量的修改可见性和原子性。  
volatile 不会造成线程的阻塞；synchronized 可能会造成线程的阻塞。  

‍
