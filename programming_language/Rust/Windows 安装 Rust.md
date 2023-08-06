# Windows Install Rust


## 1、Install rustup-init.exe

https://www.rust-lang.org/zh-CN/tools/install

![image-20230714104018199](assets/Windows%20%E5%AE%89%E8%A3%85%20Rust/image-20230714104018199.png)

### Windows Install Rust 安装太慢解决办法



1、打开powershell

2、分别执行下面两行代码：

- windows

```powershell
$ENV:RUSTUP_DIST_SERVER='https://mirrors.ustc.edu.cn/rust-static'
$ENV:RUSTUP_UPDATE_ROOT='https://mirrors.ustc.edu.cn/rust-static/rustup'
```
- linux

```sh
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

3、继续在此命令行下执行 rustup-init.exe





## 2、[Microsoft C++ 生成工具](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/) 不安装无法编译



![image-20230714104649977](assets/Windows%20%E5%AE%89%E8%A3%85%20Rust/image-20230714104649977.png)



## 3、MinGW64：https://sourceforge.net/projects/mingw-w64/files/ 

不安装代码提示，不全

![image-20230714104409591](assets/Windows%20%E5%AE%89%E8%A3%85%20Rust/image-20230714104409591.png)



![image-20230714104952300](assets/Windows%20%E5%AE%89%E8%A3%85%20Rust/image-20230714104952300.png)

```rust
~> gcc -v
Using built-in specs.
COLLECT_GCC=D:\program\mingw64\bin\gcc.exe
COLLECT_LTO_WRAPPER=D:/program/mingw64/bin/../libexec/gcc/x86_64-w64-mingw32/8.1.0/lto-wrapper.exe
Target: x86_64-w64-mingw32
Configured with: ../../../src/gcc-8.1.0/configure --host=x86_64-w64-mingw32 --build=x86_64-w64-mingw32 --target=x86_64-w64-mingw32 --prefix=/mingw64 --with-sysroot=/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64 --enable-shared --enable-static --disable-multilib --enable-languages=c,c++,fortran,lto --enable-libstdcxx-time=yes --enable-threads=win32 --enable-libgomp --enable-libatomic --enable-lto --enable-graphite --enable-checking=release --enable-fully-dynamic-string --enable-version-specific-runtime-libs --disable-libstdcxx-pch --disable-libstdcxx-debug --enable-bootstrap --disable-rpath --disable-win32-registry --disable-nls --disable-werror --disable-symvers --with-gnu-as --with-gnu-ld --with-arch=nocona --with-tune=core2 --with-libiconv --with-system-zlib --with-gmp=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-mpfr=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-mpc=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-isl=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-pkgversion='x86_64-win32-seh-rev0, Built by MinGW-W64 project' --with-bugurl=https://sourceforge.net/projects/mingw-w64 CFLAGS='-O2 -pipe -fno-ident -I/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' CXXFLAGS='-O2 -pipe -fno-ident -I/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' CPPFLAGS=' -I/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' LDFLAGS='-pipe -fno-ident -L/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/lib -L/c/mingw810/prerequisites/x86_64-zlib-static/lib -L/c/mingw810/prerequisites/x86_64-w64-mingw32-static/lib '
Thread model: win32
gcc version 8.1.0 (x86_64-win32-seh-rev0, Built by MinGW-W64 project)
```



## 4、配置 cargo 国内源

找到当前用户目录下 .cargo文件夹，建立config文件：

```bash
touch ~/.cargo/config
vim ~/.cargo/config
```

输入下面内容：

```toml
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
# 指定镜像
replace-with = '镜像源名' # 如：tuna、sjtu、ustc，或者 rustcc

# 注：以下源配置一个即可，无需全部

# 中国科学技术大学
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"

# 上海交通大学
[source.sjtu]
registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index"

# 清华大学
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"

# rustcc社区
[source.rustcc]
registry = "https://code.aliyun.com/rustcc/crates.io-index.git"
```

delete `.package-cache`

```sh
~/.cargo> rm -rf registry
~/.cargo> rm -rf .package-cache
```


## 5、Solve `Blocking waiting for file lock on package cache`

```text
rm -rf ~/.cargo/.package-cache
```


## 6、Install VScode

### VScode Install Extensions

rust-analyzer

Rust Syntax

Rust Flash Snippets

Even Better TOML

Error Lens

crates

CMake

CMake Tools

C/C++ Extension Pack



### Windows rust debug

`mkdir .vscode/launch.json`

```

