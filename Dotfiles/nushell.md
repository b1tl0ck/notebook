---
tags:
  - dotfile
create_time: 2024-10-25 15:49
modified_time: 2024-10-25 15:49
status: complete
---
nushell

1. 配置完，如果出现一跳一跳的，输一个字符跳一行的，请在nushell的配置文件内（config.nu）中修改shell_integration 配置内的 osc133 ，将true改为flase即可，不用去github看了，费劲


2. 改成false就不显示登录的提示信息
   $env.config = {
    show_banner: false  # true or false to enable or disable the welcome banner at startup


```nu
# 启动starship
use ~/.cache/starship/init.nu

# 定义别名和目录常量
const ToolsDir = "E:\\T00ls"
const ScriptDir = "E:\\Code\\SecScripts"

# 设置代理
# $env.HTTP_PROXY = ""
def --env "proxy set" [] {
    load-env { "HTTP_PROXY": "socks5://127.0.0.1:10808", "HTTPS_PROXY": "socks5://127.0.0.1:10808" }
}

proxy set

def --env "proxy unset" [] {
    load-env { "HTTP_PROXY": "", "HTTPS_PROXY": "" }
}

def "proxy check" [] {
    print "Try to check proxy..."
    let resp = (curl -I -s --connect-timeout 2 -m 2 -w "%{http_code}" -o /dev/null https://www.google.com | str trim)

    if $resp == "200" {
        print "Proxy setup succeeded!"
    } else {
        print "Proxy setup failed!"
    }
}

# Web工具
alias sqlmap = D:\CodeEnv\Python\Python311\python.exe $"E:\\T00ls\\WebExp\\数据库综合\\sqlmap\\sqlmap.py"
alias dirsearch = D:\CodeEnv\Python\Python311\python.exe $"E:\\T00ls\\Scan\\dirsearch-master\\dirsearch.py"
alias dirsearch403 = D:\CodeEnv\Python\Python311\python.exe $"E:\\T00ls\\Scan\\dirsearch_bypass403\\dirsearch.py"
alias githack = D:\CodeEnv\Python\Python311\python.exe $"E:\\T00ls\\源码泄露\\GitHack-master\\GitHack.py"
```


