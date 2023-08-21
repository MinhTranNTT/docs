# xshell 常用快捷键

```sh
Tab			# 指令补全文件
Ctrl + Insert		# 复制
Shift + Insert		# 粘贴
Alt + Insert		# 粘贴选择内容
Alt + Ctrl		# 调出 Xftp
```

## 服务器连接

```sh
ssh 192.168.245.132 22		# xshell 连接服务器 格式：ssh ip 端口
exit		# 退出
clear		# 清屏
shutdown -h now		# root关机
shutdown -r now		# root重启
```

## 目录切换

```sh
cd /		# 切换到根目录
cd  		# 切换到/root 目录下
cd ..		# 返回上一级目录
cd -		# 返回上一次目录下
```

## 文件展示

```sh
ls		# 展示当前目录下文件
ls -al		# 展示当前目录下全部文件详细信息
dir
```

‍

## 查看进程端口

```sh
ps -ef | grep tomcat		# 查看 tomcat 进程
ss -lnp | grep 8443		    # 查询是否存在端口
kill 15970		            # 关闭进程
```

## ping 与 telnet

```sh
ping 192.168.245.132		# 检测ip是否通畅
ping www.baidu.com		    # 检测是否可以访问外网
telent 192.168.245.132 22	# 检测 端口是否通畅
```

## touch（创建文件）

```sh
touch liuzonglin.txt		# 创建文件
```

## crontab（定时器）

```sh
crontab -l		# 显示 crontab 文件
crontab -e		# 修改 crontab 文件
crontab -r		# 删除 crontab 文件
```

## chmod（文件授权）

```sh
chmod +x install.sh		# 给文件赋可执行权限
chmod 777  install.sh		# 给文件赋最大权限
chmod 600  install.sh		# 给文件赋可读可写权限
```

```
# 展示当前目录下文件详细信息
ls -l
ll

# 效果一致：切换到 local 目录下
cd /usr/local/
cd /usr/local

mysql> select * from employee\G;		# Linux 查看表数据太乱，改变打印格式

echo命令 – 输出字符串或提取Shell变量的值
echo -n 'liuzonglin'		# 不输出结尾的换行符

sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config		# 修改文件中指定内容

echo \'liu\'		# 内容进行转义，显示单引号
echo "liu" > liu.txt		# 内容写入到文件中
```

## mv

```sh
mv liu liuzonglin		# 修改文件名
mv file /usr/local/		# 移动文件到指定目录下
mv -f -b log.txt liuzonglin/		# 移动文件到指定目录下 -f 直接覆盖，不显示 -b 有相同文件覆盖之前为其创建一个备份
```

## find（文件查找）

```sh
find / -name liuzonglin		# 查找执行文件
# -mtime -n或者 +n     （-n表示文件被更改距现在N天内   +n表示文件更改据现在的N天以前）
find /usr/local/las/program/tomcat7/logs/ -mtime +1  -name '*.log'		
```

## 查看 ip

```sh
ifconfig
ip a
ip addr

netstat -tunlp | grep 4443   # 查询 4443 端口					# 2. 排查端口是否存在
wget 'https://172.16.0.145:52443/management/' --no-check-certificate 		# wget 拉起页面 检查端口通不通！
```

‍

‍
