# CentOS7 openssh升级后导致旧版本不可用

```
echo 'PubkeyAcceptedKeyTypes ssh-ed25519,ssh-rsa,rsa-sha2-256,rsa-sha2-512' >> /etc/ssh/sshd_config		# 把旧版本的加密算法让它重新生效
/etc/init.d/sshd restart	# 重启 ssh服务
```

‍
