# docker Error response from daemon driver failed programming external connectivity

```bash
[root@localhost ~]# docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
b2796013b95cdf82a8bfc29f2caaf46106b09f7cbf1125338008b3f745dd6660
docker: Error response from daemon: driver failed programming external connectivity on endpoint mysql-test (743c48976fe4c4d718ef14a606b1cafd2b76175dbb2451e38f9e8fb462b2b051):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 3306 -j DNAT --to-destination 172.17.0.2:3306 ! -i docker0: iptables: No chain/target/match by that name.
 (exit status 1)).
[root@localhost ~]# docker rm -f $(docker ps -qa)
b2796013b95c
[root@localhost ~]# systemctl restart docker 
[root@localhost ~]# docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
9a141aa3c4ce233500701e9d581c063f83158285ea7b6cd817f07cab2c7245aa
```

原因：在我们启动了Docker后，我们再对防火墙firewalld进行操作，就会发生上述报错
详细原因：docker服务启动时定义的自定义链DOCKER，当 centos7 firewall 被清掉时，

firewall的底层是使用iptables进行数据过滤，建立在iptables之上，这可能会与 Docker 产生冲突。

当 firewalld 启动或者重启的时候，将会从 iptables 中移除 DOCKER 的规则，从而影响了 Docker 的正常工作。

当你使用的是 Systemd 的时候， firewalld 会在 Docker 之前启动，但是如果你在 Docker 启动之后操作 firewalld ，你就需要重启 Docker 进程了。

解决方法：输入指令  systemctl restart docker     重启docker服务及可重新生成自定义链DOCKER
————————————————
版权声明：本文为CSDN博主「shz_123」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：[https://blog.csdn.net/shz_123/article/details/123023614](https://blog.csdn.net/shz_123/article/details/123023614)

‍
