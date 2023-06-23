# 升级 CentOS 7

1. 备份数据

在进行任何系统升级之前，请务必备份您的数据以防丢失。建议将数据备份到外部存储介质，如 USB 硬盘、云存储等。

2. 更新已安装的软件包

在进行升级之前，需要先更新已安装的软件包，以保证系统当前状态的稳定性。

```sh
sudo yum update
```

3. 安装 CentOS 7 升级工具

CentOS 7 升级工具可以通过 CentOS 官方源获得。请运行以下命令以安装升级工具：

```sh
sudo yum install centos-release-7-9.2009.1.el7.centos.x86_64 -y
```

4. 安装升级工具的依赖项

运行以下命令以安装升级工具的依赖项：

```sh
sudo yum install \
  deltarpm \
  python2-future \
  python-backports \
  python-six \
  python2-ipaddress \
  python-urllib3 \
  python2-certifi \
  python2-chardet \
  python2-requests \
  -y
```

5. 升级系统

运行以下命令以升级系统：

```sh
sudo yum upgrade -y
```

在升级过程中，按照提示输入 y，确认所有提示。升级完成后，重启系统以应用更新。

```sh
sudo shutdown -r now
```

以上就是 CentOS 7 升级的步骤，升级前请确保备份数据，升级期间请按照提示操作，避免升级失败或数据丢失。



# 查看centos7 版本

要查看 CentOS 7 的版本，有几种方法可以尝试：

1. 使用 lsb_release 命令

lsb_release 命令是 Linux 标准 Base 项目所提供的一个命令工具，用于显示当前 Linux 系统的版本信息。在 CentOS 7 中，可以通过以下命令来使用 lsb_release：

```sh
lsb_release -a
```

该命令将输出包含 CentOS 的版本和信息。

2. 使用 cat 命令

CentOS 7 的版本信息被存储在 /etc/*release 文件中。可以使用 cat 命令来查看该文件的内容，方法如下：

```sh
cat /etc/*release
```

该命令将输出所有与 CentOS 7 版本相关的信息。

3. 使用 hostnamectl 命令

hostnamectl 是一个控制系统主机名和其他 network identity 属性的命令工具，它还可以用于查看系统版本信息。在 CentOS 7 中，可以使用以下命令来使用 hostnamectl：

```sh
hostnamectl
```

该命令将输出系统的 hostname 信息和 CentOS 7 版本。

以上是三种查看 CentOS 7 版本信息的方法，您可以根据自己的需要选择其中任意一种方法。

