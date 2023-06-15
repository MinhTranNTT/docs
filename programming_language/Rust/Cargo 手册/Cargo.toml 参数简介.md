- [`cargo-features`] — 只能用于 `nightly`版本的 `feature`

- `[package]` — 定义项目(package)的元信息
  
  `[name]` — 名称
  `[version]` — 版本
  `[authors]` — 开发作者
  `[edition]` — Rust edition.
  `[rust-version]` — 支持的最小化 Rust 版本
  `[description]` — 描述
  `[documentation]` — 文档 URL
  `[readme]` — README 文件的路径
  `[homepage]` - 主页 URL
  `[repository]` — 源代码仓库的 URL
  `[license]` — 开源协议 License.
  `[license-file]` — License 文件的路径.
  `[keywords]` — 项目的关键词
  `[categories]` — 项目分类
  `[workspace]` — 工作空间 workspace 的路径
  `[build]` — 构建脚本的路径
  `[links]` — 本地链接库的名称
  `[exclude]` — 发布时排除的文件
  `[include]` — 发布时包含的文件
  `[publish]` — 用于阻止项目的发布
  `[metadata]` — 额外的配置信息，用于提供给外部工具
  `[default-run]` — `[cargo run]` 所使用的默认可执行文件( binary )
  `[autobins]` — 禁止可执行文件的自动发现
  `[autoexamples]` — 禁止示例文件的自动发现
  `[autotests]` — 禁止测试文件的自动发现
  `[autobenches]` — 禁止 bench 文件的自动发现
  `[resolver]` — 设置依赖解析器( dependency resolver)
  
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



### toml 参考

```toml
[package]
name = "hello_world" # the name of the package
version = "0.1.0"    # the current version, obeying semver
authors = ["Alice <a@example.com>", "Bob <b@example.com>"]
edition = '2021'
rust-version = "1.56"
description = "A short description of my package"
documentation = "https://docs.rs/bitflags"
readme = "README.md"
homepage = "https://serde.rs/"
repository = "https://github.com/rust-lang/cargo/"
license = "MIT OR Apache-2.0"
license-file = "LICENSE.txt"
keywords = ["gamedev", "graphics"]
categories = ["command-line-utilities", "development-tools::cargo-plugins"]
workspace = "path/to/workspace/root"
build = "build.rs"
links = "foo"
exclude = ["/ci", "images/", ".*"]
include = ["/src", "COPYRIGHT", "/examples", "!/examples/big_example"]



```

