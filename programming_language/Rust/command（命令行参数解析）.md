### 解析命令行参数

[![clap-badge](https://badge-cache.kominick.com/crates/v/clap.svg?label=clap)](https://docs.rs/clap/) [![cat-command-line-badge](https://badge-cache.kominick.com/badge/command_line--x.svg?style=social)](https://crates.io/categories/command-line-interface)

此应用程序使用 `clap` 构建器样式描述其命令行界面的结构。[文档](https://docs.rs/clap/)还提供了另外两种可用的方法去实例化应用程序。

在构建器样式中，`with_name` 函数是 `value_of` 方法将用于检索传递值的唯一标识符。`short` 和 `long` 选项控制用户将要键入的标志；short 标志看起来像 `-f`，long 标志看起来像 `--file`。

```rust
use clap;

fn main() {
    let matches = clap::Command::new("My Test Program")
        .version("0.1.0")
        .author("Hackerman Jones <hckrmnjones@hack.gov>")
        .about("Teaches argument parsing")
        .arg(
            clap::Arg::new("file")
                .short('f')
                .long("file")
                .exclusive(true)
                .help("A cool file"),
        )
        .arg(
            clap::Arg::new("num")
                .short('n')
                .long("number")
                .exclusive(true)
                .help("Five less than your favorite number"),
        )
        .get_matches();

    let myfile: Option<&String> = matches.get_one("file");
    println!("The file passed is: {:?}", myfile);

    let num_str: Option<&String> = matches.get_one("num");

    match num_str {
        None => println!("No idea what your favorite number is."),
        Some(s) => match s.parse::<i32>() {
            Ok(n) => println!("Your favorite number must be {}.", n + 5),
            Err(_) => println!("That's not a number! {}", s),
        },
    }
}

```

使用信息由 `clap` 生成。实例应用程序的用法如下所示。

```rust
PS D:\.github\hello_rust> cargo run -- -h                 
   Compiling hello_rust v0.1.0 (D:\.github\hello_rust)
    Finished dev [unoptimized + debuginfo] target(s) in 0.46s
     Running `target\debug\hello_rust.exe -h`
Teaches argument parsing

Usage: hello_rust.exe [OPTIONS]

Options:
  -f, --file <file>   A cool file
  -n, --number <num>  Five less than your favorite number
  -h, --help          Print help
  -V, --version       Print version
PS D:\.github\hello_rust>

```

我们可以通过运行如下命令来测试应用程序。问题 `fn` 无法一起使用

```rust
cargo run -- -f myfile.txt
cargo run -- -n 251

```

