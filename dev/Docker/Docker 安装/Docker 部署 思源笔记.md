# Docker 部署 思源笔记

参考文档：

[思源笔记群辉 docker 安装简略版（新手向） - 链滴 (ld246.com)](https://ld246.com/article/1627285006996)

[思源笔记docker部署 - 建站教程 (jiuaidu.com)](https://jiuaidu.com/jianzhan/936357/)

‍

1. **拉取镜像**

    ```bash
    docker pull b3log/siyuan
    ```

2. **运行**

    ```bash
    docker run 
    	--name siyuan -it -d  
    	--restart=always 
    	-v /usr/local/software/siyuan/data/SiYuan:/root/Documents/SiYuan 
    	-p 6806:6806 b3log/siyuan
    ```

3. **访问路径**

    ​`http://192.168.0.104:6806`​

‍

‍
