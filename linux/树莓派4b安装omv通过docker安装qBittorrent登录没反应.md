# 树莓派4b安装omv通过docker安装qBittorrent登录没反应


### 问题：在树莓派安装omv后，安装最新版docker后启动qBittorrent，可以正常访问webUI，但以正确账户密码登录时无法跳转，错误密码时显示错误

omv 5+

docker 20+

### 原因：libseccomp2版本不够

从清华源可以看到最新为2.3.3，实际需求2.4+

```
[pi@raspberrypi:~ $ sudo apt-cache madison libseccomp2
libseccomp2 |    2.3.3-4 | http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian buster/main armhf Packages
libseccomp |    2.3.3-4 | http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian buster/main Sources]()
```

直接从源里找，发现有更新的版本

| [libseccomp2_2.3.3-4_armhf.deb](https://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/pool/main/libs/libseccomp/libseccomp2_2.3.3-4_armhf.deb) | 32.3 KiB | 2019-02-20 18:08 |
| ------------------------------------------------------------ | -------- | ---------------- |
| [libseccomp2_2.5.1-1+rpi1_armhf.deb](https://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/pool/main/libs/libseccomp/libseccomp2_2.5.1-1+rpi1_armhf.deb) | 46.4 KiB | 2020-12-30 05:17 |
| [libseccomp_2.1.1-1.debian.tar.xz](https://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/pool/main/libs/libseccomp/libseccomp_2.1.1-1.debian.tar.xz) | 4.4 KiB  | 2014-04-13 01:57 |

### 解决办法：更新libseccomp2至最新版

https://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/pool/main/libs/libseccomp/

直接从清华源下载了libseccomp2 2.5.1的文件，在树莓派里安装，问题解决。

树莓派好像默认没有开ftp，这里刚好是用树莓派作nas开的smb，直接传了进去。
