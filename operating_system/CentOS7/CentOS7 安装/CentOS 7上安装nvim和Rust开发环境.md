# CentOS 7上安装nvim和Rust开发环境

1. 安装nvim

首先，需要确保你的系统中已经安装了所需要的依赖工具和库。请确保你的系统已经安装好了以下依赖：

```sh
sudo yum install -y git automake gcc gcc-c++ kernel-devel cmake python3-devel libtool libtool-ltdl-devel
```

2. 在终端中执行以下命令，下载nvim:

```sh
cd ~
git clone https://github.com/neovim/neovim.git
```

3. 安装Rust

您可以使用rustup安装Rust。首先，下载rustup-init.sh文件：

```sh
curl https://sh.rustup.rs -sSf | sh
```

然后，运行脚本以安装rustup和默认的Rust工具链。您可以使用以下命令检查Rust版本：

```sh
rustc --version
```

4. 切换到nvim目录并执行以下命令，编译和安装nvim：

```sh
cd neovim
make CMAKE_BUILD_TYPE=Release
sudo make install
```

错误：软件包版本不一致

```sh
[root@localhost neovim]# make CMAKE_BUILD_TYPE=Release
mkdir -p ".deps"
cd .deps && \
        cmake -G 'Unix Makefiles'   \
         /root/neovim//cmake.deps
-- The C compiler identification is GNU 4.8.5
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Found GNU Make at /usr/bin/gmake
-- CMAKE_BUILD_TYPE not specified, default is 'Debug'
-- Performing Test HAS_OG_FLAG
-- Performing Test HAS_OG_FLAG - Success
-- Found Git: /usr/bin/git (found version "1.8.3.1") 
-- Performing Test HAS_NO_STACK_CHECK
-- Performing Test HAS_NO_STACK_CHECK - Success
-- Configuring done
-- Generating done
-- Build files have been written to: /root/neovim/.deps
mkdir -p build
touch build/.ran-deps-cmake
make -C .deps
make[1]: 进入目录“/root/neovim/.deps”
make[2]: 进入目录“/root/neovim/.deps”
make[3]: 进入目录“/root/neovim/.deps”
make[3]: 离开目录“/root/neovim/.deps”
make[3]: 进入目录“/root/neovim/.deps”
[  1%] Creating directories for 'treesitter-vimdoc'
[  2%] Performing download step (download, verify and extract) for 'treesitter-vimdoc'
-- Downloading...
   dst='/root/neovim/.deps/build/downloads/treesitter-vimdoc/v2.0.0.tar.gz'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/neovim/tree-sitter-vimdoc/archive/v2.0.0.tar.gz'
CMake Error at treesitter-vimdoc-stamp/download-treesitter-vimdoc.cmake:170 (message):
  Each download failed!

    error: downloading 'https://github.com/neovim/tree-sitter-vimdoc/archive/v2.0.0.tar.gz' failed
          status_code: 1
          status_string: "Unsupported protocol"
          log:
          --- LOG BEGIN ---
          Protocol "https" not supported or disabled in libcurl

  Closing connection -1

  

          --- LOG END ---
          
    


make[3]: *** [build/src/treesitter-vimdoc-stamp/treesitter-vimdoc-download] 错误 1
make[3]: 离开目录“/root/neovim/.deps”
make[2]: *** [CMakeFiles/treesitter-vimdoc.dir/all] 错误 2
make[2]: 离开目录“/root/neovim/.deps”
make[1]: *** [all] 错误 2
make[1]: 离开目录“/root/neovim/.deps”
make: *** [deps] 错误 2
```

根据您提供的信息，看起来是在编译安装nvim时遇到了下载错误。具体地，这是在下载tree-sitter-vimdoc包时出现的。

这可能是由于您的系统缺少支持https的依赖库的原因，因此无法下载该包。为了解决这个问题，您可以尝试以下解决方案：

1. 尝试更新curl和openssl库：

   ```sh
   sudo yum update curl openssl
   ```

2. 在编译安装nvim之前，安装支持https的curl依赖库：

   ```sh
   sudo yum install libcurl-devel
   ```

3. 如果上述解决方案不起作用，您可以尝试手动下载tree-sitter-vimdoc包并将其放置在正确的位置。在这种情况下，您需要从 https://github.com/neovim/tree-sitter-vimdoc/releases 下载适当版本的tar.gz文件，并将其解压缩到 `/root/neovim/.deps/build/downloads/treesitter-vimdoc/` 目录下。

   希望这些解决方案可以帮助您解决这个问题。

---

5. 检查nvim是否成功安装，执行以下命令：

```sh
nvim --version
```

如果安装成功，将会显示nvim的版本信息。

希望这些步骤可以帮助您在CentOS 7上编译安装nvim。