# rust 指令参考

## rustc 参数指令

|指令|解释|
| --------------------| ---------------------|
|rustc runoob.rs|编译 runoob.rs 文件|
|rustc --version|查看版本|
|​​​./runoob​​|执行 runoob|

## cargo 指令说明

```cargo
[root@localhost ~]# cargo
Rust's package manager

Usage: cargo [+toolchain] [OPTIONS] [COMMAND]

Options:
  -V, --version             Print version info and exit
      --list                List installed commands
      --explain <CODE>      Run `rustc --explain CODE`
  -v, --verbose...          Use verbose output (-vv very verbose/build.rs output)
  -q, --quiet               Do not print cargo log messages
      --color <WHEN>        Coloring: auto, always, never
      --frozen              Require Cargo.lock and cache are up to date
      --locked              Require Cargo.lock is up to date
      --offline             Run without accessing the network
      --config <KEY=VALUE>  Override a configuration value
  -Z <FLAG>                 Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
  -h, --help                Print help information

Some common cargo commands are (see all commands with --list):
    build, b    编译当前包
    check, c    分析当前包并报告错误，但不要构建目标文件
    clean       删除目标目录
    doc, d      构建此包及其依赖项的留档
    new         创建一个创建项目包
    init        在现有目录中创建一个新的货物包
    add         Add dependencies to a manifest file
    remove      Remove dependencies from a manifest file
    run, r      Run a binary or example of the local package
    test, t     Run the tests
    bench       Run the benchmarks
    update      Update dependencies listed in Cargo.lock
    search      Search registry for crates
    publish     Package and upload this package to the registry
    install     Install a Rust binary. Default location is $HOME/.cargo/bin
    uninstall   Uninstall a Rust binary

See 'cargo help <command>' for more information on a specific command.
```

|指令|解释|
| ------| ----------------------------------------------------|
|​`cargo --version`​|cargo 版本|
|​`cargo new greeting`​|cargo 创建项目|
|​`cargo build`​|构建|
|​`cargo run`​|运行|
|​`cargo check`​|检查代码，确保能通过编译，但是不产生任何可执行文件|
|​`cargo build --release`​|为发布构建|

## Cargo.toml

```toml
[package]
name = "rust_dome"	# 项目名
version = "0.1.0"	# 项目版本
edition = "2021"	# cargo 版本

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

# 四种依赖类型
[dependencies]		# 项目依赖包
[dev-dependencies]	# 用于测试（包括性能测试）的依赖包
[build-dependencies]	# 用于构建脚步的依赖包
[target]		# 平台特定依赖包
```

## Cargo.toml 包的四种配置

```toml
[dev]		# 缺失配置
[release]	# 优化运行时性能，适用于生产发布
[test]		# 用于cargo test 命令， 构建用于测试的可执行文件
[bench]		# 用于cargo bench 命令，构建用于性能测试的可执行文件（运行带#[bench]注解的函数）
```