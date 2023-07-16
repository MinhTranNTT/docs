### 1、安装 rustup-init.exe

https://www.rust-lang.org/zh-CN/tools/install

![image-20230714104018199](assets/Windows%20%E5%AE%89%E8%A3%85%20Rust/image-20230714104018199.png)

#### windows 安装 Rust 安装太慢解决办法



1、打开powershell

2、分别执行下面两行代码：

```rust
$ENV:RUSTUP_DIST_SERVER='https://mirrors.ustc.edu.cn/rust-static'
$ENV:RUSTUP_UPDATE_ROOT='https://mirrors.ustc.edu.cn/rust-static/rustup'
```

3、继续在此命令行下执行 rustup-init.exe





### 2、[Microsoft C++ 生成工具](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/) 不安装无法编译



![image-20230714104649977](assets/Windows%20%E5%AE%89%E8%A3%85%20Rust/image-20230714104649977.png)



### 3、MinGW64：https://sourceforge.net/projects/mingw-w64/files/ 

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



### 4、配置 cargo 国内源

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

删除 `.package-cache`

```sh
~/.cargo> rm -rf registry
~/.cargo> rm -rf .package-cache
```



### 5、解决 cargo 堵塞 blocking 问题

如果在运行cargo的时候，出现：<u>Blocking waiting for file lock on package
cache</u>

请产生.cargo文件夹下面的.package_cache文件：

```text
rm ~/.cargo/.package-cache
```

‍



### 6、安装 VScode

#### 安装 插件

rust-analyzer

Rust Syntax

Rust Flash Snippets

Even Better TOML

Error Lens

crates

CMake

CMake Tools

C/C++ Extension Pack



#### Windows rust debug

创建 `.vscode/launch.json`

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



### 参考文档

https://learnblockchain.cn/article/1069

https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index.git/

https://rsproxy.cn/

[安装 Rust - Rust 程序设计语言 (rust-lang.org)](https://www.rust-lang.org/zh-CN/tools/install)

[Windows安装Rust指南 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv17841257)
