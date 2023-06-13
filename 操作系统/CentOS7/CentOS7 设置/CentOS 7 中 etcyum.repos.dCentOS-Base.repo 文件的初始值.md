以下是 CentOS 7 中 `/etc/yum.repos.d/CentOS-Base.repo` 文件的初始值：

```sh
# CentOS-Base.repo
#
# 该文件包含了 CentOS-Base 源的基本信息，用于 yum 工具的软件包安装。
#
# 安装此文件可启用 CentOS-Base 源，并且默认情况下启用了所有仓库。
# 

# [base] 仓库是极为基本的组件，包含了 CentOS 发行版中一定需要的软件包。
# 可能会发生修改，所以不要使用缓存。
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

# [updates] 仓库包含了补丁或者错误修正的软件包，用于更新基本组件。
# 可能会发生修改，所以不要使用缓存。
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

# [extras] 仓库包含了一些 CentOS 发行版之外的软件包。
# 可能会发生修改，所以不要使用缓存。
[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

# [centosplus] 仓库包含了一些补充的软件包。
# 该仓库并不是由 CentOS 官方提供的。
# 可能会发生修改，所以不要使用缓存。
[centosplus]
name=CentOS-$releasever - Plus
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

# [contrib] 仓库提供了一些非常有用的软件包，但并不被 CentOS 发行版认为是必须的。
# 该仓库并不是由 CentOS 官方提供的。
# 可能会发生修改，所以不要使用缓存。
[contrib]
name=CentOS-$releasever - Contrib
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

```

