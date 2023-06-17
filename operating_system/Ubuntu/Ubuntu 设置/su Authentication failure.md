# su: Authentication failure

参考资料：

[Ubuntu下postgresql出现Authentication failure（认证失败）](https://www.cnblogs.com/liuyanerfly/p/13427361.html#:~:text=%E5%BD%93%E6%88%91%E4%BB%AC%E6%83%B3%E5%9C%A8%E5%88%9A%E5%AE%89%E8%A3%85,%E5%88%B0root%E7%94%A8%E6%88%B7%E3%80%82)



```shell
zonglin@zonglin-virtual-machine:~/Desktop$ su
Password: 
su: Authentication failure
zonglin@zonglin-virtual-machine:~/Desktop$ sudo passwd root
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
zonglin@zonglin-virtual-machine:~/Desktop$ su root
Password: 
su: Authentication failure
zonglin@zonglin-virtual-machine:~/Desktop$ su
Password: 
root@zonglin-virtual-machine:/home/zonglin/Desktop# ls
root@zonglin-virtual-machine:/home/zonglin/Desktop# 
root@zonglin-virtual-machine:/home/zonglin/Desktop# 
root@zonglin-virtual-machine:/home/zonglin/Desktop# 
```

使用passwd修改密码

```shell
sudo passwd root	# 修改root 密码
```

使用 su 切换用户

‍
