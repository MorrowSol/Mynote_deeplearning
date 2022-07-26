## 目录

/data/maya

## 虚拟环境

### 进入 env

```shell
source activate env_name
conda deactivate
```

### 查看虚拟环境

```shell
conda env list
conda create -n env_name python=x.x
conda create -n BBB --clone AAA
conda remove -n env_name --all

修改conda源
conda config --show
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
中科大的源
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ 
阿里云的源
conda config --add channels http://mirrors.aliyun.com/pypi/simple/

conda config --remove channels http://mirrors.aliyun.com/pypi/simple/
conda config --set show_channel_urls yes

环境迁移
进入环境
conda activate bcy
导出 environment.yml 文件：
conda env export > environment.yml
注意：如果当前路径已经有了 environment.yml 文件，conda 会重写这个文件
重现环境：不需要创建环境会自己创建
conda env create -f environment.yml
```

### 查看显卡占用

```
nvidia-smi
watch -n 1 -d nvidia-smi
指定空闲的GPU运行python程序。
CUDA_VISIBLE_DEVICES=0,2,3 python test.py
在python程序中指定GPU
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "0,2,3"
```

```
sudo apt update
sudo apt upgrade

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

改代理

```
vim ~/.bashrc
最后一行加上

export HTTP_PROXY="http://127.0.0.1:7890"
export HTTPS_PROXY="https://127.0.0.1:7890"

source ~/.bashrc
```

```
sudo apt-get install python3.8
sudo apt-get install python-pip (安装python时应该自带pip)
sudo apt install python3-pip

换源
mkdir ~/.pip
vim ~/.pip/pip.conf
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣(douban) http://pypi.douban.com/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
pip --version     # Python2.x 版本命令
pip3 --version    # Python3.x 版本命令

pip list
pip install SomePackage==1.0
pip install --upgrade SomePackage
pip search SomePackage

显示安装包信息
pip show
查看指定包的详细信息
pip show -f SomePackage
```

## 查看系统支持 cp 版本

```
import pip;
print(pip._internal.pep425tags.get_supported());

python -m pip debug --verbose
```

## ubuntu 18.04 子系统所在目录

C:\Users\用户 ID\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs

## 进程控制

| 命令      | 说明                                                                                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ctrl + z  | 将任务中断,但是此任务并没有结束,他仍然在进程中，只是放到后台并维持挂起的状态。如需其在后台继续运行，需用“bg 进程号”使其继续运行；再用"fg 进程号"可将后台进程前台化。 |
| ctrl + c  | 强行中断当前程序的执行                                                                                                                                               |
| jobs      | 查看当前有多少在后台运行的命令                                                                                                                                       |
| fg        | 将后台中的命令调至前台继续运行                                                                                                                                       |
| bg        | 将一个在后台暂停的命令，变成继续执行 （在后台执行），如果后台中有多个命令，可以用 bg %jobnumber 将选中的命令调出                                                     |
| ctrl + \  | 发送 SIGQUIT 信号给前台进程组中的所有进程，终止前台进程并生成 core 文件。                                                                                            |
| ctrl + d  | 表示结束当前输入（即用户不再给当前程序发出指令），那么 Linux 通常将结束当前程序                                                                                      |

## windows 系统 wsl linux ubuntu 初始化

改源

https://blog.csdn.net/Chaowanq/article/details/121559709

通过 date 命令，查看时间与当前时间一致，排除时间造成的证书问题。

通过 apt install ca-certificates --reinstall 无法更新安装包。

手动下载 ca-certificates deb 文件重新安装最新版。文件来自：https://pkgs.org/download/ca-certificates

Type URL
Mirror archive.ubuntu.com
Binary Package http://archive.ubuntu.com/ubuntu/pool/main/c/ca-certificates/ca-certificates_20210119~20.04.2_all.deb
Source Package ca-certificates
在终端执行以下命令：

**cd /tmp**
**wget http://archive.ubuntu.com/ubuntu/pool/main/c/ca-certificates/ca-certificates_20210119~20.04.2_all.deb**
**sudo dpkg -i ./ca-certificates_20210119~20.04.2_all.deb**

为了安全起见，先将默认的源进行备份

sudo mv /etc/apt/sources.list /etc/apt/sources.list.bak

然后添加清华镜像源(保证清华源的版本和安装 ubuntu 版本一致)，将清华源的内容贴进去

https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/

sudo vi /etc/apt/sources.list

更新一下：

sudo apt-get update && sudo apt-get upgrade

接下来下载速度就会快很多。

2.2 中文乱码

- 在/etc/environment(文件末尾添加)

```bash
# 设置中文
export LC_ALL="zh_CN.UTF-8"
```

- 执行命令：

```text
sudo locale-gen zh_CN.UTF-8
```

删除掉 Makefile 里 --Werror

使用最新版本的 OpenSSL 编译时出现此错误`src/event/ngx_event_openssl.c: In function ‘ngx_ssl_connection_error’: src/event/ngx_event_openssl.c:2048:21: error: ‘SSL_R_NO_CIPHERS_PASSED’ undeclared (first use in this function) || n == SSL_R_NO_CIPHERS_PASSED /* 182 */ ^~~~~~~~~~~~~~~~~~~~~~~ src/event/ngx_event_openssl.c:2048:21: note: each undeclared identifier is reported only once for each function it appears in objs/Makefile:1092: recipe for target 'objs/src/event/ngx_event_openssl.o' failed make[2]: *** [objs/src/event/ngx_event_openssl.o] Error 1`OpenSSL 版本： 包：libssl-dev 来源：openssl 版本：1.1.0e-1 无论如何我可以使用受支持的旧版本进行编译吗？

### https://github.com/kgretzky/evilginx/issues/2#issuecomment-292592726)

是的。我在将最新的 openssl 推送到官方存储库时遇到了类似的问题。 通常安装这个包会解决这个问题：

apt-get install libssl1.0-dev`

