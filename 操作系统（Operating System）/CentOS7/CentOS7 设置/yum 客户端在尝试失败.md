```sh
[root@localhost p2p]# yum update
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.bupt.edu.cn
base                                                                                                                                                                                                                           | 3.6 kB  00:00:00     
https://cmake.org/files/LatestRelease/centos7-x86_64/repodata/repomd.xml: [Errno 14] HTTPS Error 404 - Not Found
正在尝试其它镜像。
To address this issue please refer to the below wiki article 

https://wiki.centos.org/yum-errors

If above article doesn't help to resolve this issue please use https://bugs.centos.org/.



 One of the configured repositories failed (added from: https://cmake.org/files/LatestRelease/centos7-x86_64/),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=cmake.org_files_LatestRelease_centos7-x86_64_ ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable cmake.org_files_LatestRelease_centos7-x86_64_
        or
            subscription-manager repos --disable=cmake.org_files_LatestRelease_centos7-x86_64_

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=cmake.org_files_LatestRelease_centos7-x86_64_.skip_if_unavailable=true

failure: repodata/repomd.xml from cmake.org_files_LatestRelease_centos7-x86_64_: [Errno 256] No more mirrors to try.
https://cmake.org/files/LatestRelease/centos7-x86_64/repodata/repomd.xml: [Errno 14] HTTPS Error 404 - Not Found
```

从错误信息来看，yum 客户端在尝试从 `https://cmake.org/files/LatestRelease/centos7-x86_64/` 这个仓库中获取软件包源码时出现了 `HTTPS Error 404 - Not Found` 的错误。

这可能有几种原因导致，比如：

- 仓库地址或者软件包在仓库中已经不存在了
- 网络问题，导致无法连接到该仓库

为了解决这个问题，有以下几种方法可以尝试：

1. 确认该仓库的地址是否正确，或者该仓库是否已经停止服务。如果是这种情况，建议联系仓库管理员解决问题。
2. 修改 yum 配置，临时禁用该仓库。可以使用 `yum --disablerepo=cmake.org_files_LatestRelease_centos7-x86_64_` 命令来临时禁用该仓库，然后再尝试执行更新命令。
3. 如果该仓库已经无法使用，也可以直接永久禁用该仓库。可以使用 `yum-config-manager --disable cmake.org_files_LatestRelease_centos7-x86_64_` 命令来禁用该仓库，然后再重新执行更新命令。