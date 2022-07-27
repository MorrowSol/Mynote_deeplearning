# linux深度学习常用命令

## conda

### 创建环境

```
conda create -n env_name python=x.x
```

### 复制环境

```
conda create -n BBB --clone AAA
```

### 删除环境

```
conda remove -n env_name --all
```

### 激活环境

```
source/conda activate env_name
```

### 退出环境

```
conda deactivate
```

### 列出所有虚拟环境

```
conda env list
或
conda info -e/env
```

### 迁移环境

```
# 进入环境
conda activate bcy
# 导出 environment.yml 文件：
conda env export > environment.yml
# 注意：如果当前路径已经有了 environment.yml 文件，conda 会重写这个文件

# 重现环境：不需要创建环境会自己创建
# 注：从网上安装的可以重新下载，从文件安装的不会
conda env create -f environment.yml
```

### 修改conda源

```
查看所有配置
conda config --show
添加源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
中科大的源
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ 
阿里云的源
conda config --add channels http://mirrors.aliyun.com/pypi/simple/
删除源
conda config --remove channels http://mirrors.aliyun.com/pypi/simple/
设置展示源
conda config --set show_channel_urls yes
```

## pip

### 安装

```
pip install SomePackage
# 安装指定版本
pip install SomePackage==1.0
# 更新包
pip install --upgrade SomePackage
```

### 删除

```
pip remove SomePackage
```

### 修改pip源

```
当次指定
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas

配置修改
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣(douban) http://pypi.douban.com/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

或
在当前用户目录下创建.pip文件夹: 
mkdir ~/.pip
在该目录下创建pip.conf文件填写
vim ~/.pip/pip.conf
[global]
trusted-host=[主机名]
index-url=[镜像源的url]
```

### 查看安装的包

```
pip list
```

### 查看指定包的详细信息

```
pip show Package
```

### 查看系统支持 cp （python）版本

```
import pip;
print(pip._internal.pep425tags.get_supported());

python -m pip debug --verbose
```

## 系统包安装

### 更新

```
sudo apt update
sudo apt upgrade
```

### 修改apt源

```
1.备份系统默认源
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
2.打开软件源文件
sudo vim /etc/apt/sources.list

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
```

## 查看显卡占用

```
nvidia-smi
每1秒刷新一次
watch -n 1 -d nvidia-smi
```

## 进程控制后台运行

| 命令     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| ctrl + z | 将任务中断,但是此任务并没有结束,他仍然在进程中，只是放到后台并维持挂起的状态。如需其在后台继续运行，需用“bg 进程号”使其继续运行；再用"fg 进程号"可将后台进程前台化。 |
| ctrl + c | 强行中断当前程序的执行                                       |
| jobs     | 查看当前有多少在后台运行的命令                               |
| fg       | 将后台中的命令调至前台继续运行                               |
| bg       | 将一个在后台暂停的命令，变成继续执行 （在后台执行），如果后台中有多个命令，可以用 bg %jobnumber 将选中的命令调出 |
| ctrl + \ | 发送 SIGQUIT 信号给前台进程组中的所有进程，终止前台进程并生成 core 文件。 |
| ctrl + d | 表示结束当前输入（即用户不再给当前程序发出指令），那么 Linux 通常将结束当前程序 |

### 查看进程

ps -aux | grep xxx



## ubuntu系统内核及显卡驱动更新

### 查看已安装内核

```
1.
dpkg --get-selections | grep linux-image
2.
cat /boot/grub/grub.cfg |grep menuentry
```

### 查看当前内核

```
uname -a
```

### 禁止内核更新

#### 1.禁止自动升级

修改配置文件/etc/apt/apt.conf.d/10periodic
\#０是关闭，1是开启，将所有值改为0

```
vi etc/apt/apt.conf.d/10periodic
```

