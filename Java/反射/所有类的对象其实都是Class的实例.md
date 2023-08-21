# 所有类的对象其实都是Class的实例

```java
package Reflect;

class Demo {
    // other codes...
}

class hello {
    public static void main(String[] args) throws ClassNotFoundException { 
        Class<?> demo1 = null;
        Class<?> demo2 = null;
        Class<?> demo3 = null;
        // 一般尽量采用这种形式
        demo1 = Class.forName("Reflect.Demo");
	// 通过实例化对象，获取到类
        demo2 = new Demo().getClass();
	// 通过类获取
        demo3 = Demo.class;
        System.out.println("demo1 类名称   " + demo1.getName());
        System.out.println("demo2 类名称   " + demo2.getName());
        System.out.println("demo3 类名称   " + demo3.getName());
    }
}
```

> 运行结果：
>
> ```cmd
> demo1 类名称   Reflect.Demo
> demo2 类名称   Reflect.Demo
> demo3 类名称   Reflect.Demo
> ```
