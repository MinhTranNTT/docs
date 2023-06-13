# Error response from daemon Cannot restart container 2d3039aab086 driver failed programming external connectivity on endpoint elasticsearch

```
[root@localhost config]# docker restart 2d3039aab086
Error response from daemon: Cannot restart container 2d3039aab086: driver failed programming external connectivity on endpoint elasticsearch (584b3241a431ae34769c83d46359d04211bbc62d91474ec01b463dc01a07fa62):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 9300 -j DNAT --to-destination 172.17.0.3:9300 ! -i docker0: iptables: No chain/target/match by that name.
 (exit status 1))

```

问题

1. docker0

```
systemctl restart docker # 现已知方法重启docker 服务，方向可以自定义docker 网络尝试一下，
```

‍
