`&mut` 引用（指向同一块地址）可变
`&` 引用 本身是不可变的
`let` 声明变量, 默认情况下变量是不可变的
Rust 中格式字符串中的占位符不是 "% + 字母" 的形式，而是一对 {}。
Rust 语言为了高并发安全而做的设计：在语言层面尽量少的让变量的值可以改变。所以 a 的值不可变。但这不意味着 a 不是"变量"（英文中的 variable），官方文档称 a 这种变量为"不可变变量"。
`a = "abc";` 错误在于当声明 a 是 123 以后，a 就被确定为整型数字，不能把字符串类型的值赋给它。
`a = 4.56;` 错误在于自动转换数字精度有损失，Rust 语言不允许精度有损失的自动数据类型转换。
`a = 456;` 错误在于 a 不是个可变变量。
`let mut a = 1;` 变量变得"可变"（mutable）只需一个 mut 关键字。

```rust
fn main() {
    let mut a = 123;
    println!("a is {}", a); //
    println!("a is {}, a again is {}", a, a); // 把 a 输出两遍，那岂不是要写成
    println!("a is {0}, a again is {0}", a);
    println!("{{}}"); // {}

    a = 456;
    const b: i32 = 123; // 常量
}

```



数据类型

`3.0` 默认类型 `f64`，可以使用 `let y: f32 = 3.0;` 显示类型修改类型为 `f32`
加上 `=` 号是自运算的意思 `sum += 1` 等同于 `sum = sum + 1`。
`+` 加，`-` 减，`*` 乘，`/` 除，`%` 求余

```rust
fn main() {
    let x = 2.0;
    let y: f32 = 3.0;
    println!("{0},{1}", x, y);

    let sum = 5 + 10;
    let difference = 95.5 - 4.3;
    let product = 4 * 30;
    let quotient = 56.7 / 32.2;
    let remainder = 43 % 5;
    println!("{0},{1},{2},{3},{4}", sum, difference, product, quotient, remainder);
}

```



### `ref` 简介

在 Rust 语言中，`ref` 是一个关键字，用于创建引用绑定。

当我们使用 `let` 语句来创建变量时，如果希望该变量是一个引用，我们可以在绑定名前加上 `ref` 关键字，这样绑定的结果就是一个指向变量的不可变引用。

例如：`let ref bar = foo;` 中 `bar` 是一个不可变引用，指向 `foo` 所指的位置

```rust
let foo = "Hello";
let ref bar = foo;

```

这个语法的作用与 `&` 运算符类似，区别在于 `ref` 关键字可以让我们轻松地创建一个具有变量名的引用，而不需要通过 `&` 运算符显式地写出变量名和 `&` 符号。它还可以用于在模式匹配中捕获变量的引用。

例如：`first_element` 是一个不可变引用，指向 vec 的第一个元素

```rust
let mut vec = vec![1, 2, 3];
if let Some(&ref first_element) = vec.first() {
    println!("The first element of the vector is {}", first_element);
}

```

这里，`Some(&ref first_element)` 表示匹配一个指向可选值中第一个元素的引用，而 `ref` 关键字使得 `first_element` 成为一个具有变量名的引用。由于 `first` 方法返回的是一个 `Option<&T>` 类型的值，因此需要在模式中使用 `&` 符号解引用匹配一个指向元素的引用。

总之，`ref` 关键字是 Rust 语言中创建引用绑定的一种方式，它能够让我们轻松地创建一个具有变量名的引用，方便使用和模式匹配。









### `?` 简介

`?`是一个语法糖，用于在函数返回值类型为 `Result` 的情况下，简化错误处理的写法。当我们使用 `?` 语法糖来处理 `Result` 类型时，它会默认执行一次 `match` 语句，若返回了 `Err`，则直接将该 `Err` 作为整个函数的返回值。若返回了 `Ok`，则取出 `Ok` 中的值，并将其返回。这样，我们便可以省去不必要的 `match` 语句，节省了时间和代码量。但是需要注意的是，使用 `?` 只能在返回值类型为 `Result` 的函数中使用。

当函数返回值类型为`Result`的时候，使用`?`可以大大减少代码量，例如下面这个例子：

```rust
fn parse_input(input: &str) -> Result<i32, ParseIntError> {
    let parsed = input.parse::<i32>()?;
    Ok(parsed)
}

```

在这里，我们只需要在 `input.parse::<i32>()` 后面加上一个 `?`，如果出现了错误，它会直接返回错误值。另外，在这里，`ParseIntError` 类型是标准库 `std::num::ParseIntError` 的别名。

再看下一个例子：

```rust
fn read_file_content(file_path: &str) -> Result<String, std::io::Error> {
    let mut file = File::open(file_path)?;
    let mut content = String::new();
    file.read_to_string(&mut content)?;
    Ok(content)
}

```

这个例子中，我们打开一个文件，从中读取内容并返回它。在文件打开和读取时，都可以使用 `?` 来简化代码。



让我举例说明。

假设有一个函数`divide`，它接受两个`i32`参数并返回它们的商，除数不能为0，如果除数为0，则返回一个错误。

不使用`?`的代码如下：

```rust
fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        return Err("Cannot divide by zero".to_string());
    }
    Ok(a / b)
}

fn main() {
    let result = divide(10, 0);
    match result {
        Ok(v) => println!("{}", v),
        Err(e) => println!("Error: {}", e),
    }
}

```

这里我们需要编写一个`match`语句来处理`Result`类型的返回值，其中`Ok`分支表示操作成功，`Err`分支表示出现错误。在`Err`分支中，我们需要编写错误处理代码。

使用`?`的代码如下：

```rust
fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        return Err("Cannot divide by zero".to_string());
    }
    Ok(a / b)
}

fn main() -> Result<(), String> {
    let result = divide(10, 0)?;
    println!("{}", result);
    Ok(())
}

```

使用`?`可以使代码更加清晰简洁，只需要在函数调用的末尾加上一个`?`即可。如果函数返回一个`Err`，它会直接返回，否则它会返回包装在`Ok`中的值。我们还可以看到，我们不需要一个`match`语句来处理错误，而是可以使用`?`在函数调用之间进行链式调用。最后，我们需要在`main`函数中返回一个`Result`类型的值，这里我使用了一个简单的空元组`()`。



### `?` 和 `unwrap()` 区别

在 Rust 中，`?`和`unwrap()`都可以用来处理 `Result` 类型的值，但它们之间有着重要的区别。

- `?`：如果在函数中使用 `?`，如果函数返回一个 `Err`，那么这个错误会被返回给调用函数，并且它的类型会与当前函数的返回类型相同（通常是`Result`）。这意味着在使用 `?` 时，你必须声明当前函数的返回类型，以便编译器知道如何返回错误。

使用`?`的优点是它使代码更加简洁，对于错误处理代码的重复性更少。然而，它的缺点是它只适用于函数返回`Result`类型的情况。

- `unwrap()`：如果我们使用`unwrap()`，它会将`Result`类型的值解包并返回其中的`Ok`值，如果`Result`为`Err`，则会产生一个运行时错误并使程序崩溃。因此，我们通常只在我们非常确定`Result`是`Ok`时使用`unwrap()`。如果我们使用`unwrap()`并且`Result`实际上是一个`Err`，那么程序将会崩溃。所以，使用`unwrap()`需要非常小心。

总之，我们应当尽可能地使用`?`来处理错误，只有当我们非常确定`Result`是`Ok`时才使用`unwrap()`。这样可以使我们的代码更加健壮和稳定。

以下是一个使用?的示例代码：

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_file_contents(file_path: &str) -> io::Result<String> {
    let mut file = File::open(file_path)?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}

fn main() {
    let file_path = "example.txt";
    match read_file_contents(file_path) {
        Ok(contents) => println!("{}", contents),
        Err(error) => println!("Failed to read {}: {}", file_path, error),
    }
}

```

在上述代码中，我们使用了?运算符来处理读取文件时的错误。如果在文件打开或读取期间发生错误，?会将错误返回给调用函数，并在main函数中打印出错误信息。

下面是一个使用unwrap()的示例代码：

```rust
fn divide(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("Cannot divide by zero!");
    } else {
        a / b
    }
}

fn main() {
    let a = 10;
    let b = 2;
    let result = divide(a, b).unwrap();
    println!("{} / {} = {}", a, b, result);
}

