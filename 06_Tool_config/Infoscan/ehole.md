---
tags:
  - 信息收集
create_time: 2024-06-04 10:40
modified_time: 2024-06-04 10:40
status: complete
---
https://github.com/EdgeSecurityTeam/EHole
## 使用

配合[红队中易被攻击的一些重点系统漏洞整理]食用效果更佳：[https://forum.ywhack.com/bountytips.php?Vulnerability](https://forum.ywhack.com/bountytips.php?Vulnerability)

```shell
EHole version: 2.0
Usage: Ehole [-f|-l] [parameter]

Options:
  -f string
        Fofa searches for assets , supports IP and IP segments。(192.168.1.1 | 192.168.1.0/24)
  -ftime string
        fofa timeout (default "10")
  -h    this help
  -json string
        out json
  -l string
        Probe based on local file
  -log string
        Log file name (default "server.log")
  -t string
        thread (default "100")
```

EHole(棱洞)2.0提供了**两种**指纹识别方式，可从本地读取识别，也可以从FOFA进行批量调用API识别(需要FOFA密钥)，同时支持结果JSON格式输出。

**1.本地识别：**

```shell
EHole -l url.txt   //URL地址需带上协议,每行一个
```

**2.FOFA识别:**

注意：从FOFA识别需要配置FOFA 密钥以及邮箱，在config.ini内配置好密钥以及邮箱即可使用。

```shell
EHole -f 192.168.1.1/24  //支持单IP或IP段
```

**3.结果输出：**

```shell
EHole -l url.txt -json export.json  //结果输出至export.json文件
```

## 指纹编写

EHole(棱洞)2.0改变了原有的指纹识别规则，2.0版指纹从外部文件读入，识别方式：

**指纹格式：**

```json
关键字匹配：
{
		"cms": "seeyon",
		"method": "keyword",
		"location": "body",
		"keyword": ["/seeyon/USER-DATA/IMAGES/LOGIN/login.gif"]
}
```

```json
faviconhash匹配：
{
		"cms": "CapRover",
		"method": "faviconhash",
		"location": "body",
		"keyword": ["988422585"]
}
```

1. cms：系统名称
2. method：识别方式 (支持三种识别方式，分别为：keyword、faviconhash、regula)
3. location：位置（指纹识别位置，提供两个位置，一个为body，一个为header）
4. keyword：关键字（favicon图标hash、正则表达式、关键字）

- [!] 注意：keyword支持多关键字匹配，需要所有关键字匹配上才能识别。如：

```json
"keyword": ["sys/ui/extend/theme/default/style/icon.css", "sys/ui/extend/theme/default/style/profile.css"]
```