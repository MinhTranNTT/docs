# Docker swarm

[Docker Swarm 介绍和工作原理][https://blog.csdn.net/qq1010267837/article/details/125003810](https://blog.csdn.net/qq1010267837/article/details/125003810)

## 1. 初始化一个节点 `docker swarm init`​（docker-1）

```sh
[root@localhost ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
6090f5e5a6ee   bridge    bridge    local
ea337009fb83   host      host      local
e4d1ce606183   none      null      local
[root@localhost ~]# docker swarm -h
Flag shorthand -h has been deprecated, please use --help

Usage:  docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm # 初始化一个集群
  join        Join a swarm as a node and/or manager # 以节点的身份加入一个蜂群并进行管理
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

Run 'docker swarm COMMAND --help' for more information on a command.
[root@localhost ~]# docker swarm init -h
Flag shorthand -h has been deprecated, please use --help

Usage:  docker swarm init [OPTIONS]

Initialize a swarm

Options:
      --advertise-addr string                  Advertised address (format: # 发布地址
                                               <ip|interface>[:port])
      --autolock                               Enable manager autolocking (requiring an
                                               unlock key to start a stopped manager)
      --availability string                    Availability of the node
                                               ("active"|"pause"|"drain") (default "active")
      --cert-expiry duration                   Validity period for node certificates
                                               (ns|us|ms|s|m|h) (default 2160h0m0s)
      --data-path-addr string                  Address or interface to use for data
                                               path traffic (format: <ip|interface>)
      --data-path-port uint32                  Port number to use for data path traffic
                                               (1024 - 49151). If no value is set or is
                                               set to 0, the default port (4789) is used.
      --default-addr-pool ipNetSlice           default address pool in CIDR format
                                               (default [])
      --default-addr-pool-mask-length uint32   default address pool subnet mask length
                                               (default 24)
      --dispatcher-heartbeat duration          Dispatcher heartbeat period
                                               (ns|us|ms|s|m|h) (default 5s)
      --external-ca external-ca                Specifications of one or more
                                               certificate signing endpoints
      --force-new-cluster                      Force create a new cluster from current state
      --listen-addr node-addr                  Listen address (format:
                                               <ip|interface>[:port]) (default 0.0.0.0:2377)
      --max-snapshots uint                     Number of additional Raft snapshots to retain
      --snapshot-interval uint                 Number of log entries between Raft
                                               snapshots (default 10000)
      --task-history-limit int                 Task history retention limit (default 5)
[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:20:74:87 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.102/24 brd 192.168.1.255 scope global noprefixroute dynamic ens33
       valid_lft 6123sec preferred_lft 6123sec
    inet6 fe80::686a:cd80:2b45:7881/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:a7:c7:42:08 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:a7ff:fec7:4208/64 scope link 
       valid_lft forever preferred_lft forever
5: veth5ea21b5@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether c6:73:7a:3c:f6:6d brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::c473:7aff:fe3c:f66d/64 scope link 
       valid_lft forever preferred_lft forever
[root@localhost ~]# docker swarm init --advertise-addr 192.168.1.102
Swarm initialized: current node (lxry278x1aixiyahh6lu6zfxx) is now a manager.

To add a worker to this swarm, run the following command:
    # 使用以下指令在其他节点中执行，加入到当前集群中
    docker swarm join --token SWMTKN-1-3tubipd5uic1iklj44smzgc3zaqtizrp7p1llvac4p1yxzf67b-afuzei64x988refyd7gmcxdh9 192.168.1.102:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

[root@localhost ~]# 
```

## 2. 生成 docker 加入集群命令 `docker swarm join-token worker`​(docker-1)

```sh
[root@localhost ~]# docker swarm join-token worker # 生成 docker 加入集群命令
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3tubipd5uic1iklj44smzgc3zaqtizrp7p1llvac4p1yxzf67b-afuzei64x988refyd7gmcxdh9 192.168.1.102:2377

[root@localhost ~]# 
```

## 3. 生成一个设置主节点命令`docker swarm join-token manager`​(docker-1)

```
[root@localhost ~]# docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3tubipd5uic1iklj44smzgc3zaqtizrp7p1llvac4p1yxzf67b-9k0nke0pk02o3ing6nbzingh4 192.168.1.102:2377

[root@localhost ~]# 

```

## 4. 加入一个节点 `docker swarm join`​（docker-2）

​![image](assets/image-20230221223557-gcjv38i.png)  
​![image](assets/image-20230221223603-52gqcek.png)  
​![image](assets/image-20230221223607-vgfc6av.png)​

‍

‍
