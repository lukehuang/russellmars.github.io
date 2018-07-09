---
title: vagrant-starter
date: 2018-06-29 20:18:33
tags: [vagrant, linux, 开发环境]
---

vagrant 的简单安装和使用，搭建统一开发平台。
<!--more-->

### 安装
+ 下载安装[VirtualBox](https://www.virtualbox.org/wiki/Downloads)
+ 下载安装[vagrant](https://www.vagrantup.com/downloads.html)
+ 下载[box](http://www.vagrantbox.es/)，我这里下载里一个`CentOS 7.0 x64`，要注意下载provider为VirtualBox的box，下载后，打开powershell/cmd，安装box 
``` bash
  # vagrant box add [title] [url]
  # title：box使用的使用的名字
  # url为box的地址，可以是在线地址，也可以是本地的
  vagrant box add CentOS7.0_64 C:\Users\russellmars\Downloads\centos-7.0-x86_64.box
  ```
### 启动
+ 新建一个目录，并初始化环境
```
mkdir hello
cd hello
vagrant init CentOS7.0_64
```
初始化成功后，该目录会出现一个`Vagrantfile`的配置文件，先用默认配置
+ 启动box
```
$ vagrant up
```
如果启动成功，则会出现如下信息
```
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 4.3.28
    default: VirtualBox Version: 5.2
==> default: Mounting shared folders...
    default: /vagrant => D:/database/vagrant/hello
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: flag to force provisioning. Provisioners marked to run always will still run.
``` 

+ 连接到远程linux，可以使用xshell或者win10 自带的OpenSSH客户端，账号密码默认为`vagrant`
```
ssh vagrant@127.0.0.1 -p 2222
```
启动以后，`hello`目录就与linux下的`/vagrant`目录下的文件双向同步了，可以新建几个文件测试一下

+ 其他命令
```
#关闭
vagrant halt
#重启
vagrant reload
```

### 相关配置
#### 网络配置
+ 转发端口，将虚拟机的端口映射到主机
```
config.vm.network "forwarded_port", guest: 80, host: 8080
```
以上是将虚拟机的80端口映射到主机的8080端口上
+ 私有网络
```
config.vm.network "private_network", ip: "192.168.33.10"
```
只有主机可以通过192.168.33.10来访问虚拟机
+ 公共网络
```
config.vm.network "public_network", ip: "192.168.0.17"
```
局域网内的成员可以通过192.168.33.10来访问虚拟机

### 共享目录
```
# 添加新的共享文件夹
config.vm.synced_folder "src/", "/srv/website"
```
第一个参数是主机上目录的路径。如果路径是相对的，则相对于项目根目录。第二个参数必须是访客机器中共享文件夹的绝对路径。如果该文件夹不存在，该文件夹将被创建

```
# 禁用默认的共享文件夹
config.vm.synced_folder ".", "/vagrant", disabled: true
```



### thanks
+ [官方文档](https://www.vagrantup.com/docs/)
+ [windows下vagrant的安装使用](https://www.cnblogs.com/vishun/archive/2017/06/02/6932454.html)