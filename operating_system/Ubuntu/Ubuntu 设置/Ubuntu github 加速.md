```sh
zonglin@zonglin-VirtualBox:~$ nslookup github.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   github.com
Address: 20.205.243.166

zonglin@zonglin-VirtualBox:~$ nslookup github.global.ssl.fastly.net
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   github.global.ssl.fastly.net
Address: 199.59.148.89
Name:   github.global.ssl.fastly.net
Address: 2a03:2880:f11a:83:face:b00c:0:25de
```



修改 hosts 

```sh
127.0.0.1       localhost
127.0.1.1       zonglin-VirtualBox

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
20.205.243.166          github.com
199.59.148.89           github.global.ssl.fastly.net
```



重启网络

```sh
sudo systemctl restart NetworkManager.service
```



参考文档：

https://juejin.cn/post/7171096770250801188#heading-4

https://digitalixy.com/linux/969827.html