https://www.jianshu.com/p/1cbff1431590

sudo /usr/local/nginx/sbin/nginx -s reload

## ubuntu系统内核及显卡驱动更新

### 查看已安装内核

dpkg --get-selections | grep linux-image

### 查看当前内核

uname -a

### 禁止内核更新

**1.禁止自动升级**

修改配置文件/etc/apt/apt.conf.d/10periodic
\#０是关闭，1是开启，将所有值改为0
vi etc/apt/apt.conf.d/10periodic

APT::Periodic::Update-Package-Lists "0";
APT::Periodic::Download-Upgradeable-Packages "0";
APT::Periodic::AutocleanInterval "0";



sudo apt-mark hold linux-image-generic linux-headers-generic 

sudo apt-mark hold linux-image-5.3.0-42-generic
sudo apt-mark hold linux-image-extra-5.3.0-42-generic

#### 查看内核

```
cat /boot/grub/grub.cfg |grep menuentry
```

#### 查看引导启动文件

cat /etc/default/grub

**修改默认启动内核：**

老版本：

GRUB_DEFAULT="Ubuntu, with Linux 5.4.0-99-generic"

新版本：

GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.4.0-99-generic"

GRUB_DEFAULT="1> 6"           \> 和4**之间有空格**（？）

0 是 Ubuntu

1代表启动时第一层菜单里的 Advanced options for Ubuntu，

2，3，4，5，6为查看内核中依次的顺序  其中2，3     4，5     6，7 为一组的正常和恢复模式 

我们选择正常，也就是2，4，6 

6即为指定内核的Index。

更改配置文件后更新，有错误会提醒

sudo update-grub

### 重启内核更新

sudo apt-mark unhold linux-image-5.3.0-42-generic
sudo apt-mark unhold linux-image-extra-5.3.0-42-generic

### 查看显卡信息

 lshw -c video

### 查看显卡内核模块版本

cat /proc/driver/nvidia/version

### 查看显卡驱动版本

sudo dpkg --list | grep nvidia-*

sudo dpkg --get-selections | grep nvidia-*

上面两个版本需一致 否则报 Failed to initialize NVML: Driver/library version mismatch 错误

### 查看当前使用cuda版本

nvcc -V

### 查看ubuntu更新日志

cat /var/log/apt/history.log

### 查看报错详情（有用）

dmesg |tail -4

dmesg 显示开机后系统信息 -T显示人类时间

tail 查看文件   -4  最后四行

### nvidia显卡卸载安装

#### 查看可用版本

sudo ubuntu-drivers devices

#### 安装

sudo ubuntu-drivers autoinstall

或

sudo apt install nvidia-driver-版本号

#### 文件安装

sudo chmod a+x NVIDIA-Linux-x86_64-450.80.02.run

ubuntu 16.04默认安装了第三方开源的驱动程序nouveau，安装nvidia显卡驱动首先需要禁用nouveau，不然会碰到冲突的问题，导致无法安装nvidia显卡驱动。

sudo ./NVIDIA-Linux-x86_64-450.80.02.run -no-x-check -no-nouveau-check -no-opengl-files

#### 卸载

sudo /usr/bin/nvidia-uninstall 

sudo apt-get --purge remove nvidia-*

sudo apt-get purge nvidia*

sudo apt-get purge libnvidia*

## 查看进程

ps -aux | grep asd

## 创建用户

```
useradd username -m (-m 相当于会创建对应的用户家目录)
usermod -s /bin/bash username (指定shell，否则会非常不便于终端操作)
```

userdel 用户名。若想将它在系统上的文件也删除掉，使用命令：userdel -r 用户名

## 用户加入组

sudo usermod -a -G group user   额外添加组

sudo usermod  -g group user  g为覆盖  只加入该组

详情查看 usermod -h

## 用户删除组

sudo gpasswd -d user group

## 改变文件夹权限

chmod user:group dir
