## windows 子系统 wsl ubuntu 初始化

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

https://github.com/kgretzky/evilginx/issues/2#issuecomment-292592726)

是的。我在将最新的 openssl 推送到官方存储库时遇到了类似的问题。 通常安装这个包会解决这个问题：

apt-get install libssl1.0-dev`

https://www.jianshu.com/p/1cbff1431590

sudo /usr/local/nginx/sbin/nginx -s reload

**ubuntu 18.04 子系统所在目录**

C:\Users\用户 ID\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs

