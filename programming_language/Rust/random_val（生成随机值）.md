### 生成随机数

[![rand_distr-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand_distr)](https://docs.rs/rand_distr/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

这段代码使用了 Rust 的 `rand crate`（随机数生成库），生成了不同类型的随机数，并打印出来。
`let mut rng = rand::thread_rng();` 创建了一个随机数生成器（RNG），使用了当前线程作为种子。
`rng.gen::<u8>()` 通过 `gen()` 方法生成一个 u8 类型的随机数。
同样的方法可以生成 `u16`、`u32`、`i32` 和 `f64` 类型的随机数。
最后通过 `println!` 宏打印出来。

```rust
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();
    
    println!("Random u8: {}", rng.gen::<u8>());
    println!("Random u16: {}", rng.gen::<u16>());
    println!("Random u32: {}", rng.gen::<u32>());
    println!("Random i32: {}", rng.gen::<i32>());
    println!("Random float: {}", rng.gen::<f64>());
}

```



### 生成范围内随机数

[![rand-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand)](https://docs.rs/rand/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

通过 `rng.gen_range()` 方法生成一个区间范围内的随机数，但是该方法需要传入一个范围参数，否则编译时会出错。新版本的Rust中，数字区间的语法已经改为双点号 `(0..10)`

```rust
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();
    println!("Integer: {}", rng.gen_range(0..10));
    println!("Float: {}", rng.gen_range(0.0..10.0));
}

```



### 生成给定分布随机数

[![rand-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand)](https://docs.rs/rand/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

这段代码报错是因为 `sample()` 方法需要的是一个泛型类型实现了 `Distribution trait`，但是这里的 `Uniform` 类型并没有实现 `Distribution trait`，导致编译器无法通过类型检查。要解决这个问题，可以通过为 `Uniform` 类型实现 `Distribution trait` 的方式来解决。这里我们通过为 `Uniform` 类型实现 `Distribution trait` 并改用 `rng.sample(die)` 方法来生成随机数。`rand::thread_rng()` 创建一个随机数生成器 `Uniform::from(1..7)` 创建一个区间为 `[1, 6]` 的均匀分布 `loop` 进入一个无限循环 `die.sample(&mut rng)` 从均匀分布中抽取一个随机样本，表示掷骰子的结果。如果掷骰子的结果为 `6`，则跳出循环

```rust
use rand::distributions::{Distribution, Uniform};

fn main() {
    let mut rng = rand::thread_rng();
    let die = Uniform::from(1..7);
    loop {
        let throw = die.sample(&mut rng);
        println!("Roll the die: {}", throw);
        if throw == 6 {
            break;
        }
    }
}

```



### 生成给定分布随机数

[![rand_distr-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand_distr)](https://docs.rs/rand_distr/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

`main` 函数，返回类型为 `Result<(), NormalError>`，可能出现错误类型为 `NormalError` 
`rand::thread_rng();` 创建一个随机数生成器
`Normal::new(2.0, 3.0)?;` 创建一个均值为 2，标准差为 3 的正态分布
`normal.sample(&mut rng);` 从正态分布中抽取一个随机样本
`Ok(())` 表示函数执行成功

```rust
use rand_distr::{Distribution, Normal, NormalError};

fn main() -> Result<(), NormalError> {
    let mut rng = rand::thread_rng();
    let normal = Normal::new(2.0, 3.0)?;
    let v = normal.sample(&mut rng);
    println!("{} is from a N(2, 9) distribution", v);
    Ok(())
}

```



### 生成自定义类型随机值

[![rand-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand)](https://docs.rs/rand/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

```rust
use rand::distributions::{Distribution, Standard};
use rand::Rng;

#[derive(Debug)]
#[allow(dead_code)]
struct Point {
    x: i32,
    y: i32,
}

impl Distribution<Point> for Standard {
    fn sample<R: Rng + ?Sized>(&self, rng: &mut R) -> Point {
        let (rand_x, rand_y) = rng.gen();
        Point {
            x: rand_x,
            y: rand_y,
        }
    }
}

fn main() {
    let mut rng = rand::thread_rng();
    let rand_tuple = rng.gen::<(i32, bool, f64)>();
    let rand_point: Point = rng.gen();
    println!("Random tuple: {:?}", rand_tuple);
    println!("Random Point: {:?}", rand_point);
}

```



### 从一组字母数字字符创建随机密码

[![rand-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand)](https://docs.rs/rand/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

使用 `Rust` 的线程随机数生成器生成一个包含 30 个字母和数字的随机字符串
`rand::thread_rng()` 创建一个新的线程随机数生成器
`sample_iter(&Alphanumeric)` 采样字母和数字
`take(30)` 只取 30 个元素
`map(char::from)` 将每个元素映射到一个字符
`collect()`  将字符收集到一个字符串中

```rust
use rand::distributions::Alphanumeric;
use rand::Rng;

fn main() {
    let rand_string: String = rand::thread_rng()
        .sample_iter(&Alphanumeric)
        .take(30)
        .map(char::from)
        .collect();
    println!("{}", rand_string);
}

```



### 从一组用户定义字符创建随机密码

[![rand-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand)](https://docs.rs/rand/) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

```rust
use rand::Rng;

fn main() {
    // 定义一个包含所有可用字符的常量字符数组
    const CHARSET: &[u8] = b"ABCDEFGHIJKLMNOPQRSTUVWXYZ\
        abcdefghijklmnopqrstuvwxyz\
        0123456789)(*&^%$#@!~";

    const PASSWORD_LEN: usize = 30;
    let mut rng = rand::thread_rng();

    // 使用 map 函数生成一个长度为 PASSWORD_LEN 的密码字符串
    let password: String = (0..PASSWORD_LEN)
        .map(|_| {
            // 随机选择 CHARSET 中的一个字符作为密码的一位
            let idx = rng.gen_range(0..CHARSET.len());
            CHARSET[idx] as char
        })
        .collect();
    println!("{:?}", password);
}

```

