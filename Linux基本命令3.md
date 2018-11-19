将程序放到后台后,除了使用fg调到前台后按下Ctrl-C退出,

还可以使用kill命令:

kill 命令 : 给进程发送信号

kill -9 发送SIGKILL,杀死进程

SIGSEGV 段错误信号



添加环境变量: 在bashrc中添加一行 

```bash
export PATH=$PATH:/path/to/add
```

添加之后可以使用env命令查看环境变量



添加用户 假设用户组为colin 用户名为colin

```bash
sudo useradd -s /bin/bash -g colin -d /home/colin -m colin

sudo passwd colin
```

**如果用户不在sudoers中,将无法使用sudo命令.**

删除用户

```bash
sudo userdel –r colin 
```

## 网络配置

ifconfig  (用ip代替ifconfig:https://www.linux.com/learn/replacing-ifconfig-ip)

> Examples for deprecated commands and their replacements:
>
> arp => ip n (ip neighbor)
>
> ifconfig => ip a (ip addr), ip link, ip -s (ip -stats)
>
> iptunnel => ip tunnel
>
> iwconfig => iw
>
> nameif => ip link, ifrename
>
> netstat => ss, ip route (for netstat-r), ip -s link (for netstat -i), ip maddr (for netstat-g)

查看网卡信息 ifconfig

关闭网卡 sudo ifconfig 网卡号 down 

启动网卡 sudo ifconfig 网卡号 down 

配置临时IP sudo ifconfig 网卡号 IP地址



netstat (用ss代替 https://unix.stackexchange.com/questions/258711/alternative-to-netstat-s)

> SS Examples :
>
> get all connections: ss | less
>
> ss -t get tcp connections not in listen mode (server programs)
>
> ss -u get udp connections not in listen mode
>
> ss -x get unix socket pipe connections
>
> ss -ta get all tcp connections
>
> ss -au get all udp connections
>
> ss -nt all tcp without host name
>
> ss -ltn listening tcp without host resolution
>
> ss -ltp listening tcp with PID and name
>
> ss -s prints statstics
>
> ss -tn -o tcp connection with domain host and show keepalive timer
>
> ss -tl4 ip4 connections 

## 安装配置网络服务

### ftp (vsftpd)

/etc/config/

1.安装vsftpd服务器

```bash
sudo apt-get install vsftpd
```

2.配置vsftpd.conf文件
sudo vi /etc/vsftpd.conf
添加下面设置

```config
anonymous_enable=YES
anon_root=/home/colin/ftp
no_anon_password=YES
write_enable=YES
anon_upload_enable=YES
anon_mkdir_write_enable=YES
```

需要注意的是ftp的根目录权限应该为555(r-xr-xr-x),否则报错无法登陆

> 500 OOPS: vsftpd: refusing to run with writable root inside chroot()
> Login failed.
> 421 Service not available, remote server has closed connection

当已存在同名文件时,不能上传.

lftp相对于ftp客户端有上传下载整个目录,命令补全等功能.

### nfs服务器

nfs可以挂载目录到本地

1.安装nfs服务器

```bash
sudo apt-get install nfs-kernel-server
```
2.设置/etc/exports配置文件

```bash
sudo vi /etc/exports
```

添加这行配置

```bash
/home/用户名/nfs *(rw,sync,no_root_squash)
```

3.在用户目录下创建nfs目录

```bash
mkdir /home/用户名/nfs
```

4.重启服务器，重新加载配置文件

```bash
sudo /etc/init.d/nfs-kernel-server restart
```
5.挂载服务器，把服务器共享目录nfs挂在到/mnt节点

```bash
sudo mount –t nfs –o nolock –o tcp IP:/home/用户名/nfs /mnt
```
6.卸载网络共享目录

```bash
sudo umount /mnt
```

## ssh服务器

1.安装ssh服务器

```bash
sudo apt-get install openssh-server
```

2.连接使用

ssh  user@ip



## umask权限掩码

(~umask)&0666