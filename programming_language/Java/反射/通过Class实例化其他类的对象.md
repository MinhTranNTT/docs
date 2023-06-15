# 通过Class实例化其他类的对象

```java
class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "[" + this.name + "  " + this.age + "]";
    }
}

class hello {
    public static void main(String[] args) throws InstantiationException, IllegalAccessException, ClassNotFoundException {
        Class<?> demo = null;
        demo = Class.forName("Reflect.Person");
        Person per = null;
        per = (Person) demo.newInstance();
        per.setName("Rollen");
        per.setAge(20);
        System.out.println(per);
    }
}
```

> 运行结果：
>
> ```cmd
> [Rollen  20]
> ```

但是注意一下，当我们把 `Person`​​ 中的默认的无参构造函数取消的时候，比如自己定义只定义一个有参数的构造函数之后，会出现错误：

比如我定义了一个构造函数

```java
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
```

然后继续运行上面的程序，会出现：

```cmd
Exception in thread "main" java.lang.InstantiationException: Reflect.Person
        at java.base/java.lang.Class.newInstance(Unknown Source)
        at Reflect.hello.main(he.java:40)
Caused by: java.lang.NoSuchMethodException: Reflect.Person.<init>()
        at java.base/java.lang.Class.getConstructor0(Unknown Source)
        ... 2 more
```

所以大家以后再编写使用Class实例化其他类的对象的时候，一定要自己定义无参的构造函数
