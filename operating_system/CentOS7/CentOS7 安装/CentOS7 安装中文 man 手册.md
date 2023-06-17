## 1、更新yum列表

```shell
yum update
```

## 2、查询yuam列表中 中文man的软件包名称

```shell
yum list | grep man.*zh
```

> 执行效果：
>
> 得到类似如下结果，如果没获取请检查yum源的设定保证yum update到最新清单
>
> ```shell
> [root@localhost ~]# yum list | grep man.*zh
> man-pages-zh-CN.noarch                   1.5.2-4.el7                   base
> ```

## 3、安装上面出现的软件包：

```shell
yum -y install man-pages-zh-CN.noarch
```

## 4、编辑当前用户的bash 配置文件，添加一个cman别名命令

```shell
vi .bashrc
alias cman='man -M /usr/share/man/zh_CN'
```

## 5、让当前bash从新载入下配置文件

```shell
source .bashrc
```

## 6、使用举例

```shell
cman ls 可查看中文的ls命令使用手册了。
```

‍

### 参考文档：

[Centos 7 安装中文man手册 (huati365.com)](https://blog.huati365.com/a710d7f4f788d04e)
