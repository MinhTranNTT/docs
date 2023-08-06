# Rust 更新


稳定版和nightly版的升级


```rust
~> rustup update                                                                                                  2023/07/16 01:06:05 下午
info: syncing channel updates for 'stable-x86_64-pc-windows-msvc'
info: checking for self-update

  stable-x86_64-pc-windows-msvc unchanged - rustc 1.71.0 (8ede3aae2 2023-07-12)

info: cleaning up downloads & tmp directories

```


rustup升级

```rust
~> rustup self update                                                                                             2023/07/16 01:11:12 下午
info: checking for self-update
  rustup unchanged - 1.26.0

```

nightly版安装

命令行输入

```rust
rustup install nightly

```

```rust
$ rustup install nightly
info: syncing channel updates for 'nightly'
info: downloading toolchain manifest
info: downloading component 'rustc'
info: downloading component 'rust-std'
info: downloading component 'rust-docs'
info: downloading component 'cargo'
info: installing component 'rustc'
info: installing component 'rust-std'
info: installing component 'rust-docs'
info: installing component 'cargo'

nightly installed: rustc 1.9.0-nightly (02310fd31 2016-03-19)

```

查询版本

```rust
rustup run nightly rustc --version
```

选择稳定版或者nightly版

如果想长期使用 nightly版。

```rust
rustup default nightly
```