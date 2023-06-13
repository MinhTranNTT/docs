# spring 自动装配 bean 有哪些方式？

no：默认值，表示没有自动装配，应使用显式 bean 引用进行装配。  
byName：它根据 bean 的名称注入对象依赖项。  
byType：它根据类型注入对象依赖项。  
构造函数：通过构造函数来注入依赖项，需要设置大量的参数。  
autodetect：容器首先通过构造函数使用 autowire 装配，如果不能，则通过 byType 自动装配。  

‍
