---
tags:
  - 信息收集
create_time: 2024-06-03 17:52
modified_time: 2024-06-03 17:52
status: complete
---
https://github.com/P1kAju/httpx


go安装 --- [[../CodeEnv/go]]


## 安装

```bash
# 直接go安装
go install -v github.com/P1kAju/httpx/cmd/httpx@latest

# git clone后自己构建
git clone https://github.com/P1kAju/httpx.git
cd httpx
go mod tidy
cd cmd/httpx
go build
mv httpx /usr/local/bin/
httpx -version
```

