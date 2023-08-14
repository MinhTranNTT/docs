### 打印彩色文本到终端

[![ansi_term-badge](https://badge-cache.kominick.com/crates/v/base64.svg?label=ansi_term)](https://docs.rs/ansi_term/) [![cat-command-line-badge](https://badge-cache.kominick.com/badge/command_line--x.svg?style=social)](https://crates.io/categories/command-line-interface)


```rust
use ansi_term::Colour;

fn main() {
    println!(
        "This is {} in color, {} in color and {} in color",
        Colour::Red.paint("red"),
        Colour::Blue.paint("blue"),
        Colour::Green.paint("green")
    );
}

```



### 终端中的粗体文本

[![ansi_term-badge](https://badge-cache.kominick.com/crates/v/base64.svg?label=ansi_term)](https://docs.rs/ansi_term/) [![cat-command-line-badge](https://badge-cache.kominick.com/badge/command_line--x.svg?style=social)](https://crates.io/categories/command-line-interface)

```rust
use ansi_term;

fn main() {
    println!(
        "{} and this is not",
        ansi_term::Style::new().bold().paint("This is Bold")
    );
}

```



### 终端中的粗体和彩色文本

[![ansi_term-badge](https://badge-cache.kominick.com/crates/v/base64.svg?label=ansi_term)](https://docs.rs/ansi_term/) [![cat-command-line-badge](https://badge-cache.kominick.com/badge/command_line--x.svg?style=social)](https://crates.io/categories/command-line-interface)

```rust
use ansi_term::Colour;
use ansi_term::Style;

fn main() {
    println!(
        "{}, {} and {}",
        Colour::Yellow.paint("This is colored"),
        Style::new().bold().paint("this is bold"),
        Colour::Yellow.bold().paint("this is bold and colored")
    );
}

```

