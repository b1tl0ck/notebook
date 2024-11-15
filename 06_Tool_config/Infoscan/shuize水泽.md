---
tags:
  - 信息收集
create_time: 2024-06-03 17:16
modified_time: 2024-06-03 17:16
status: complete
---
https://github.com/0x727/ShuiZe_0x727
## 安装
为了避免踩坑,建议安装在如下环境中

- 当前用户对该目录有写权限，不然扫描结果无法生成。root权限即可
- Python环境必须是3.7以上，因为使用了异步。建议VPS环境是ubuntu20，默认是python3.8。安装模块的时候切记不要走豆瓣的源
- 在配置文件iniFile/config.ini里加入api（fofa、shodan、github、virustotal）

```bash
chmod 777 build.sh
./build.sh
```

python3 ShuiZe.py -h


## 使用

配置接口：iniFile/config.ini

不开启漏洞扫描（不加参数会默认开启）
`python3 ShuiZe.py -d hackerone.com --justInfoGather=JUSTINFOGATHER`

| 语法                                                       | 功能                         |
| :------------------------------------------------------- | :------------------------- |
| python3 ShuiZe.py -d domain.com                          | 收集单一的根域名资产                 |
| python3 ShuiZe.py --domainFile domain.txt                | 批量跑根域名列表                   |
| python3 ShuiZe.py -c 192.168.1.0,192.168.2.0,192.168.3.0 | 收集C段资产                     |
| python3 ShuiZe.py -f url.txt                             | 对url里的网站漏洞检测               |
| python3 ShuiZe.py --fofaTitle XXX大学                      | 从fofa里收集标题为XXX大学的资产，然后漏洞检测 |
| python3 ShuiZe.py -d domain.com --justInfoGather 1       | 仅信息收集，不检测漏洞                |
| python3 ShuiZe.py -d domain.com --ksubdomain 0           | 不调用ksubdomain爆破子域名         |

