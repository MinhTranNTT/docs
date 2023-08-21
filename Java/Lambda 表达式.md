# Lambda 表达式

参考文档：

[Lambda学会这几种即可_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1f34119771/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=9bfc54d2ed901f1eab04708cc346c2f5)

[【IT老齐289】Java语法中的方法引用：：是什么？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1je4y1c79s/?spm_id_from=333.1007.tianma.1-1-1.click&vd_source=9bfc54d2ed901f1eab04708cc346c2f5)

[Java8新特性之二：方法引用 - 无恨之都 - 博客园 (cnblogs.com)](https://www.cnblogs.com/wuhenzhidu/p/10727065.html#:~:text=Java8%E6%96%B0%E7%89%B9%E6%80%A7%E4%B9%8B%E4%BA%8C%EF%BC%9A%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%201%201%E3%80%81%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%E7%9A%84%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF%20%E6%88%91%E4%BB%AC%E7%94%A8Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%9D%A5%E5%AE%9E%E7%8E%B0%E5%8C%BF%E5%90%8D%E6%96%B9%E6%B3%95%E3%80%82%20%E4%BD%86%E6%9C%89%E4%BA%9B%E6%83%85%E5%86%B5%E4%B8%8B%EF%BC%8C%E6%88%91%E4%BB%AC%E7%94%A8Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%BB%85%E4%BB%85%E6%98%AF%E8%B0%83%E7%94%A8%E4%B8%80%E4%BA%9B%E5%B7%B2%E7%BB%8F%E5%AD%98%E5%9C%A8%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%8C%E9%99%A4%E4%BA%86%E8%B0%83%E7%94%A8%E5%8A%A8%E4%BD%9C%E5%A4%96%EF%BC%8C%E6%B2%A1%E6%9C%89%E5%85%B6%E4%BB%96%E4%BB%BB%E4%BD%95%E5%A4%9A%E4%BD%99%E7%9A%84%E5%8A%A8%E4%BD%9C%EF%BC%8C%E5%9C%A8%E8%BF%99%E7%A7%8D%E6%83%85%E5%86%B5%E4%B8%8B%EF%BC%8C%E6%88%91%E4%BB%AC%E5%80%BE%E5%90%91%E4%BA%8E%E9%80%9A%E8%BF%87%E6%96%B9%E6%B3%95%E5%90%8D%E6%9D%A5%E8%B0%83%E7%94%A8%E5%AE%83%EF%BC%8C%E8%80%8CLambda%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%8F%AF%E4%BB%A5%E5%B8%AE%E5%8A%A9%E6%88%91%E4%BB%AC%E5%AE%9E%E7%8E%B0%E8%BF%99%E4%B8%80%E8%A6%81%E6%B1%82%EF%BC%8C%E5%AE%83%E4%BD%BF%E5%BE%97Lambda%E5%9C%A8%E8%B0%83%E7%94%A8%E9%82%A3%E4%BA%9B%E5%B7%B2%E7%BB%8F%E6%8B%A5%E6%9C%89%E6%96%B9%E6%B3%95%E5%90%8D%E7%9A%84%E6%96%B9%E6%B3%95%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9B%B4%E7%AE%80%E6%B4%81%E3%80%81%E6%9B%B4%E5%AE%B9%E6%98%93%E7%90%86%E8%A7%A3%E3%80%82%20%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%E5%8F%AF%E4%BB%A5%E7%90%86%E8%A7%A3%E4%B8%BALambda%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%E5%8F%A6%E5%A4%96%E4%B8%80%E7%A7%8D%E8%A1%A8%E7%8E%B0%E5%BD%A2%E5%BC%8F%E3%80%82%202,2%E3%80%81%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%E7%9A%84%E5%88%86%E7%B1%BB%203%203%E3%80%81%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%E4%B8%BE%E4%BE%8B%203.1%20%E9%9D%99%E6%80%81%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%20%E6%9C%89%E4%B8%80%E4%B8%AAPerson%E7%B1%BB%2C%E5%A6%82%E4%B8%8B%E6%89%80%E7%A4%BA%EF%BC%9A%20)

---

## Lambda

使用场景 Lambda 只能使用在函数式接口，​

**什么是函数式接口：**​<u>就是一个</u>​**<u>有且仅有一个抽象方法</u>**​<u>，但是可以有</u>​***<u>多个非抽象方法的接口</u>***​<u>。函数式接口可以被隐式转换为Lambda表达式</u>。Lambda表达式和方法引用（实际上也可以认为是Lambda表达式）

```java
interface MyInterface {	// 函数式接口： 一个接口里面只有一个方法（并且这个方法是抽象的）
    int show(int i, int k);
}
```



### 旧的接口实现

```java
MyInterface m = new MyInterface() {
    @Override
    public int show(int i, int k) {
        return 0;
    }
};
m.show(1, 1);
```

‍

### Lambda接口实现

```java
MyInterface m = (k, v) -> {
    return k + v;
};
m.show(1, 1);
```

‍

## “::”方法引用  

**方法引用：**方法引用可以理解为Lambda表达式的另外一种表现形式。

案例准备

```java
public class Person {

    private String name;
    private String age;

    public Person() {
    }

    public Person(String name, String age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age='" + age + '\'' +
                '}';
    }

    public static int compare(Person a, Person b) {
        int i = a.getAge().compareTo(b.getAge());
        if (i != 0) {
            return i;
        } else {
            return a.getName().compareTo(b.getName());
        }
    }
    public static int compare(Person a, Person b, Person c) {
        return 0;
    }
	// static 属于 class 不属于 对象

    public Person concat(Person b) {
        this.setName(this.getName() + "," + b.getName());
        System.out.println(this);
        return this;
    }

}
```

### 五种实现方式

#### 实现一：方法引用写法，调用 static 静态方法，参数类型动态推断

```java
@Test
public void test() {

    List<Person> list = new ArrayList<>();
    list.add(new Person("liu", "1"));
    list.add(new Person("zong", "2"));
    list.add(new Person("lin", "3"));

    // 传统写法
    list.sort(new Comparator<Person>() {
        @Override
        public int compare(Person o1, Person o2) {
            return 0;
        }
    });
    // lambda
    list.sort((a, b) -> Person.compare(a, b));

    // 方法引用写法，调用 static 静态方法，参数类型动态推断
    list.sort(Person::compare);
    System.out.println(list);
}
```

#### 实现二：stream 留处理

```java
@Test
public void test1() {
    List<Person> list = new ArrayList<>();
    list.add(new Person("liu", "1"));
    list.add(new Person("zong", "2"));
    list.add(new Person("lin", "3"));
    list.stream().sorted(Person::compare).forEach(person -> System.out.println(person));
    list.stream().sorted(Person::compare).forEach(System.out::println);
}
```

#### 实现三：调用对象方法

```java
@Test
public void test2() {
    List<Person> list = new ArrayList<>();
    list.add(new Person("liu", "1"));
    list.add(new Person("zong", "2"));
    list.add(new Person("lin", "3"));
    Person a = new Person("liuzonglin", "1");
    list.stream().sorted(Person::compare).forEach(a::concat);
}
```

#### 实现四：`:: new 实例化对象`​

```java
@Test
public void test3() {
    // stream 留处理

    List<Person> list = new ArrayList<>();
    list.add(new Person("liu", "1"));
    list.add(new Person("zong", "2"));
    list.add(new Person("lin", "3"));
    Person a = new Person("liuzonglin", "1");
    list.stream().sorted(Person::compare).collect(Collectors.toList());
    list.stream().sorted(Person::compare).collect(Collectors.toCollection(ArrayList::new));

}
```

‍

#### 实现五：代特定实例

```java
@Test
public void test4() {
    String[] strings = {

      "liu", "zong", "lin"
    };
    // 传统写法
    Arrays.sort(strings, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            return 0;
        }
    });
    // lambda
    Arrays.sort(strings, (a, b) -> a.compareToIgnoreCase(b));
    // :: 写法 String指代a
    Arrays.sort(strings, String::compareToIgnoreCase);

}
```
