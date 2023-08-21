# 为什么 Spring 官方不推荐使用 @Autowired 属性注入

[为什么Spring官方不推荐使用@Autowired属性注入_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1FD4y1N7dQ/?spm_id_from=333.999.0.0&vd_source=9bfc54d2ed901f1eab04708cc346c2f5)

[(31条消息) 为什么spring不推荐@Autowired注入，提示：Field injection is not recommended_诺浅的博客-CSDN博客](https://blog.csdn.net/qq32933432/article/details/105785546)

[(31条消息) 为什么 Spring和IDEA 都不推荐使用 @Autowired 注解_autowired不推荐_时间静止不是简史的博客-CSDN博客](https://blog.csdn.net/qq_43371556/article/details/123529701)

[@RequiredArgsConstrutor循环依赖问题 - 码农教程 (manongjc.com)](http://www.manongjc.com/detail/28-xwegbfnayfvskop.html)

---

**Spring 官方不推荐使用 @Autowired 属性注入的原因有：**

1. 强制依赖：对于属性注入，Spring 强制要求容器必须提供一个对象，否则会抛出依赖注入异常。
2. 不可控：属性注入是不可控的，可能会导致 Spring 容器注入不正确的或者不合适的对象，从而导致不可预知的结果。
3. 测试困难：因为属性注入是不可控的，所以在测试时可能会遇到很多困难，导致测试不能正确执行。
4. 可读性差：属性注入的代码可读性较差，可能会导致维护困难，容易出错。

‍

当使用 `@Autowired`​ 属性注入时，idea 提示了一个警告添加了 `Field injection is not recommended`​**(不推荐使用属性注入)**

​`@Autowired`​​ 本质是通过反射类进行的属性注入的，对象创建完成之后 `@Autowired`​​ 才进行属性注入操作

‍

**Java 对象的创建过程**

```java
public class Data {
    private Long gradeClassId;
    private String stuno;
    private String name;
}
```

​`Data data = new Data(1, "admin", "ad");`​ 

Java 对象创建的核心过程如下：

1. 使用 new 关键字来创建对象，并调用类的构造函数，为对象分配内存空间。
2. <u>系统调用类的构造函数，初始化对象的实例变量</u>。**先调用构造函数，在 init() 初始化对象**
3. 对象创建完毕，可以通过引用来调用对象的方法。
4. 对象在不被引用时，可以被垃圾回收器回收，以释放内存空间。

‍

**Spring 官方推荐使用构造器实现属性注入**

```java
@Service
public class ScoresServiceImpl implements IScoresService {

    private final ScoresRepository scoresRepository;

    private final StudentRepository studentRepository;

    public ScoresServiceImpl(ScoresRepository scoresRepository, StudentRepository studentRepository) {
        this.scoresRepository = scoresRepository;
        this.studentRepository = studentRepository;
    }
}
```

> 扩展

使用 `@RequiredArgsConstructor`​

lombok 的注解，通过构造器注入对象，对象必须 final 关键字的属性，才能生成对应的构造器。

final 一旦初始化，值就不可更改

```java
@Service
@RequiredArgsConstructor
public class ScoresServiceImpl implements IScoresService {
    private static final ScoresRepository scoresRepository;	// static 属性不生成构造器
    private final StudentRepository studentRepository;		// @RequiredArgsConstructor 仅仅 final 修饰的属性
}
```

‍
