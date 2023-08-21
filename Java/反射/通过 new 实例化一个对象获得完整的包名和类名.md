# 通过 new 实例化一个对象获得完整的包名和类名

```java
package Reflect;
// 通过一个对象获得完整的包名和类名
class Demo {
    // other codes...
}

class hello {
    public static void main(String[] args) {
        Demo demo = new Demo();
        System.out.println(demo.getClass().getName());
    }
}
```

> 运行结果：
>
> ```cmd
> Reflect.Demo
> ```

‍
