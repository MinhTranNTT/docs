# Rust 基础编程概念

[Search Results for &apos;rand&apos; - crates.io: Rust Package Registry](https://crates.io/search?q=rand)

```rust
    let mut guess = String::new();
    std::io::stdin().read_line(&mut guess).expect("无法读取行"); // &mut 引用（指向同一块地址）可变； & 引用 本身是不可变的
    println!("你猜测的数是：{}", guess);

    let a = 12;  // let 声明变量, 默认情况下变量是不可变的
    println!("a is {}", a); // Rust 中格式字符串中的占位符不是 "% + 字母" 的形式，而是一对 {}。
    println!("a is {}, a again is {}", a, a);   // 把 a 输出两遍，那岂不是要写成
    println!("a is {0}, a again is {0}", a);
    println!("{{}}");   // {}

    // Rust 语言为了高并发安全而做的设计：在语言层面尽量少的让变量的值可以改变。所以 a 的值不可变。但这不意味着 a 不是"变量"（英文中的 variable），官方文档称 a 这种变量为"不可变变量"。
    a = "abc";  // 错误在于当声明 a 是 123 以后，a 就被确定为整型数字，不能把字符串类型的值赋给它。
    a = 4.56;   // 错误在于自动转换数字精度有损失，Rust 语言不允许精度有损失的自动数据类型转换。
    a = 456;    // 错误在于 a 不是个可变变量。
    let mut a = 123;    // 变量变得"可变"（mutable）只需一个 mut 关键字。
    a = 456;
    const a: i32 = 123;     // 常量
```

‍

# 2、数据类型

```rust
    2、数据类型
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
    println!("{0},{1}", x, y);
  
    // 加上 = 号是自运算的意思 sum += 1 等同于 sum = sum + 1。
    let sum = 5 + 10; // 加
    let difference = 95.5 - 4.3; // 减
    let product = 4 * 30; // 乘
    let quotient = 56.7 / 32.2; // 除
    let remainder = 43 % 5; // 求余
    println!("{0},{1},{2},{3},{4}", sum, difference, product, quotient, remainder);
```
