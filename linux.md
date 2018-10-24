# tar
解压 tar -xzvf xxxx.tar.gz  
# 执行脚本
./xxx.sh  
# 挂载cd-rom
mount /dev/cdrom /mnt/cdrom  
cp xxx.xxx /mnt  

# df -Th
查看分区信息

# ip
cd /etc/sysconfig/network-script  
NETMASK=255.255.255.0  
IPADDR=192.168.244.2  
GATEWAY=192.168.244.1  
BROADCAST=192.168.244.255  
NETWORK=192.168.244.0  


# env
env |grep oracle

# 配置密码
passwd username

# wsl 
wsl(windows subsystem forlinux)。wsl不支持linux kernel的一些特性。wsl不是linux内核也不包含linux内核。这个子系统其实相当于用nt的api重写了linux内核的api.    
安装方式:  
1、lxrun.exe 仅用于配置适用于 Linux 的 Windows 子系统的旧分发版。  
```
用法:
    /install - 安装子系统
        可选参数:
            /y - 不提示用户接受或创建子系统用户
    /uninstall - 卸载子系统
        可选参数:
            /full - 执行完全卸载
            /y - 不提示用户确认
    /setdefaultuser - 设置默认子系统用户。如果该用户帐户不存在，则会创建该帐户。
        可选参数:
            username - 提供用户名
            /y - 如果提供了用户名，则不提示创建密码
```
2、应用商店搜索linux

# ssh

ssh -p xx user@ip