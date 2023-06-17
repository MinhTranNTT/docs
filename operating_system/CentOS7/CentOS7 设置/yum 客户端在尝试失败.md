```sh
[root@localhost p2p]# yum update
```

从错误信息来看，yum 客户端在尝试从 `https://cmake.org/files/LatestRelease/centos7-x86_64/` 这个仓库中获取软件包源码时出现了 `HTTPS Error 404 - Not Found` 的错误。

这可能有几种原因导致，比如：

- 仓库地址或者软件包在仓库中已经不存在了
- 网络问题，导致无法连接到该仓库

为了解决这个问题，有以下几种方法可以尝试：

1. 确认该仓库的地址是否正确，或者该仓库是否已经停止服务。如果是这种情况，建议联系仓库管理员解决问题。
2. 修改 yum 配置，临时禁用该仓库。可以使用 `yum --disablerepo=cmake.org_files_LatestRelease_centos7-x86_64_` 命令来临时禁用该仓库，然后再尝试执行更新命令。
3. 如果该仓库已经无法使用，也可以直接永久禁用该仓库。可以使用 `yum-config-manager --disable cmake.org_files_LatestRelease_centos7-x86_64_` 命令来禁用该仓库，然后再重新执行更新命令。