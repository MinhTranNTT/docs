# nvm  指令

1. ​`nvm -v`​​，安装成功则显示版本号和列出了各种使用命令。

    ```
    $ nvm -v
    1.1.10
    ```

2. ​`nvm ls`​​列出所有已经安装的Node版本

    ```
    $ nvm ls

      * 14.21.1 (Currently using 64-bit executable)
        14.19.3
        10.16.3
    ```

3. 安装最新版 Node

    ```
    $ nvm install node
    ```

4. ​`nvm list available`​​列出所有可以安装的Node版本号

    ```
    $ nvm list available

    |   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
    |--------------|--------------|--------------|--------------|
    |    19.6.1    |   18.14.1    |   0.12.18    |   0.11.16    |
    |    19.6.0    |   18.14.0    |   0.12.17    |   0.11.15    |
    |    19.5.0    |   18.13.0    |   0.12.16    |   0.11.14    |
    |    19.4.0    |   18.12.1    |   0.12.15    |   0.11.13    |
    |    19.3.0    |   18.12.0    |   0.12.14    |   0.11.12    |
    |    19.2.0    |   16.19.1    |   0.12.13    |   0.11.11    |
    |    19.1.0    |   16.19.0    |   0.12.12    |   0.11.10    |
    |    19.0.1    |   16.18.1    |   0.12.11    |    0.11.9    |
    |    19.0.0    |   16.18.0    |   0.12.10    |    0.11.8    |
    |   18.11.0    |   16.17.1    |    0.12.9    |    0.11.7    |
    |   18.10.0    |   16.17.0    |    0.12.8    |    0.11.6    |
    |    18.9.1    |   16.16.0    |    0.12.7    |    0.11.5    |
    |    18.9.0    |   16.15.1    |    0.12.6    |    0.11.4    |
    |    18.8.0    |   16.15.0    |    0.12.5    |    0.11.3    |
    |    18.7.0    |   16.14.2    |    0.12.4    |    0.11.2    |
    |    18.6.0    |   16.14.1    |    0.12.3    |    0.11.1    |
    |    18.5.0    |   16.14.0    |    0.12.2    |    0.11.0    |
    |    18.4.0    |   16.13.2    |    0.12.1    |    0.9.12    |
    |    18.3.0    |   16.13.1    |    0.12.0    |    0.9.11    |
    |    18.2.0    |   16.13.0    |   0.10.48    |    0.9.10    |

    This is a partial list. For a complete list, visit https://nodejs.org/en/download/releases
    ```

5. ​`nvm install <版本号>`​​安装指定版本号的Node

    ```
    $ nvm install 18.14.1
    Downloading node.js version 18.14.1 (64-bit)... 
    Extracting node and npm...
    Complete
    npm v9.3.1 installed successfully.


    Installation complete. If you want to use this version, type

    nvm use 18.14.1
    ```

6. ​`nvm use <版本号>`​​使用特定版本的Node

    ```
    $ nvm use 18.14.1
    Now using node v18.14.1 (64-bit)
    ```

7. ​`nvm uninstall <版本号>`​​卸载版本号的Node

    ```
    $ nvm uninstall 10.16.3
    ```

8. 查看 node 版本

    ```
    $ node -v
    v18.14.1
    ```
    