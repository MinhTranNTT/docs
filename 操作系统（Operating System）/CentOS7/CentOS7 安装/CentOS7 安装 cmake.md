# CentOS 7上安装CMake

1. 确保您的系统已更新：

```sh
sudo yum update
```

2. 安装CMake：

```sh
sudo yum install cmake
```

3. 等待安装完成后，您可以验证CMake是否已正确安装：

```sh
cmake --version
```

# CMake添加到环境变量中

1. 打开您的`.bashrc`文件：

```sh
nano ~/.bashrc
```

2. 在文件末尾添加以下行：

```sh
export PATH="/usr/bin/cmake/bin:$PATH"
```

注意：如果CMake安装到不同的位置，则需要相应地更改上述路径。



查看CMake的安装位置

```sh
which cmake
```

该命令将输出CMake的安装路径。如果CMake已成功安装，应该会输出类似于以下内容的路径：

```sh
/usr/bin/cmake
```

3. 保存并关闭`.bashrc`文件。

4. 要使更改生效，请运行以下命令：

```sh
source ~/.bashrc
```

5. 您现在可以验证CMake是否已添加到您的环境变量中：

```sh
cmake --version
```

这将输出CMake的版本信息。

# CMake版本不够

```sh
[root@localhost p2p]# cargo build
   Compiling prost-build v0.10.4
error: failed to run custom build command for `prost-build v0.10.4`

Caused by:
  process didn't exit successfully: `/root/p2p/target/debug/build/prost-build-15519ea4ece2e850/build-script-build` (exit status: 101)
  --- stdout
  cargo:rerun-if-changed=/root/.cargo/registry/src/github.com-1ecc6299db9ec823/prost-build-0.10.4/third-party/protobuf/cmake
  CMAKE_TOOLCHAIN_FILE_x86_64-unknown-linux-gnu = None
  CMAKE_TOOLCHAIN_FILE_x86_64_unknown_linux_gnu = None
  HOST_CMAKE_TOOLCHAIN_FILE = None
  CMAKE_TOOLCHAIN_FILE = None
  CMAKE_GENERATOR_x86_64-unknown-linux-gnu = None
  CMAKE_GENERATOR_x86_64_unknown_linux_gnu = None
  HOST_CMAKE_GENERATOR = None
  CMAKE_GENERATOR = None
  CMAKE_PREFIX_PATH_x86_64-unknown-linux-gnu = None
  CMAKE_PREFIX_PATH_x86_64_unknown_linux_gnu = None
  HOST_CMAKE_PREFIX_PATH = None
  CMAKE_PREFIX_PATH = None
  CMAKE_x86_64-unknown-linux-gnu = None
  CMAKE_x86_64_unknown_linux_gnu = None
  HOST_CMAKE = None
  CMAKE = None
  running: cd "/root/p2p/target/debug/build/prost-build-846b4b5e07079cc9/out/build" && CMAKE_PREFIX_PATH="" "cmake" "/root/.cargo/registry/src/github.com-1ecc6299db9ec823/prost-build-0.10.4/third-party/protobuf/cmake" "-Dprotobuf_BUILD_TESTS=OFF" "-DCMAKE_INSTALL_PREFIX=/root/p2p/target/debug/build/prost-build-846b4b5e07079cc9/out" "-DCMAKE_C_FLAGS= -ffunction-sections -fdata-sections -fPIC -m64" "-DCMAKE_C_COMPILER=/usr/bin/cc" "-DCMAKE_CXX_FLAGS= -ffunction-sections -fdata-sections -fPIC -m64" "-DCMAKE_CXX_COMPILER=/usr/bin/c++" "-DCMAKE_ASM_FLAGS= -ffunction-sections -fdata-sections -fPIC -m64" "-DCMAKE_ASM_COMPILER=/usr/bin/cc" "-DCMAKE_BUILD_TYPE=Debug"
  -- Configuring incomplete, errors occurred!

  --- stderr
  CMake Error at CMakeLists.txt:2 (cmake_minimum_required):
    CMake 3.1.3 or higher is required.  You are running version 2.8.12.2


  thread 'main' panicked at '
  command did not execute successfully, got: exit status: 1

  build script failed, must exit now', /root/.cargo/registry/src/github.com-1ecc6299db9ec823/cmake-0.1.50/src/lib.rs:1098:5
  note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

这个错误提示显示CMake版本不够，需要至少3.1.3版本，但是您的系统中CMake版本只有2.8.12.2。

要在本地编译 cmake，您可以按照以下步骤进行操作：

1. 安装必要的依赖项，包括编译器、make 工具、ncurses 开发包等。在 CentOS 7 上，可以使用以下命令安装：

```sh
yum install gcc gcc-c++ make ncurses-devel
```

2. 下载 cmake 的源代码包，并解压到本地目录。

在官方网站上可以找到最新的版本：https://cmake.org/download/

```sh
wget https://cmake.org/files/v3.21/cmake-3.21.3.tar.gz
tar -zxvf cmake-3.21.3.tar.gz
```

3. 进入解压后的目录，并运行以下命令进行编译安装：

```sh
cd cmake-3.21.3
./bootstrap
make
sudo make install
```

`./bootstrap` 命令会检查系统环境，并生成 Makefile 文件；`make` 命令会编译源代码；`sudo make install` 命令会将编译好的二进制文件安装到系统中。

问题：

```sh
-- Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR (missing: OPENSSL_CRYPTO_LIBRARY OPENSSL_INCLUDE_DIR) 
CMake Error at Utilities/cmcurl/CMakeLists.txt:525 (message):
  Could not find OpenSSL.  Install an OpenSSL development package or
  configure CMake with -DCMAKE_USE_OPENSSL=OFF to build without OpenSSL.


-- Configuring incomplete, errors occurred!
See also "/root/p2p/cmake-3.21.3/CMakeFiles/CMakeOutput.log".
See also "/root/p2p/cmake-3.21.3/CMakeFiles/CMakeError.log".
---------------------------------------------
Error when bootstrapping CMake:
Problem while running initial CMake
---------------------------------------------
```

这个错误提示是在编译 CMake 时出现的，是因为编译 CMake 所需的 OpenSSL 库未能被找到。

解决此问题的步骤如下：

1. 检查 OpenSSL 是否已安装

   在命令行中输入以下命令，检查 OpenSSL 是否已安装：

```sh
openssl version
```

如果 OpenSSL 尚未安装，你可以使用以下命令在 CentOS 7 上安装：

```sh
yum install openssl-devel
```

2. 手动设置 OpenSSL 路径

如果 OpenSSL 已安装但仍出现错误，则需要手动设置 OpenSSL 的路径。可以尝试运行以下命令来设置 OpenSSL 路径：

```sh
cmake -DOPENSSL_ROOT_DIR=/usr/lib64/openssl-1.0
```

如果你的 OpenSSL 安装路径与上述路径不同，可以根据实际情况进行修改。

3. 使用 -DCMAKE_USE_OPENSSL=OFF 参数

如果无法找到 OpenSSL 库，也可以尝试使用 `-DCMAKE_USE_OPENSSL=OFF` 参数来关闭 OpenSSL 支持。在编译 CMake 时使用以下命令：

```sh
./bootstrap -- -DCMAKE_USE_OPENSSL=OFF
```

然后再执行 `make` 命令重新编译即可。

---



4. 安装完成后，可以使用以下命令验证 cmake 是否正确安装：

```sh
cmake --version
```

如果安装成功，该命令会显示 cmake 的版本信息。

注意：在执行 `make` 命令之前，可以使用 `make -j4` 等命令来指定编译使用的 CPU 核心数，以加快编译速度。