# linux系统安装

## 基础环境安装

按照安装顺序进入桌面

安装更新 弹窗（系统提示，可不更新）

安装浏览器  下载好

```
sudo apt install ./google-chrome-stable_current_amd64.deb
```

## 显卡驱动及cuda安装

### 安装显卡驱动

1. 可通过 软件和更新-附加驱动 选择驱动 自动安装（建议）

2. 自行安装：

   查看可安装驱动版本:

   ```
   ubuntu-drivers devices
   ```

   选择某版本安装：

   ```
   sudo apt install nvidia-driver-*
   ```

3. 上面两种方式都需要网络环境，还可以选择用安装包安装

   此方法比较麻烦，可去网络自行查看，不做介绍

安装完成后检查

```
nvidia-smi
```

### 安装cuda11.3   

可去官网下载对应的包 地址：https://developer.nvidia.com/cuda-toolkit-archive

建议使用runfile文件安装 一开始没反应是在加载 等一会

```
sudo sh cuda_11.3.1_465.19.01_linux.run  
```

安装时注意不要安装驱动，因为已经安装更新的了

安装完检查 nvcc -V

这时你会发现没有找到，不要安装，是因为环境变量没有配置

打开自己user的.bashrc文件 可以用nano

```
nano ~/.bashrc
```

添加环境变量

```
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib63:$LD_LIBRARY_PATH
```

ctrl+o写入    ctrl+x离开

重新打开终端 更新环境变量

再次检查 发现已经找到cuda

```
nvcc -V 
```

### 安装cudnn 

地址：https://developer.nvidia.com/rdp/cudnn-archive

根据cuda版本选择 建议选择tar包下载  

```
# 解压 
tar -xvf  cudnn-linux-x86_64-8.4.0.27_cuda11.6-archive.tar.xz
# 复制文件
sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
# 添加权限
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```

## 开启ssh和ftp

**固定ip地址：192.168.11.11**

### ssh

```
sudo apt-get install openssh-server
sudo service ssh start
sudo systemctl start ssh
sudo systemctl enable ssh
```

### ftp

```
sudo apt-get install vsftpd
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

## 系统美化 

### 安装 GNOME Tweaks 工具和 gnome 扩展

GNOME Tweaks 工具是必须的，我们需要它来更改主题和图标，GNOME Tweaks 工具可以在Ubuntu的软件商店找到，也可以通过以下命令安装：

```
sudo apt-get install gnome-tweak-tool
```

为了更加的可自定义性，还需要去安装一下扩展：

```
sudo apt-get install gnome-shell-extensions
```

这里还要执行以下命令：浏览器插件 方便启用

```
sudo apt-get install chrome-gnome-shell 
```

### 进入GNOME官方插件中心安装插件

https://extensions.gnome.org/  

**User Themes** 主题 

主题商店：https://www.gnome-look.org/browse/

**Dash to Dock** 自定义dock

**Fildem global menu** 全局菜单 

还需安装一个程序见github页 https://github.com/gonzaarcr/Fildem

新建文件 添加设置 开机启动
