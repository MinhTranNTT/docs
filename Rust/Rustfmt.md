Rustfmt 是 Rust 格式化工具，用于将 Rust 代码按照统一的格式风格进行排版和格式化。Rustfmt 可以在编写代码时自动格式化，也可以在代码编译时使用 Cargo 命令进行格式化，使用方法如下：

1. 在项目目录下，打开终端，输入以下命令安装 Rustfmt：

```sh
cargo install rustfmt
```

2. 运行 Rustfmt 格式化代码，可使用以下命令：

```sh
cargo fmt
```

Rustfmt 会自动格式化当前目录下的所有 Rust 代码文件。如果你只想格式化某个文件，可使用以下命令：

```sh
rustfmt <filename>
```

Rustfmt 会自动对文件进行格式化，并输出格式化后的文件内容。

