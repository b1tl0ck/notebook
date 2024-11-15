---
tags:
  - env
create_time: 2024-06-04 09:18
modified_time: 2024-06-04 09:18
status: complete
---
## linux

1. 首先我们进入GO官方，查找对应要进行下载到Linux操作系统对应的版本，复制链接地址https://go.dev/dl/
2. 下载
```bash
# 进入目录
cd /usr/local
# 下载go资源目录
wget https://go.dev/dl/go1.22.3.linux-amd64.tar.gz
# 解压文件
tar -zxvf go1.19.4.linux-386.tar.gz
```

3. 创建资源目录
官方建议放在 /home/go 下，创建三个目录：bin（编译后可的[执行文件](https://so.csdn.net/so/search?q=%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6&spm=1001.2101.3001.7020)的存放路径）、pkg（编译包时，生成的.a文件的存放路径）、src（源码路径，一般我们的工程就创建在src下面）
`mkdir -p /home/go/bin /home/go/pkg /home/go/src`

4. 编辑/etc/profile，配置go的环境变量
```bash
vim /etc/profile

# 文件末尾添加
export GOROOT=/usr/local/go
export GOPATH=/home/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# 更新配置文件
source /etc/profile
go version
```



## windows

安装点点点


go中没有项目的说法，只有包,其中有两个重要的路径：GOROOT 和 GOPATH。
GOROOT是gosdk的安装目录，GOPATH是我们的工作空间, 用来存放包的目录，可以设定为自己的位置，以后go项目需要的依赖库都会存放在这个路径下面。


在Path中添加`C:/GO/bin`
新建GOROOT = `C:/GO`
新建GOPATH = 对应路径

GOPATH可以设置多个，多个目录使用; 分号隔开，其中，第一个将会是默认的包目录，使用 go get 下载的包都会在第一个path中的src目录下，使用 goinstall时，在哪个GOPATH中找到了这个包，就会在哪个GOPATH下的bin目录生成可执行文件。

GOPATH是一个路径，用来存放开发中需要用到的代码包和go项目。gopath文件夹下需创建三个子文件夹src、pkg、bin


## GO ENV

```shell
set GO111MODULE=on
# 编译目标的指令集架构为 64位 x86架构
set GOARCH=amd64
# 存放go编译安装可执行二进制文件的地方
set GOBIN=
set GOCACHE=C:\Users\zion\AppData\Local\go-build
set GOENV=C:\Users\zion\AppData\Roaming\go\env
set GOEXE=.exe
set GOEXPERIMENT=
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=C:\Users\zion\go\pkg\mod
set GONOPROXY=
set GONOSUMDB=
# 编译目标系统
set GOOS=windows
set GOPATH=C:\Users\zion\go
set GOPRIVATE=
set GOPROXY=https://goproxy.cn
set GOROOT=D:\CodeEnv\Go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLCHAIN=auto
set GOTOOLDIR=D:\CodeEnv\Go\pkg\tool\windows_amd64
set GOVCS=
set GOVERSION=go1.21.5
set GCCGO=gccgo
set GOAMD64=v1
set AR=ar
set CC=gcc
set CXX=g++
set CGO_ENABLED=0
set GOMOD=NUL
set GOWORK=
set CGO_CFLAGS=-O2 -g
set CGO_CPPFLAGS=
set CGO_CXXFLAGS=-O2 -g
set CGO_FFLAGS=-O2 -g
set CGO_LDFLAGS=-O2 -g
set PKG_CONFIG=pkg-config
set GOGCCFLAGS=-m64 -fno-caret-diagnostics -Qunused-arguments -Wl,--no-gc-sections -fmessage-length=0 -ffile-prefix-map=C:\Users\zion\AppData\Local\Temp\go-build1040615750=/tmp/go-build -gno-record-gcc-switches
```

### GO111MODULE

go module是go官方自带的go依赖管理库，在1.13版本正式推荐使用。默认值为空，即go判断开不开启module的依据是看当前项目的根目录下有没有go.mod文件，当然我们也可手动更改为 on(全部开启)/off(全部不开启，不建议)：

`go env -w GO111MODULE=on`
### GOPROXY

`GOPROXY` 就是设置Golang的全局代理。在下载依赖包的时候，一般是访问github的仓库，国内的环境很容易被墙，所以最好设置一个速度快的代理。`Go`在此版本中`GOPROXY`的默认值为`https://proxy.golang.org`，国内是无法访问的。因此，这里推荐使用七牛云的代理`https://goproxy.cn`

`go env -w GOPROXY=https:``//goproxy``.cn,direct`


### GOPATH

在go 1.11版本之前，`GOPATH`主要用于指定我们的开发工作区(Workspace)，是存放源代码、测试文件、库静态文件、可执行文件的目录。但是自从go 1.11引入module功能之后，`GOPATH`的作用就不大了。目前在`go module`模式下，使用go get下载的外部依赖模块就位于`$GOPATH/pkg/mod`目录下。而因为`GOPATH`的默认值为`C:\Users\%UserName%\go`，所以下载的外部依赖包也就存放在C盘下面。如果怕之后安装的依赖包太多占用C盘空间的话，我们可以更改`GOPATH`到其他位置，比如设置为go的安装目录下面：

` go env -w GOPATH=D:\Golang1.19\gopath


### GOMODCACHE

即存储`go`下载的外部依赖模块文件的目录，默认值为`$GOPATH/pkg/mod`，因此我们一般只需要更改`GOPATH`的值即可，此环境变量的值就会自动做出相应的变动。当然你也可以设置为其他值。

### GOCACHE

此目录存放go项目在构建过程中产生的缓存，默认值为`C:\Users\%UserName%\AppData\Local\go-build`，我们也可以更改为其他值，比如设置到`GOPATH`目录下：

`go env -w GOCACHE=D:\Golang1.19\gopath\go-build-cache


### GOENV

此目录存放go环境变量的值的配置文件，默认值为`C:\Users\%UserName%\AppData\Roaming\go\env`，此值一般不需要更改，并且也不能通过go env的方式来更改此变量值。我们可以来到此目录下，发现有一个env文件，打开就可以看到文件的内容就是我们之前自定义过的go环境变量的值：

### GOROOT

即go语言的安装目录，此值切记不要修改，保持默认值即可。如果乱改了，然后发现无法在控制台中使用go命令了，可以打开上一条中提到的env配置文件，删除自己自定义的值，即可恢复GOROOT为默认值。

### 其他相关命令

```shell
// 打印指定go环境变量的值
go env $variable_name
// 将指定go环境变量的值重置为默认
go env -u $variable_name
```

## 换源

Windows 版本
```bash
 SETX GO111MODULE on    
 go env -w GOPROXY=https://goproxy.cn,direct
 SETX GOPROXY https://goproxy.cn,direct
```

Linux 版本
```bash
echo "export GO111MODULE=on" >> ~/.profile
echo "export GOPROXY=https://goproxy.cn" >> ~/.profile
source ~/.profile
```

