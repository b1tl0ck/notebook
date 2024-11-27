---
tags:
  - dotfile
create_time: 2024-06-03 09:46
modified_time: 2024-06-03 09:46
status: complete
---
## scoop

### 安装scoop

在 PowerShell 中打开远程权限

```powershell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser;
```

自定义 Scoop 安装目录

```powershell
$env:SCOOP='Your_Scoop_Path'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
```

如果跳过该步骤， Scoop 将默认把所有用户安装的 App 和 Scoop 本身置于C:\Users\user_name\scoop
下载并安装 Scoop

```powershell
iwr -useb get.scoop.sh | iex
scoop update
```

上面无法下载的话，可以采用下方的国内镜像

```powershell
iwr -useb https://gitee.com/glsnames/scoop-installer/raw/master/bin/install.ps1 | iex
scoop config SCOOP_REPO 'https://gitee.com/glsnames/scoop-installer'
scoop update
```

### 添加scoop库

官方推荐仓库

```powershell
$ scoop bucket known
main [默认]
extras [墙裂推荐]
versions
nightlies
nirsoft
php
nerd-fonts
nonportable
java
games
jetbrains
```

分别下载git、extras、nerd-fonts等

```powershell
# 安装git
scoop install git
# 添加额外库
scoop bucket add extras
scoop bucket add nerd-fonts
# scoop插件
scoop install scoop-completion
# 安装FiraCode-NF字体
scoop install FiraCode-NF
# 使用 Linux 命令行
scoop install gow
# 调用管理员权限（win11 24H2已自带）
scoop install sudo
# 常用字体
monaco
FiraCode Nerd Font
CaskaydiaCove Nerd Font
```

### Scoop 的管理与配置

```powershell
# 查看已安装程序
scoop list
# 查看更新
scoop status
# 删除旧版本
scoop cleanup
# 自身诊断
scoop checkup
# 搜索应用程序
scoop search <APPname>
# 卸载scoop
scoop uninstall scoop
```


### 整合Scoop

打开Windows terminal，输入以下命令：

```powershell
Import-Module "$($(Get-Item $(Get-Command scoop).Path).Directory.Parent.FullName)\modules\scoop-completion"
Import-Module "$($(Get-Item $(Get-Command scoop).Path).Directory.Parent.FullName)\modules\scoop-completion" -ErrorAction SilentlyContinue
```



## starship

打开powershell，使用 Scoop 安装

```powershell
scoop install starship
```

或者使用winget安装

```powershell
winget install --id Starship.Starship
```

使用VScode打开配置文件

```powershell
code $PROFILE
```

配置文件中添加：

```powershell
Invoke-Expression (&starship init powershell)
# 没有就添加
$ENV:STARSHIP_CONFIG = "C:\Users\zion\.config\starship.toml"
```

starship.toml见[starship](starship.md)


## powershell

PROFILE

```powershell
# 加载starship
Invoke-Expression (&starship init powershell)
# $ENV:STARSHIP_CONFIG = "C:\Users\zion\.config\starship.toml"

Import-Module PSReadLine

# 设置预测文本来源为历史记录
Set-PSReadLineOption -PredictionSource History

# 搜索历史时(↑)移动光标到行未而不是行首
Set-PSReadLineOption -HistorySearchCursorMovesToEnd

# 设置 Ctrl+d 为退出 PowerShell
Set-PSReadlineKeyHandler -Key "Ctrl+d" -Function ViExit

# TAB补全时提供可选择菜单
Set-PSReadLineKeyHandler -Key "Tab" -Function MenuComplete

# 根据历史补全词(一次只补全部分)
Set-PSReadLineKeyHandler -Key "Ctrl+RightArrow" -Function ForwardWord
 
# 设置上下键为搜索历史记录
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward

# 配置系统代理
function proxy {
    $env:HTTP_PROXY="http://127.0.0.1:10809"
    $env:HTTPS_PROXY="http://127.0.0.1:10809"
}
function unproxy {
    $env:HTTP_PROXY=""
    $env:HTTPS_PROXY=""
}

# 定义别名和目录常量
const ToolsDir = "E:\\T00ls"
const ScriptDir = "E:\\Code\\SecScripts"

# web工具
Function dirsearch {& "D:\\CodeEnv\\Python\\Python311\\python.exe" "E:\\T00ls\\Scan\\dirsearch-master\\dirsearch.py" @args}
Function dirsearch403 {& "D:\\CodeEnv\\Python\\Python311\\python.exe" "E:\\T00ls\\Scan\\dirsearch_bypass403\\dirsearch.py" @args}
Function githack {& "D:\\CodeEnv\\Python\\Python311\\python.exe" "E:\\T00ls\\源码泄露\\GitHack-master\\GitHack.py" @args}
function sqlmap {& "D:\\CodeEnv\\Python\\Python311\\python.exe" "E:\\T00ls\\WebExp\\数据库综合\\sqlmap\\sqlmap.py" @args}
```



```powershell
# scoop代理设置
function proxy_scoop {scoop config proxy 127.0.0.1:10809}
function unproxy_scoop {scoop config rm proxy}

# 配置git代理
function unproxygit {git config --global --unset http.proxy && git config --global --unset https.proxy}
function proxygit {git config --global http.proxy socks5://127.0.0.1:10808 && git config --global https.proxy socks5://127.0.0.1:10808}
```




## windows terminal

配色方案

```json
{
	"background": "#282828",
	"black": "#282828",
	"blue": "#458588",
	"brightBlack": "#928374",
	"brightBlue": "#83A598",
	"brightCyan": "#8EC07C",
	"brightGreen": "#B8BB26",
	"brightPurple": "#D3869B",
	"brightRed": "#FB4934",
	"brightWhite": "#EBDBB2",
	"brightYellow": "#FABD2F",
	"cursorColor": "#EBDBB2",
	"cyan": "#689D6A",
	"foreground": "#EBDBB2",
	"green": "#98971A",
	"name": "GruvboxDark",
	"purple": "#B16286",
	"red": "#CC241D",
	"selectionBackground": "#665C54",
	"white": "#A89984",
	"yellow": "#D79921"
}
```

