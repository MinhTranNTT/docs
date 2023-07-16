### 设置自动保存时自动格式化代码

使用 `Ctrl + ,` 打开设置

![image-20230615080913215](assets/VScode%20设置/image-20230615080913215.png)



### 设置连字字体

下载最新的发布文件 https://github.com/tonsky/FiraCode/

在 `Visual Studio Code` 中启用连字字体需要用到两个选项：

```json
"editor.fontFamily": "Fira Code Light, Consolas, Microsoft YaHei",
"editor.fontLigatures": true,

```



### 文件夹里只有一个文件折叠问题，文件折叠问题解决

![image-20230615082047450](assets/VScode%20设置/image-20230615082047450.png)



### 打开一个项目打开新窗口

文件 -> 打开最近的文件 -> `Ctrl + 鼠标左键`​ 即可打开新窗口

![image](assets/VScode%20设置/image-20230221221932-2mzz88y.png)​

‍

### VSCode 连接远程 Linux 开发

安装 vscode

`Remote - SSH` 使用SSH打开远程机器上的任何文件夹，并利用VS Code的完整特性集。

配置本地 `C:\Users\liuzonglin\.ssh\config`

阅读更多关于SSH配置文件的信息：https://linux.die.net/man/5/ssh_config 

`Host` 服务地址 `HostName` 服务名  `User` 服务器用户

```config
Host 192.168.1.100
    HostName 192.168.1.100
    User root
```

![image-20230521103940576](assets/VScode%20设置/image-20230521103940576.png)

![image-20230521104053407](assets/VScode%20设置/image-20230521104053407.png)

![image-20230521104116740](assets/VScode%20设置/image-20230521104116740.png)



### 设置扩展

`Error Lens` 改进错误、警告和其他语言诊断的高亮显示，编辑页显示。

`IntelliJ IDEA Keybindings` IntelliJ IDEA 键绑定的快捷键

`Material Icon Theme` Visual Studio 代码的材质设计图标

`rust-analyzer` Rust 语言支持 Visual Studio Code



### 参考文档：

[你被vscode的这个小特性困扰过吗？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1uG4y1S7Uf/?spm_id_from=333.999.0.0&vd_source=9bfc54d2ed901f1eab04708cc346c2f5)