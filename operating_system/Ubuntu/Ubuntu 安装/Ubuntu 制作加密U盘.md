# Ubuntu 制作加密U盘

注：加密后U盘只能在 Linux 系统上使用！

## 加密U盘之前需做的准备

1. 一个普通的空U盘
2. 在 Windows 上安装 VMware

阿里云盘：

「VMware-workstation-full-16.1.2-17966106」永久有效下载链接：[https://www.aliyundrive.com/s/KUN9Ei65fmE](https://www.aliyundrive.com/s/KUN9Ei65fmE)

官网下载地址：

[https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)

下载注意版本号，不然后面无法激活！！！

3. 需要安装 ubuntu 操作系统镜像包

ubuntu-20.04.3-desktop-amd64.iso （*其他版本也可以！！！）

官网下载地址：

https://ubuntu.com/download/desktop

__大概需要15分钟

### 一、安装 VMware

‍

### 二、安装 ubuntu 操作系统



#### 设置中文



### 三、安装

对着桌面鼠标右击

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image050-20230208193511-te1ri1l.gif)​

sudo su -  按 Enter

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image052-20230208193511-xoc3b85.gif)​

输入 root 密码 按 Enter

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image054-20230208193511-vg93vf8.gif)​

切换到 root 用户

sudo apt-get install -y cryptsetup

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image056-20230208193511-j3gh1rv.gif)​

![](assets/clip_image058-20230208193511-prtrw1z.gif)​

安装完成！！！

插入U盘

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image060-20230208193511-3fyo91h.gif)​

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image062-20230208193511-vidmg0z.gif)​

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image064-20230208193511-q9eshzs.gif)​

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image066-20230208193511-duwvxnh.gif)​

Next ：下一步

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image068-20230208193511-m0ar4g9.gif)​

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image070-20230208193511-96te71l.gif)​

等待一会

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image072-20230208193511-qwa0o4d.gif)​

开锁！

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image074-20230208193511-jq2h37l.gif)​

![](assets/Ubuntu%20%E5%88%B6%E4%BD%9C%E5%8A%A0%E5%AF%86U%E7%9B%98/clip_image076-20230208193511-lvg4io5.gif)​

其他机器使用，只能支持 linux 系统 拔掉U盘插入会需要输入密码。。输入密码可以永久保存密码，也可以零时设置临时密码。这个看个人使用！！！
