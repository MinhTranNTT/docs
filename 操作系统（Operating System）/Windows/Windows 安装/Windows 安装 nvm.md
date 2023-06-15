# Windows 安装 nvm

参考资料：

[https://blog.csdn.net/MJOY791270505/article/details/126400205](https://blog.csdn.net/MJOY791270505/article/details/126400205)

[nvm的安装和使用（详细）_nvm安装_深沉影子灰的博客-CSDN博客](https://blog.csdn.net/m0_64697285/article/details/127318141)

---

## 1. nvm是什么?

*<u>node.js version management</u>*，顾名思义是一个 nodejs 的版本管理工具。为了解决 node 各种版本存在不兼容现象，nvm 是让你在同一台机器上安装和切换不同版本的 node 的工具，通过它可以安装和切换不同版本的 nodejs。

## 2. 下载安装(win)

可在点此在github上下载最新版本,本次下载安装的是windows版本。  
nvm-github下载地址*：*​*[https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)*

**1. 选择下载**

* **nvm-setup.zip：**安装版，**推荐使用。**
* **nvm-noinstall.zip：**绿色免安装版，但使用时需进行配置环境变量。

双击nvm-setup.exe文件安装

**2. 安装注意事项**

1. 安装路径最好不要出现**中文和空格**
2. 配置淘宝源 ...\nvm\settings.txt 无法修改下面的权限问题统一解决方式

    ```
    root: D:\program\nvm
    path: D:\program\nodejs
    node_mirror: https://npm.taobao.org/mirrors/node/
    npm_mirror: https://npm.taobao.org/mirrors/npm/
    ```
3. 安装后无法正常下载使用 nodejs 软件安装后没有操作权限  
    ​![img](https://img2022.cnblogs.com/blog/2402369/202211/2402369-20221111003355180-1293555815.png)  
    报错：

    ```cmd
    D:\program>nvm install node
    panic: runtime error: slice bounds out of range [:1] with length 0

    goroutine 1 [running]:
    main.versionNumberFrom({0x1180e0c8, 0x4})
            C:/Users/corey/OneDrive/Documents/workspace/libraries/oss/coreybutler/nvm-windows/src/nvm.go:496 +0x116
    main.getVersion({0x1180e0c8, 0x4}, {0x3a9d26, 0x2}, {0x0, 0x0, 0x0})
            C:/Users/corey/OneDrive/Documents/workspace/libraries/oss/coreybutler/nvm-windows/src/nvm.go:233 +0x367
    main.install({0x1180e0c8, 0x4}, {0x3a9d26, 0x2})
            C:/Users/corey/OneDrive/Documents/workspace/libraries/oss/coreybutler/nvm-windows/src/nvm.go:273 +0xbb
    main.main()
            C:/Users/corey/OneDrive/Documents/workspace/libraries/oss/coreybutler/nvm-windows/src/nvm.go:87 +0xaea

    D:\program>
    D:\program>nvm install 14.19.3
    Downloading node.js version 14.19.3 (64-bit)...
    Error while creating D:\program\nvm\v14.19.3\node64.exe - open D:\program\nvm\v14.19.3\node64.exe: The system cannot find the path specified.
    Could not download node.js v14.19.3 64-bit executable.
    ```

‍