```
APT::Periodic::Update-Package-Lists "0";
APT::Periodic::Download-Upgradeable-Packages "0";
APT::Periodic::AutocleanInterval "0";
```
#### 2.固定版本

```
sudo apt-mark hold linux-image-generic linux-headers-generic 
sudo apt-mark hold linux-image-5.3.0-42-generic
sudo apt-mark hold linux-image-extra-5.3.0-42-generic

# 重启内核更新
sudo apt-mark unhold linux-image-5.3.0-42-generic
sudo apt-mark unhold linux-image-extra-5.3.0-42-generic
```

#### 3.更改引导启动文件

```
cat /etc/default/grub
```

**修改默认启动内核：**

老版本：

```
GRUB_DEFAULT="Ubuntu, with Linux 5.4.0-99-generic"
```

新版本：

```
GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.4.0-99-generic"
GRUB_DEFAULT="1> 6"           > 和6之间有空格 ？？
```

0 是 Ubuntu

1代表启动时第一层菜单里的 Advanced options for Ubuntu，

2，3，4，5，6为查看内核中依次的顺序  其中2, 3    4, 5    6, 7 分别为一组的正常和恢复模式 

我们选择正常，也就是2，4，6 

6即为指定内核的Index。

**更改配置文件后更新，有错误会提醒：**

```
sudo update-grub
```

### 查看显卡硬件信息

```
 lshw -c video
```

### 查看显卡内核模块版本

```
cat /proc/driver/nvidia/version
```

### 查看显卡驱动版本

```
sudo dpkg --list | grep nvidia-*
sudo dpkg --get-selections | grep nvidia-*
```

上面两个版本需一致 否则报 Failed to initialize NVML: Driver/library version mismatch 错误

### 查看当前使用cuda版本

```
nvcc -V
```

### 查看ubuntu更新日志

```
cat /var/log/apt/history.log
```

### 查看报错详情（有用）

```
dmesg |tail -4
```

dmesg 显示开机后系统信息 -T显示人类时间

tail 查看文件   -4  最后四行

### nvidia显卡卸载安装

#### 查看可用版本

```
sudo ubuntu-drivers devices
```

#### 安装

```
sudo ubuntu-drivers autoinstall
```

或

```
sudo apt install nvidia-driver-版本号
```

#### 文件安装

```
sudo chmod a+x NVIDIA-Linux-x86_64-450.80.02.run
```

ubuntu 16.04默认安装了第三方开源的驱动程序nouveau，安装nvidia显卡驱动首先需要禁用nouveau，不然会碰到冲突的问题，导致无法安装nvidia显卡驱动。

```
sudo ./NVIDIA-Linux-x86_64-450.80.02.run -no-x-check -no-nouveau-check -no-opengl-files
```

#### 卸载

```
sudo /usr/bin/nvidia-uninstall 
sudo apt-get --purge remove nvidia-*
sudo apt-get purge nvidia*
sudo apt-get purge libnvidia*
```

# linux常用命令

## 用户/用户组操作

### 创建用户

```
useradd username -m (-m 相当于会创建对应的用户家目录)
# 指定家目录
useradd username -d /home/user
usermod -s /bin/bash username (指定shell，否则会非常不便于终端操作)

# 综合命令
useradd username -m -s /bin/bash
useradd username -d /home/user -s /bin/bash
```

### 删除用户

```
userdel username。
# 若想将它在系统上的文件也删除掉
userdel -r username
```

### 用户加入组

```
# 额外添加组
sudo usermod -a -G group user  
# g为覆盖  只加入该组
sudo usermod  -g group user  
```

详情查看 usermod -h

### 用户删除组

```
sudo gpasswd -d user group
```

### 改变文件夹权限

```
chmod user:group dir
```

## 改系统代理

```
vim ~/.bashrc

# 最后一行加上
export HTTP_PROXY="http://127.0.0.1:7890"
export HTTPS_PROXY="https://127.0.0.1:7890"

# 更新
source ~/.bashrc
```

