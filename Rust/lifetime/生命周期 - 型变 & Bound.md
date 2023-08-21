# 生命周期 - 型变 & Bound 

- 型变：里氏替换原则 父类可以出现的地方，字类可以替代
    - `invariant`（不变）
        - 当使用类没有发生变化，则称为invariant(不变)。
    - `covariant`（协变）
        - 当本使用父类的地方，却使用了子类，则称为Covariant(协变)。
    - `contra-variant`（逆变）
        - 当本实用子类的地方，却使用了父类，则称为Contravariant(逆变)。
- 由于 Rust 类型中没有字类概念，让我们看其他语言中的型变示例
    - 示例：[_01_extra_example.scala](./_01_extra_example.scala) 

        协变 示例
        
        ```scala
        object _01_extra_example {
            class A

            class B extends A

            class C extends B

            private def f1(a: A): Unit = {
                println("Test passed")
            }

            def main(args: Array[String]: Unit) {
                val a = new A
                val b = new B

                f1(a) // Test passed
                f1(b) // Test passed
            }
        }
        ```

    - 示例：[_02_extra_example.scala](./_02_extra_example.scala) 

        scala 标识符设置协变、逆变

        ```scala
        object _02_extra_example {
            class A

            class B extends A

            class C extends B

            // class Foo[T]    // 类型构造器
            // 不变: B 继承 A 但是 Foo[A] 和 Foo[B] 是没有关系的
            // Test passed
            // error


            // class Foo[+T]   // Foo[+T] 协变
            // 协变: Foo[A] 和 Foo[B] 是否存在关系，主要是看 A 和 B 是什么关系，所以
            // Test passed
            // Test passed

            class Foo[-T]   // 逆变
            // 逆变: A 是 B 的父类，Foo[A] 和 Foo[B] 的关系就会反过来为，Foo[B] 是 Foo[A] 的父类

            def f1(a: Foo[T]): Unit = {
                println("Test passed")
            }

            def main(args: Array[String]: Unit) {
                
                val a = new Foo[A]
                val b = new Foo[B]

                f1(a)
                f1(b)
            }
        }
        ```

    - 示例：[_03_extra_example.scala](./_03_extra_example.scala) 
        
        函数的入参是逆变 函数出参是协变的

        ```scala
        object _03_extra_example {
            class A

            class B extends A

            class C extends B


            def f(fun: B => B): Unit = {
                fun(new B)
                println("f works")
            }

            def main(args: Array[String]: Unit) {
                val a = new A
                val b = new B
                val c = new C

                f((b: B) => b)  // 不变：B => B

                // 当本实用子类的地方，却使用了父类，则称为Contravariant(逆变)。
                // 函数的入参是逆变 函数出参是协变的
                f((a: A) => c)  // A => C : B => B 

                f((c: C) => c)  // 当本使用父类的地方，却使用了子类，则称为Covariant(协变)。
            }
        }
        ```

    - 为什么重要？
        - 虽然 Rust 类型中没有子类概念，但生命周期有
        - 理解类型的型变后，就能很容易理解生命周期型变
- `T`，`&T`，`&mut T`
    - `T`（`T` 代表所有的类型）
        - `T` 可能代表 拥有 `ownership`（所有权）的类型，可能代表 `&T`，也可能代表 `&mut T`）这三者是一个包含关系。
    - `&T`（引用的类型）
    - `&mut T`（引用可变的类型） 
        - `&T`，`&mut T` 是两个完全不同的类型，不存在交集。
- 生命周期 Bound
    - `'a : 'b`
        - `'a` 是 `'b` 的一个子类，也就是 `'a` 能够替换 `'b`，也就是 `'a` 要比 `'b` 长（或者相等）
    - `'a : 'b` 和 `T: 'a`
        - 如果 `T` 内含引用，那么它这些引用的生命周期一定要比 `'a` 长
    - `&'static T` 与 `T: 'static`
        - `&'static T` 可以理解是一个 `&T`
        - `T: 'static` 这里 `T` 代表所有的类型，如果 `T` 内含引用，那么这个引用的生命周期一定要和 `'static` 一样长或更长
