### Metasploit

Ubuntu下允许root用户ssh远程登录

```bash
└─# sudo service postgresql start && msfdb init && msfconsole
```

**工作区**

```bash
msf6 > workspace -h
Usage:
    workspace          List workspaces
    workspace [name]   Switch workspace

OPTIONS:

    -a, --add <name>          添加工作空间。
    -d, --delete <name>       删除工作空间。
    -D, --delete-all          删除所有工作区。
    -h, --help                Help banner。
    -l, --list                工作区列表。
    -r, --rename <old> <new>  重命名工作区。
    -S, --search <name>       搜索工作空间。
    -v, --list-verbose        详细列出工作空间。
```

**配置PostgreSQL数据库**

Metasploit的一个重要特性是支持PostgreSQL数据库，使用它来存储渗透测试结果和漏洞信息。

**准备工作**

启动服务，然后使用 Metasploit msfdb 初始化数据库

1、启动数据库

```text
└─# systemctl start postgresql
```

2、初始化数据库

```text
└─# msfdb init
Creating database user 'msf'
Enter password for new role:
Enter it again:
Creating databases 'msf' and 'msf_test'
Creating configuration file in /usr/share/metasploit-framework/config/database.yml
Creating initial database schema 
```

msfdb 还可以用来管理Metasploit Framework数据库

```text
└─# msfdb                                          
Manage the metasploit framework database                      
  msfdb init     # start and initialize the database          
  msfdb reinit   # delete and reinitialize the database        
  msfdb delete   # delete database and stop using it          
  msfdb start    # start the database                          
  msfdb stop     # stop the database                          
  msfdb status   # check service status                        
  msfdb run      # start the database and run msfconsole      
```

3、修改数据库配置文件

```text
└─# cat /usr/share/metasploit-framework/config/database.yml  
development:                                                         
  adapter: postgresql                                                
  database: msf                                                      
  username: msf                                                      
  password: 9JHbuu/CdoGT0kvBiSXf+VLDRQ9dKKpMYyWKY6Ui2jc=             
  host: localhost                                                    
  port: 5432                                                         
  pool: 5                                                            
  timeout: 5                                                         
production:                                                          
  adapter: postgresql                                                
  database: msf                                                      
  username: msf                                                      
  password: 9JHbuu/CdoGT0kvBiSXf+VLDRQ9dKKpMYyWKY6Ui2jc=             
  host: localhost                                                    
  port: 5432                                                         
  pool: 5                                                            
  timeout: 5                                                         
test:                                                                
  adapter: postgresql                                                
  database: msf_test                                                 
  username: msf                                                      
  password: 9JHbuu/CdoGT0kvBiSXf+VLDRQ9dKKpMYyWKY6Ui2jc=             
  host: localhost                                                    
  port: 5432                                                         
  pool: 5                                                            
  timeout: 5                                                         
```

4、确定是否连接到数据库

启动msfconsole，然后执行db_status，检查数据库连接情况。

```text
msf6 > db_status
[*] postgresql connected to msf
msf >
```

如果要手动连接到数据库，可以使用如下命令：

```text
db_connect <user:pass>@<host:port>/<database>
```

我们可以使用databse.yml文件测试db_connect命令

```text
msf6 > db_disconnect // 断开连接
msf6 > db_status // 查看连接状态
[*] postgresql selected, no connection
msf6 > msf > db_connect
[*]    Usage: db_connect <user:pass>@<host:port>/<database>
[*]       OR: db_connect -y [path/to/database.yml]
[*] Examples:
[*]        db_connect user@metasploit3
[*]        db_connect user:pass@192.168.0.2/metasploit3
[*]        db_connect user:pass@192.168.0.2:1500/metasploit3
msf6 > db_connect -y /usr/share/metasploit-framework/config/database.yml // 连接数据库
[*] Rebuilding the module cache in the background...
msf6 > db_status // 查看连接状态
[*] postgresql connected to msf
msf >
```

‍

### 参考文档

[Metasploit快速入门（一） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/62429064)

[Home | Metasploit Documentation Penetration Testing Software, Pen Testing Security](https://docs.metasploit.com/)

[第1天-3_msf攻击永恒之蓝_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV163411972n?p=3&spm_id_from=pageDriver&vd_source=9bfc54d2ed901f1eab04708cc346c2f5)
