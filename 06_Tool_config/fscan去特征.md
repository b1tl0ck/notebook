---
tags:
  - tool
create_time: 2024-06-18 18:34
modified_time: 2024-06-18 18:34
status: complete
---
### fscan关键字去除（用garble bulid就不用这步）

fscan字符串主要出现在github.com/shadow1ng/fscan/Plugins/、github.com/shadow1ng/fscan/WebScan/、github.com/shadow1ng/fscan/common这三个导入包中，还有一处是出现在fscan version:地方。将这三个包，替换成本地导入的方式。

将fscan源码拖入到Sublime中，并打开mian.go，可以看到导入包中存在github.com/shadow1ng/fscan/Plugins、github.com/shadow1ng/fscan/common。在Sublime中进行全局替换，将github.com/shadow1ng/fscan替换为main，替换为你想替换的任意字符都行

在common/flag.go文件中，将fscan version替换为sc version，这里也是替换为你想替换的字符就行。

接下来，在GOPATH下的src目录中，新建一个main目录。并将fscan源码中的common、Plugins、WebScan目录，全部拷贝到新建立的main目录中。


### 配置
利用本文工具内容所造成后果与作者毫不相关，需要下载go

go配置自行搜索
```bash
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go

export GOPROXY=http://mirrors.aliyun.com/goproxy/
go env -w GOPRIVATE="gitlab.intra.knownsec.com/*"
这里需要安装garble进行混淆

go install mvdan.cc/garble@latest
```
`ls -lah ~/go/bin/ | grep garble`
出现这个证明安装成功

`export PATH=$PATH:/root/go/bin/`

0x03 编译混淆

首先克隆源码
`git clone https://github.com/shadow1ng/fscan.git`
使用garble混淆

`garble -literals build main.go`
这个混淆偏移是我一般在使用的

`GOOS=windows GOARCH=amd64 garble -literals -tiny -seed=random build -o appwudi.exe main.go`
或者更好一些的

`garble -tiny -literals -seed=random build -ldflags="-w -s -H windowsgui" -race wudiggnm.go`

```bash
garble(混淆库)：
-tiny             删除额外信息                    
-literals         混淆文字
-seed=random       base64编码的随机种子
go：
-w                 去掉调试信息，不能gdb调试了
-s                 去掉符号表
-H windowsgui     隐藏执行窗口，不占用 cmd 终端。（被查杀率高）
-race             使数据允许竞争检测，编译时改变了生成后的文件特征
，使得杀软无法检测，当然有一天也会失效的。
```

### upx（加壳后，签名后的exe无法运行）
最后upx压缩+加壳
`upx app.exe`

### 签名
一般程序被执行得时候，杀软都会检测是否存在数字签名，所以很有必要再加一层数字签名

我这里选择了一款一直在用的签名劫持软件 sign-sacker

### 去特征

`Mangle_1.2_windows_amd64.exe -I music.exe -M -O music169.exe`