- 生命周期型变，与泛型类似 [rust reference](https://rustwiki.org/zh-CN/reference/subtyping.html) and [rust nomicon](https://nomicon.purewhite.io/subtyping.html)
- 示例：[_13_example.rs](./_13_example.rs)

    

    ```rust
    #![allow(unused)]

    use std::marker::PhantomData;

    fn main() {}

    // &‘r 是协变的，整个 PhantomData<T> 协变的，所以 Foo<'r> 也遵循协变原则
    struct Foo<'r> {
        _phantom: PhantomData<&'r ()>,
    }

    fn foo<'short, 'long: 'short>( // 生命周期已经标识，long 是 short 的子类
        mut short_foo: Foo<'short>, // 如何判断 Foo<'long> 是 Foo<'short> 的子类
        mut long_foo: Foo<'long>
    ) {
        short_foo = long_foo;   // 遵循协变原则所以字类可以赋值给父类的
        // long_foo = short_foo // 这里遵循协变，所以父类不可赋值给子类
    }
    ```

- 示例：[_14_example.rs](./_14_example.rs)

    ```rust
    #![allow(unused)]
    use std::marker::PhantomData;

    fn main() {}

    struct Foo<'r> {    // Foo<'r> 是逆变的
        _phantom: PhantomData<fn(&'r ())>, // 对于函数而言参数是逆变的
    }

    fn foo<'short, 'long: 'short>(mut short_foo: Foo<'short>, mut long_foo: Foo<'long>) {
        // short_foo = long_foo;    // 逆变子类无法赋值给父类
        long_foo = short_foo
    }
    ```

- 示例：[_15_example.rs](./_15_example.rs)

    ```rust
    #![allow(unused)]
    use std::marker::PhantomData;

    fn main() {}

    struct Foo<'r> {
        _phantom: PhantomData<&'r ()>,
    }

    fn foo<'short, 'long: 'short>(
        // `&mut Foo<'short>` 和 `&mut Foo<'long>` 是两个完全不同的类型，不存在交集
        // `&mut Foo<'short>` 可以理解成 `&'a mut T` 不变的，`*mut T` 也是不变的
        mut short_foo: &mut Foo<'short>, 
        mut long_foo: &mut Foo<'long>
    ) {
        // short_foo = long_foo;    // 遵循不变原则：所以生命周期无法协变，逆变了
        // long_foo = short_foo
    }
    ```

- 示例：[_16_example.rs](./_16_example.rs)

    ```rust
    #![allow(unused)]
    fn main() {
        let string = String::from("hello world"); // string 没有引用
        // foo(&string);    // foo 约束 string 为 `&'static T`，所以 string 不满足
        bar(&string);   // string 没有引用，所以 `T: 'static` 不做约束
    }

    fn foo<T>(_input: &'static T) // `&'static T` 自能代表一种不可变引用，它的的生命周期只能是 `'static` 这么长
    {
        println!("foo works");
    }

    fn bar<T: 'static>(_input: &T) // `T` 代表所有的类型，如果 `T` 包含引用，这个引用的生命周期一定要比 `'static` 长；如果不包含引用不受约束
    {
        println!("bar works");
    }
    ```

- 示例：[_17_example.rs](./_17_example.rs)

    ```rust
    struct Manager<'a> {
        text: &'a str,
    }

    struct Interface<'a> {  // 所以 `’a` 是不变的
        manager: &'a mut Manager<'a>,   // `&'a mut Manager<'a>` 可以看作 `&'a mut T` 对于 `‘a` 是协变的，对于 `T` 是不变的
    }

    struct List<'a> {
        manager: Manager<'a>,
    }

    impl<'a> List<'a> {
        pub fn get_interface(&'a mut self) -> Interface {
            Interface {
                manager: &mut self.manager,
            }
        }
    }

    impl<'a> Interface<'a> {
        pub fn noop(self) {
            println!("interface consumed");
        }
    }

    fn use_list(list: &List) {
        println!("{}", list.manager.text);
    }

    fn main() {
        let mut list = List {
            manager: Manager { text: "hello" },
        };

        list.get_interface().noop();
        use_list(&list);
    }
    ```

## 扩展

`逆变（Inversion）` 和 `协变（Coercion）`是两种不同的类型转换机制。

- 逆变是指将一个类型转换为另一个类型，同时保证转换后的类型安全性。逆变可以通过 `trait` 实现，例如，可以使用 `AsRef` 和 `AsMut traits` 将一个引用类型转换为另一个引用类型，这被称为逆变。

- 协变是指将一个类型转换为另一个类型，但转换后的类型安全性不能得到保证。协变通常是通过手动类型转换实现的，例如，将一个整数类型转换为另一个整数类型。协变可能会导致运行时错误，因此在编写代码时要格外小心。
