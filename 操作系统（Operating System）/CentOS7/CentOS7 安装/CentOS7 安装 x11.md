# CentOS7 安装 x11

## 先做了解

- Secure Shell (SSH)是一种加密协议，可以在不安全的网络上安全地传输数据。
- X11- forwarding是一个安全的shell特性，它允许通过现有的SSH shell会话转发X11连接，用于在服务器上运行X11程序，而ssh-client通过用户的X11-server显示图形窗口。

### SSh 与 X11 的区别：

虽然SSH (Secure Shell)允许用户在客户机上远程连接服务器，但是这种Shell访问只允许用户和服务器应用程序之间基于文本的交互。

然而，X11是一个允许服务器应用程序显示图形界面的系统(本质上是基于像素的输出，显示自己的窗口)。这是一个长期建立的协议，但它传输数据没有加密。

X11-forwarding允许通过已经建立和加密的SSH连接安全地运行X11程序。

## 准备工作

操作系统版本：Centos7.9      # 版本随意
windows安装以下软件：
Xming 6.9 Xming下载地址： [https://sourceforge.net/projects/xming/](https://sourceforge.net/projects/xming/)
Xshell 6.0
Putty下载地址：https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230131800620-2036003566.png)

---

以上软件安装好
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230140741652-834941515.png)记录一下，一会会用到

## 打开xshell > 文件 > 默认会话属性

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230141004966-859117087.png)

点击“隧道”- 勾选“转发X11连接到（X）”，选择“X DISPLAY(D)”，后面输入的内容就是之前桌面右下角显示的数字。
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230141227865-161861354.png)

用xshell 连接 centos 并安装 xorg-x11-xauth

```
yum -y install xorg-x11-xauth    # 安装 xorg-x11-xauth
```

装完之后，退出ssh连接，然后重新连接。接着安装图形界面可以使用的软件包测试一下。

```
yum -y install firefox gedit    # 安装试图界面，有图形界面不需用执行
gedit &    	# 使用软件包测试一下
firefox &   # 使用软件包测试一下
```

执行后会在桌面弹出两个图形框
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230141950088-1245834073.png)

使用 putty![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230142119043-426433567.png)

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230142323192-525618146.png)
返回上面的session选项卡，输入服务器地址，远程连接服务器。

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230142458232-2092017181.png)

Open 连接 > 输入用户名 > 输入密码

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230142706309-28544498.png)

完成！

## 参考博客：

[centos7安装图形x11_Centos7 使用ssh进行x11图形界面转发参考](https://blog.csdn.net/weixin_42131601/article/details/113707277?utm_term=centos%E5%AE%89%E8%A3%85x11&utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduweb~default-0-113707277&spm=3001.4430 "centos7安装图形x11_Centos7 使用ssh进行x11图形界面转发参考")
