# powershell

参考文档
- https://www.cnblogs.com/lsgxeva/p/9309576.html
- https://blog.csdn.net/qq_40492048/article/details/118417473
- https://blog.csdn.net/qq_22841387/article/details/123024452


## powershell 指令

- Windows 查看文件 md5

    ```powershell
    certutil -hashfile file MD5 # file : 文件所在的绝对路径
    ```

- Windows 查看文件树状结构

    ```powershell
    tree /f
    ```

- Windows 修改指定用户密码

    ```powershell
    
    net user administrator 1qaz@WSX # net user 当前用户名 设置的密码
    ```

- 指令打开VSCode

    ```powershell
    code .
    ```

- 在 Windows PowerShell 快速打开当前路径所在【文件资源管理器】

    ```powershell
    explorer .
    ```
