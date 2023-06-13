# Debian6 设置 static ip

```
root@liuzonglin:~# vim /etc/network/interfaces		# 配置网卡信息

# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback

auto enp4s		# 网卡名称
iface enp4s inet static		# 静态
address 192.168.31.127		# 配置ip
netmask 255.255.192.0		# 配置网关
gateway 192.168.1.1		# 配置网关
root@liuzonglin:~# /etc/init.d/networking restart		# 重启网卡服务
root@liuzonglin:~# vim /etc/resolv.conf		# 配置DNS
nameserver 114.114.114.114
```

‍
