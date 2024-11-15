---
tags:
  - tool
create_time: 2024-05-30 14:17
modified_time: 2024-05-30 14:29
status: complete
---
## 插件
Turbo intruder -> 并发测试
Copy as python requests -> 将请求携程批量测试脚本
Copy as go requests -> 将请求携程批量测试脚本
logger++ -> 加强版history
wsdler -> 跑接口
Sharpener -> 皮肤

Fiora -> 用于验证部分nday
nuclei -> 开源扫描器的官方插件，把请求改成该扫描器的模板
xia_sql -> 用于测试sql注入
xia_yue -> 挖越权漏洞
xia_liao -> 自动生成一些数据便于注册填写
captcha-killer-modified -> 用于识别图片验证码的登录口爆破
knife -> 多种用途(中文字符显示，过滤杂包，常用payload插入等) **导入配置**
burpFakeIP -> 用于测试IP绕过类漏洞
apikit -> api漏洞测试
Hae -> 用于history敏感信息高亮 **导入配置**
Cors CrossDomain Vul Scan -> 用于测试Cors漏洞
JSONP HUNTER -> 用于挖掘jsonpxss和jsonp劫持漏洞
403Byoasser -> 用来绕过403响应

## proxy-浏览器不代理的站点
防止垃圾报文
```
127.0.0.1
::1
localhost
*.firefox.com
*.firefox.com.cn
*.googleapis.com
*.shodan.io
*.rnozilla.org
*.mozilla.net
*.mozilla.com
*.mozilla.org
*.wappalyzer.com
*.alicdn.com
*.bdstatic.com
*.firefoxchina.cn
*.jqueryui.com
*.jquery.com
*.digicert.com
*.g-fox.cn
*.baidu.com
*.globalsign.com
*.rapidssl.com
*.pki.goog
*.bcebos.com
*.gtime.com
*.unpkg.com
*.lencr.org
*.qunar.com/passport/qcodeLogin.jsp
```

## 图片验证码插件

左下角请求接口
```python
POST /reg HTTP/1.1
Host: 127.0.0.1:8888
Authorization:Basic f0ngauth
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:97.0) Gecko/20100101 Firefox/97.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 8332

<@BASE64><@IMG_RAW></@IMG_RAW></@BASE64>
```

随后在本地启动`codereg.py`脚本，运行识别脚本
```python
# -*- coding:utf-8 -*-
# author:f0ngf0ng
# @Date: 2022/3/11 下午1:44
import argparse
import ddddocr                       # 导入 ddddocr
from aiohttp import web
import base64

parser = argparse.ArgumentParser()

parser.add_argument("-p", help="http port",default="8888")
args = parser.parse_args()
ocr = ddddocr.DdddOcr()
port = args.p

auth_base64 = "f0ngauth" # 可自定义auth认证

async def handle_cb(request):
    if request.headers.get('Authorization') != 'Basic ' + auth_base64:  # 可自定义auth认证，不需要注释就好
        return web.Response(text='Forbidden', status='403')
    print(await request.text())
    img_base64 = await request.text()
    img_bytes = base64.b64decode(img_base64)
    # return web.Response(text=ocr.classification(img_bytes)[0:4]) 验证码取前四位
    # return web.Response(text=ocr.classification(img_bytes)[0:4].replace("0","o")) 验证码取前四位、验证码中的0替换为o
    return web.Response(text=ocr.classification(img_bytes)[0:4])

app = web.Application()
app.add_routes([
    web.post('/reg', handle_cb),
])

if __name__ == '__main__':
    web.run_app(app, port=port)
```

