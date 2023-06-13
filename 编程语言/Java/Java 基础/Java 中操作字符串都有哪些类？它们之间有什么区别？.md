# Java 中操作字符串都有哪些类？它们之间有什么区别？

操作字符串的类有：String、StringBuffer、StringBuilder。

‍

String 和 StringBuffer、StringBuilder 的区别在于 

String 声明的是不可变的对象，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，

而 StringBuffer、StringBuilder 可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的，

但 StringBuilder 的性能却高于 StringBuffer，

所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

‍

‍
