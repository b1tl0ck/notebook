---
tags:
  - env
create_time: 2024-10-09 10:46
modified_time: 2024-10-09 10:46
status: complete
---
Sudo apt upgrade
Sudo apt install vim


## 安装必要环境
将一下内容写入一个 sh 脚本文件中，为 chmod 777 xxx. Sh 文件赋权限后直接./xxx. Sh 运行脚本 (python 2 部分网络原因，最后逐一执行了。)

```bash
#!/bin/bash
cd ~
sudo apt install tzdata
sudo apt install vim
sudo apt install libxml2-dev
sudo apt install libxslt-dev
sudo apt install libmysqlclient-dev
sudo apt install libsqlite3-dev
sudo apt install zlib1g-dev
sudo apt install libffi-dev
sudo apt install libssl-dev
sudo apt install python3-pip
sudo apt install wget
sudo apt install curl
sudo apt install gcc
sudo apt install clang # 安装失败，clang-17替代
sudo apt install make
sudo apt install zip
sudo apt install build-essential
sudo apt install libncursesw5-dev libgdbm-dev libc6-dev
sudo apt install tk-dev
sudo apt install openssl
sudo apt install virtualenv
sudo apt install git
sudo apt install proxychains4
sudo apt install ruby-dev

# 环境基础
sudo apt install -y build-essential checkinstall libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev

# python2安装
wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz
wget http://47.109.207.27:8679/Python-2.7.18.tgz
tar xzf Python-2.7.18.tgz
cd Python-2.7.18
sudo ./configure --enable-optimizations
sudo make altinstall
sudo ln -sfn '/usr/local/bin/python2.7' '/usr/bin/python2'
cd ../
sudo rm -rf Python-2.7.18 Python-2.7.18.tgz

# pip2
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py -O get-pip2.py
wget http://47.109.207.27:8679/get-pip2.py
sudo python2 get-pip2.py
sudo rm -rf get-pip2.py

sudo pip2 install pathlib2



```


```bash
# 下面没用sh脚本，是踩坑记录
# pip3
wget https://bootstrap.pypa.io/get-pip.py
wget http://47.109.207.27:8679/get-pip.py
sudo python3 get-pip.py
sudo rm -rf get-pip.py

sudo python2 -m pip install --upgrade pip
sudo python3 -m pip install --upgrade pip

pip3 install --upgrade pip

# setuptools 36.6.1 -> python2
wget https://mirrors.aliyun.com/pypi/packages/56/a0/4dfcc515b1b993286a64b9ab62562f09e6ed2d09288909aee1efdb9dde16/setuptools-36.6.1.zip
unzip setuptools-36.6.1.zip
cd setuptools-36.6.1
sudo python2 setup.py install
cd ../
sudo rm -rf setuptools-36.6.1 setuptools-36.6.1.zip

# setuptools 65.4.1 -> python3
wget https://mirrors.aliyun.com/pypi/packages/03/c9/7b050ea4cc4144d0328f15e0b43c839e759c6c639370a3b932ecf4c6358f/setuptools-65.4.1.tar.gz
tar -zxvf setuptools-65.4.1.tar.gz
cd setuptools-65.4.1
sudo python3 setup.py install
cd ../
sudo rm -rf setuptools-65.4.1 setuptools-65.4.1.tar.gz


sudo pip2 config set global.index-url https://mirrors.aliyun.com/pypi/simple
sudo pip3 config set global.index-url https://mirrors.aliyun.com/pypi/simple

```

## 安装 pwn 环境

### Pwntools

```bash
sudo python2 -m pip install --upgrade pwntools
# 禁止全局安装，此命令强制安装
sudo pip3 install --break-system-packages pwntools
```

### Pwndbg+pwngdb

```bash
# 可能需要proxychains代理网络，然后pt moudle不用代理再跑一次
# pwndbg
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh

# pwngdb
git clone https://github.com/scwuaptx/Pwngdb.git
cp ~/pwn/tools/Pwngdb/.gdbinit ~/pwn/tools/

vim ~/pwn/tools/.gdbinit
# 注释掉第一行 然后在第二行写入
source ~/pwn/tools/pwndbg/gdbinit.py

# 完整.gdbinit
#source ~/peda/peda.py
source ~/pwn/tools/pwndbg/gdbinit.py
source ~/pwn/tools/Pwngdb/pwngdb.py
source ~/pwn/tools/Pwngdb/angelheap/gdbinit.py
```

### Patchelf

```bash
sudo apt install patchelf
```

### Glibc-all-in-one

```bash
#glibc-all-in-one
git clone https://github.com/matrix1001/glibc-all-in-one.git
cd glibc-all-in-one
python3 update_list
cat list
```

### Ropper

```bash
sudo pip3 install capstone filebytes unicorn keystone-engine ropper
```

### Qemu-system

```bash
sudo apt-get install qemu-system
```

### Ropgadget

```bash
sudo -H python3 -m pip install ROPgadget
```

### One_gadget、seccomp-tools

```bash
sudo gem install one_gadget
# 没找到
sudo gem install seccomp-tools
```

## 其他

```bash
# dash栏点击最小化
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```


Zsh
```bash
sudo apt install zsh
# 设置为默认shell,弹出的设置界面选0
chsh -s /bin/zsh

# 安装插件
# 自动提示
sudo apt install zsh-autosuggestions
# 高亮
sudo apt install zsh-syntax-highlighting
# 启用插件，打开~/.zshrc，添加以下内容
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# 允许使用通配符
set nonomatch

# 使用fzf (fuzzy finder)
sudo apt install fzf
# 在.zshrc添加
source /usr/share/doc/fzf/examples/completion.zsh
source /usr/share/doc/fzf/examples/key-bindings.zsh

```


Starship
```bash
1. 安装
   curl -sS https://starship.rs/install.sh | sh
2. 使用 Add the following to the end of `~/.zshrc`:
   eval "$(starship init zsh)"
3. 配置
   ~/.config/starship.toml
```

