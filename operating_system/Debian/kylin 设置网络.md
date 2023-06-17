# kylin 设置网络

```sh
chmod +x /etc/rc.d/rc.local		# 授权重启
nohup /usr/local/las/program/network/bootup.sh & >> /etc/rc.d/rc.local		# 开机自启
```

```sh
root@liuzonglin:~# cat bootup.sh 
#!/bin/bash

# /etc/resolv.conf     nameserver 114.114.114.114
echo "nameserver 114.114.114.114" > /etc/resolv.conf
sudo systemctl daemon-reload
sudo systemctl restart systemd-resolved
iptables -F
iptables -X
root@liuzonglin:~#
```

‍