```

在上述代码中，我们使用了 `unwrap()` 来解包函数 `divide` 返回的值。在这种情况下，我们确信 `b` 不会为零，所以我们可以用 `unwrap()` 来获取 `divide` 的返回值。然而，如果 `b` 为零，程序将会崩溃并打印 `"Cannot divide by zero!"`。因此，我们需要小心使用 `unwrap()`。





### dyn 和 impl 是 Rust 中的关键字，用于定义 trait 对象和泛型实现。

- dyn：用于定义 trait 对象。在 Rust 中，trait 对象是一种不确定类型，可以在运行时确定具体的类型。通过将 trait 指针包装在 Box 中创建 trait 对象。例如，`Box<dyn MyTrait>` 表示一个 MyTrait trait 对象的指针。
- impl：用于定义泛型实现。在 Rust 中，可以定义泛型类型和泛型函数。通过 impl 关键字，可以为泛型类型或函数实现具体的行为。例如，对于泛型类型 T，可以使用 `impl<T> MyTrait for T {}` 为 T 实现 MyTrait trait。

```rust
trait Animal {
    fn make_sound(&self) -> String;
}

struct Dog {
    name: String,
}

impl Animal for Dog {
    fn make_sound(&self) -> String {
        return format!("{} barks", self.name);
    }
}

fn make_animal_sound(animal: &dyn Animal) {
    println!("{}", animal.make_sound());
}

fn main() {
    let dog = Dog {
        name: String::from("Rufus")
    };

    make_animal_sound(&dog);
}

```

这个例子中，我们定义了一个 `Animal` trait，然后使用 `impl` 为 `Dog` 类型实现了这个 trait。`make_animal_sound` 函数接收一个 `dyn Animal` 类型的参数，这意味着它可以接收任何实现了 `Animal` trait 的类型。在 `main` 函数中，我们创建了一个 `Dog` 实例并将其传递给 `make_animal_sound` 函数。`make_animal_sound` 函数调用了 `Dog` 实例上的 `make_sound` 方法，因为 `Dog` 类型实现了 `Animal` trait。



### 判断一个整数是否为 2 的幂次方的

这段代码是用来判断一个整数是否为 2 的幂次方的。我们知道，2 的幂次方在二进制中的表现形式是只有一个 1，其余全是 0。例如，2 的 3 次方是 8，其二进制表示为 0b1000。而 9 就不是 2 的幂次方，因为它的二进制表示为 0b1001，其中不只有一个 1。

而这段代码的实现方法是，首先判断这个数是否是正整数，如果是负数或者 0，那么显然不是 2 的幂次方。接下来，我们用 n 和 n - 1 做位运算，如果结果为 0，那么说明 n 中只有一个 1，其余全是 0，满足 2 的幂次方的特点，返回 true。否则，返回 false。

具体来说，如果一个数是 2 的幂次方，那么它的二进制表示形如 1000、10000、100000 这样的形式，即只有一位是 1，其余位都是 0。而将这个数减去 1，会使得原来那个二进制位由 1 变成 0，后面所有的 0 位都变成 1。例如，对于数 8，其二进制表示为 1000，将它减去 1 得到 0111。然后，把这两个数做位与运算，得到结果 0，这说明它们二进制位上只有一个 1。而对于不是 2 的幂次方的数，它们的二进制表示中肯定会有多个 1，减去 1 后，其中的某一位上的 1 会变成 0，导致位与的结果不为 0。

因此，这段代码的实现方法非常巧妙和高效，只需要进行一次判断和一次位运算就能得到答案。

案例：

当 n = 4 时，4 的二进制表示为 0b100，n - 1 的二进制表示为 0b011，它们进行与运算得到的结果为 0，因此 n 是 2 的幂次方。

当 n = 5 时，5 的二进制表示为 0b101，n - 1 的二进制表示为 0b100，它们进行与运算得到的结果为 0b100，不为 0，因此 n 不是 2 的幂次方。

当 n = 16 时，16 的二进制表示为 0b10000，n - 1 的二进制表示为 0b1111，它们进行与运算得到的结果为 0，因此 n 是 2 的幂次方。

```rust
impl Solution {
    pub fn is_power_of_two(n: i32) -> bool {
        n > 0 && n & (n - 1) == 0
    }
}
```



### 与运算符

与运算符是 `&` 符号。它的作用是对两个二进制数的每一位进行与运算，即如果两个对应位都为 1，则该位结果为 1；否则为 0。例如，对于二进制数 0b1101 和 0b1010，它们进行与运算的结果为 0b1000。在 Rust 中，与运算符也可以用于布尔类型的值进行逻辑运算，其中 true 表示 1，false 表示 0。
