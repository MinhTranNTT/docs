# Debian6 配置 snmp

1.首先在堡垒机页面上配置，系统设置-连接设置-SNMP设置，输入地址和团体名，如果服务端没有收到信息则执行以下步骤  
2.到堡垒机后台，开通远程访问  
修改主配置文件  
注释掉下面这一行：  
agentAddress udp:127.0.0.1:161  
并把下列行取消注释：  
agentAddress udp:161,udp6:[::1]:161  
并修改为  
agentAddress udp:161  
​![image](assets/Debian6%20%E9%85%8D%E7%BD%AE%20snmp/image-20230302210019-t60uowa.png)​  
在此处添加一条新的接入指令  
​​![image](assets/Debian6%20%E9%85%8D%E7%BD%AE%20snmp/image-20230302210022-a084mcl.png)​  
查看此处的团体名是否正确，默认为public(此处根据用户要求修改，如过用户没有要求，则默认为public)，将原来的systemonly修改为AllView  
​​![image](assets/Debian6%20%E9%85%8D%E7%BD%AE%20snmp/image-20230302210025-81hat96.png)​  
修改完毕后重启SNMP服务 /etc/init.d/snmpd restart

snmpwalk -v 2c -c public localhost     在本机上执行这条命令，查看是否有数据
