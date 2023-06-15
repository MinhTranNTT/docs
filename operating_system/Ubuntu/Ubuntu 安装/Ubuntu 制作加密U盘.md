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

windows 上安装 VMware 步骤如下：

​![](assets/clip_image002-20230208193511-lldgrtk.gif)下一步

​![](assets/clip_image004-20230208193511-757lcrd.gif)​

​![](assets/clip_image006-20230208193511-bhgm8m4.gif)​

​![](assets/clip_image008-20230208193511-n6y8fcx.gif)​

​![](assets/clip_image010-20230208193511-izpjkva.gif)​

​![](assets/clip_image012-20230208193511-kkab61i.gif)​

​![](assets/clip_image014-20230208193511-yysja49.gif)​

​![](assets/clip_image016-20230208193511-00kr1xu.gif)​

​![](assets/clip_image018-20230208193511-k5ak18z.gif)完成！！！

‍

### 二、安装 ubuntu 操作系统

​![](assets/clip_image020-20230208193511-pcp2dxj.gif)​

或者 Ctrl + N

​![](assets/clip_image022-20230208193511-lu50ca1.gif)​

​![](assets/clip_image024-20230208193511-wdepxfm.gif)​

​![](assets/clip_image026-20230208193511-jbqsaxq.gif)​

​![](assets/clip_image028-20230208193511-ee12xnd.gif)​

​![](assets/clip_image030-20230208193511-h2vqp2c.gif)​

​![](assets/clip_image032-20230208193511-huz4ilg.gif)​

​![](assets/clip_image034-20230208193511-nnfl5k6.gif)​

​![](assets/clip_image036-20230208193511-6jl6a2q.gif)​

​![](assets/clip_image038-20230208193511-wmmdcbz.gif)​

后等待默认安装

​![](assets/clip_image040-20230208193511-mb31tom.gif)​

大概需要15分钟左右，

​![](assets/clip_image042-20230208193511-owdqtbr.gif)​

​![](assets/clip_image044-20230208193511-5bi64ao.gif)​

#### 设置中文

​![](assets/clip_image046-20230208193511-mtma97q.gif)​

​![](assets/clip_image048-20230208193511-esng60t.gif)​

### 三、安装

对着桌面鼠标右击

​![](assets/clip_image050-20230208193511-te1ri1l.gif)​

sudo su -  按 Enter

​![](assets/clip_image052-20230208193511-xoc3b85.gif)​

输入 root 密码 按 Enter

​![](assets/clip_image054-20230208193511-vg93vf8.gif)​

切换到 root 用户

sudo apt-get install -y cryptsetup

​![](assets/clip_image056-20230208193511-j3gh1rv.gif)​

​![](assets/clip_image058-20230208193511-prtrw1z.gif)​

安装完成！！！

插入U盘

​![](assets/clip_image060-20230208193511-3fyo91h.gif)​

​![](assets/clip_image062-20230208193511-vidmg0z.gif)​

​![](assets/clip_image064-20230208193511-q9eshzs.gif)​

​![](assets/clip_image066-20230208193511-duwvxnh.gif)​

Next ：下一步

​![](assets/clip_image068-20230208193511-m0ar4g9.gif)​

​![](assets/clip_image070-20230208193511-96te71l.gif)​

等待一会

​![](assets/clip_image072-20230208193511-qwa0o4d.gif)​

开锁！

​![](assets/clip_image074-20230208193511-jq2h37l.gif)​

​![](assets/clip_image076-20230208193511-lvg4io5.gif)​

其他机器使用，只能支持 linux 系统 拔掉U盘插入会需要输入密码。。输入密码可以永久保存密码，也可以零时设置临时密码。这个看个人使用！！！
