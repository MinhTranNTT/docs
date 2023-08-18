# `dyn`

[dyn - Rust](https://rustwiki.org/zh-CN/std/keyword.dyn.html)
- `dyn` 是 Rust 中的一个关键字，用于指定 `trait` 对象中的动态分发机制。

    - `trait` 是一种用于定义接口的机制，它类似于 `Java` 或 `C#` 中的接口。通过定义 `trait`，我们可以指定一个类型所必须实现的方法或属性，从而使得这个类型具有某些特定的行为或能力。但是，和其他面向对象语言不同的是，`trait` 并不支持运行时的动态分发，也就是说，我们在**编译时并不知道使用哪个具体的方法或实现**。

    - 为了解决在**运行时确定**的这个问题。`trait` 对象可以使用 `dyn` 关键字来标记。

- 示例：[dyn_example.rs](./dyn_example.rs)

```rust
trait Foo {
    fn foo(&self);
}

struct Bar;

impl Foo for Bar {
    fn foo(&self) {
        println!("Bar");
    }
}

fn main() {
    // Box 是一个泛型类型，可以包装任何类型的值，并且可以在编译时防止错误的类型被传递给一个函数或方法。
    // b 指向一个 Bar 类型的对象，但是将 b 封装为 Box<dyn Foo> 类型，因为 b 的实际类型在运行时才会确定。
    // let b: Box<Bar> = Box::new(Bar);
    let b: Box<dyn Foo> = Box::new(Bar);

    b.foo(); // 调用 Bar::foo 方法
}
```

在上面的例子中，`Box<dyn Foo>` 表示一个 trait 对象，它是一个 `Foo` trait 的实例。当我们使用 `Box::new(Bar)` 创建它时，实际上是将 `Bar` 实例包装在一个 `Box` 中，并将其转换为一个 `Foo` trait 对象。然后我们可以通过 `b.foo()` 调用 `Foo` trait 中的 `foo` 方法，并最终调用到 `Bar` 的 `foo` 方法。当我们调用方法时，Rust 会使用动态分发机制来查找正确的方法实现，以此实现运行时的多态性。