{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(Windows) Launch",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${workspaceRoot}/target/debug/example.exe",	// 执行文件
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "console": "externalTerminal"
        }
    ]
}
```



## 扩展

### 使用 cargo expand 查看被宏隐藏的代码

使用 VScode 安装扩展 `Rust Macro Expand` 

需要安装以下软件:

- [cargo-expand](https://github.com/dtolnay/cargo-expand) A cargo crate for easier handling of compiler commands
- Rust nightly compiler, you can install it with `rustup toolchain install nightly`

#### cargo expand简介

- `cargo expand` 目前这个需要安装 `nightly` 的 `toolchain`，`rustup toolchain install nightly-x86_64-unknown-linux-gnu`

    ```sh
    ~> rustup toolchain install nightly
    info: syncing channel updates for 'nightly-x86_64-pc-windows-msvc'
    info: latest update on 2023-07-26, rust version 1.73.0-nightly (864bdf784 2023-07-25)
    info: downloading component 'cargo'
    info: downloading component 'clippy'
    info: downloading component 'rust-docs'
    info: downloading component 'rust-std'
    info: downloading component 'rustc'
    58.2 MiB /  58.2 MiB (100 %)  40.5 MiB/s in  1s ETA:  0s
    info: downloading component 'rustfmt'
    info: installing component 'cargo'
    info: installing component 'clippy'
    info: installing component 'rust-docs'
    13.7 MiB /  13.7 MiB (100 %)   2.0 MiB/s in  4s ETA:  0s
    info: installing component 'rust-std'
    23.6 MiB /  23.6 MiB (100 %)  16.2 MiB/s in  1s ETA:  0s
    info: installing component 'rustc'
    58.2 MiB /  58.2 MiB (100 %)  17.4 MiB/s in  3s ETA:  0s
    info: installing component 'rustfmt'

    nightly-x86_64-pc-windows-msvc installed - rustc 1.73.0-nightly (864bdf784 2023-07-25)

    info: checking for self-update
    ```

- 安装命令：`cargo +nightly install cargo-expand`

    ```sh
    ~> cargo +nightly install cargo-expand
        Updating `ustc` index
    Downloaded cargo-expand v1.0.62 (registry `ustc`)
    Downloaded 1 crate (26.8 KB) in 1.29s
    Installing cargo-expand v1.0.62
    Downloaded ansi_colours v1.2.2 (registry `ustc`)
    ...
        Finished release [optimized] target(s) in 1m 25s
    Installing C:\Users\liuzonglin\.cargo\bin\cargo-expand.exe
    Installed package `cargo-expand v1.0.62` (executable `cargo-expand.exe`)
    ```

#### 使用方法

- 源代码

    ```rust
    #![allow(non_camel_case_types)]
    use async_trait::async_trait;
    fn main() {}
    #[async_trait]
    trait example {
        async fn fetch(trace_id: &str, span_id: &str);
    }
    ```

- 运行 `cargo expand`

    ```sh
    ~> cargo expand
        Checking lifetime_kata v0.1.0 (D:\.github\rust\lifetime_kata)
        Finished dev [unoptimized + debuginfo] target(s) in 0.04s

    #![feature(prelude_import)]
    #![allow(non_camel_case_types)]
    #[prelude_import]
    use std::prelude::rust_2021::*;
    #[macro_use]
    extern crate std;
    use async_trait::async_trait;
    fn main() {}
    trait example {
        #[must_use]
        #[allow(clippy::type_complexity, clippy::type_repetition_in_bounds)]
        fn fetch<'life0, 'life1, 'async_trait>(
            trace_id: &'life0 str,
            span_id: &'life1 str,
        ) -> ::core::pin::Pin<
            Box<
                dyn ::core::future::Future<Output = ()> + ::core::marker::Send + 'async_trait,
            >,
        >
        where
            'life0: 'async_trait,
            'life1: 'async_trait;
    }
    ```


## reference

- https://learnblockchain.cn/article/1069
- https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index.git/
- https://rsproxy.cn/
- [安装 Rust - Rust 程序设计语言 (rust-lang.org)](https://www.rust-lang.org/zh-CN/tools/install)
- [Windows安装Rust指南 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv17841257)
- [rust中使用cargo expand查看被宏隐藏的代码](https://blog.csdn.net/hbuxiaofei/article/details/113958825)
- [cargo-expand 1.0.62 - Docs.rs](https://docs.rs/crate/cargo-expand/latest)