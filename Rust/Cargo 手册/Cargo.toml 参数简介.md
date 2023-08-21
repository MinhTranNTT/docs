
```toml
[package] # 定义项目(package)的元信息
name = "async"  # 项目名称
version = "0.1.0" # 版本号
authors = ["Your Name <you@example.com>"] # 项目作者
edition = "2021"  # 项目使用的 Rust 版本；支持的最小化 Rust 版本
description = "A simple example of a Rust library" # 项目描述
documentation = "https://docs.rs/async" # 项目文档地址
readme = "README.md" # 项目 README 文件
homepage = "https://github.com/rust-lang/rust-cookbook" # 项目主页
repository = "https://github.com/rust-lang/rust-cookbook" # 项目仓库地址
license = "MIT OR Apache-2.0" # 项目使用的开源协议
license-file = "LICENSE-APACHE" # 项目使用的开源协议文件
keywords = ["example", "cookbook", "library"] # 项目关键字
categories = ["development-tools", "web-programming"] # 项目类别
workspace = "./" # 项目工作空间
build = "build.rs" # 项目构建脚本
links = "async" # 项目库链接名称
exclude = ["/doc", "tests/*", "tests"] # 发布时排除的文件
include = ["src/*"] # 发布时包含的文件
publish = false # 发布时是否发布
metadata = true # 是否生成 crate 信息
default-run = "main" # `[cargo run]` 所使用的默认可执行文件( binary )


# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
libp2p = "0.46.1"
tokio = { version = "1.19.2", features = ["full"] }
async-std = { version = "1.12.0", features = ["attributes"] }
futures = "0.3"
mini-redis = "0.4"
bytes = "1"

[workspace]
members = [
    "examples/my_redis",
    "examples/rust-hyper-ex",
    "examples/p2p",
    
]
```

  
- Cargo Target 列表: (查看 `Target 配置` 获取详细设置)

  - `[lib]` — Library target 设置.
  
- `[[bin]]` — Binary target 设置.
  - `[[example]]` — Example target 设置.
  
- `[[test]]` — Test target 设置.
  - `[[bench]]` — Benchmark target 设置.

- Dependency tables:

  - `[dependencies]` — 项目依赖包
  - `[dev-dependencies]` — 用于 examples、tests 和 benchmarks 的依赖包
  - `[build-dependencies]` — 用于构建脚本的依赖包
  - `[target]` — 平台特定的依赖包

- `[badges]` — 用于在注册服务(例如 crates.io ) 上显示项目的一些状态信息，例如当前的维护状态：活跃中、寻找维护者、deprecated

- `[features]` — `features` 可以用于条件编译

- `[patch]` — 推荐使用的依赖覆盖方式

- `[replace]` — 不推荐使用的依赖覆盖方式 (deprecated).

- `[profile]` — 编译器设置和优化

- `[workspace]` — 工作空间的定义
