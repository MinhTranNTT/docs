# String s = new String("xyz"); 创建了几个字符串对象

如果**字符串常量池**已经有“xyz”，则是一个；否则会创建两个字符串对象。

当字符创常量池没有 “xyz”，此时会创建如下两个对象：

一个是字符串字面量 "xyz" 所对应的、驻留（intern）在一个全局共享的字符串常量池中的实例，此时该实例也是在堆中，字符串常量池

只放引用。

另一个是通过 new String() 创建并初始化的，内容与"xyz"相同的实例，也是在堆中。

‍
