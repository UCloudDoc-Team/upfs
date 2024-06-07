# 挂载文件系统(Linux)

## 步骤一、安装客户端

UPFS是支持POSIX文件语义的分布式并行文件系统，使用基于FUSE的用户态文件系统访问。您需要在使用产品前先安装客户端。

### 方式一、通过包管理器安装

对于使用CentOS8/7, Ubuntu22.04/20.04, Debian11, Rocky9/8的用户，推荐使用该方式安装客户端。

如果是 CentOS 系统的主机，请执行以下命令安装：

```shell
yum install upfs_client_v2.0-2.0-1
```

如果是 Ubuntu/Debian 系统的主机，请执行以下命令安装：

```shell
apt install upfs_client_v2.0-2.0-1
```

如果是 Rocky 系统的主机，请执行以下命令安装：

```shell
dnf install upfs_client_v2.0-2.0-1
```

### 方式二、通过客户端安装包安装

当系统安装源缺失或系统发现版版本较低，无法通过包管理器安装依赖时，也可以通过下载提供的安装包手动安装客户端。

1.下载安装包

```shell
wget https://upfs-public.cn-bj.ufileos.com/upfs_client-v2.0.tar.gz
```

2.解压安装包

```shell
tar zxvf upfs_client-v2.0.tar.gz
```

3.执行安装脚本

```shell
cd  upfs_client-v2.0 && ./install.sh
```

## 步骤二、挂载文件系统

执行以下命令即可挂载文件系统，注意两个挂载地址都要填，用逗号分割，且中间没有空格。

```shell
mount -t upfs <mount_address1>,<mount_address2>/<resource_id>  /path/to/mount 
```

除此之外，还可在挂载时通过-o添加额外选项（选项之间使用逗号分割），以下列出所有的可选项：

| 选项名称            | 作用描述                                |
|-----------------|-------------------------------------|
| ro              | 只读模式                                |
| rw              | 读写模式      |                          
| _netdev         | 表示待挂载的文件系统是网络文件系统     |              
| nosuid          | 将文件的 Set User ID（SUID）位置为0    |      
| conf=/conf/path | 指定配置文件，默认配置文件路径/etc/upfs_client.ini |

示例：

```shell
mount -t upfs 100.64.240.95:10109,100.64.240.97:10109:/upfs-yc3ae1gwpwg /mnt  -o ro,nosuid,conf=/etc/upfs.conf
```

以下列出配置文件中的一些可选项（**注： 配置文件中的选项均不建议修改，日志文件路径变更会导致程序不可用。**）：

|选项名称  |     作用描述|
|-|:-:|
|fuse_file_entry_timeout | lookup/reddirplus返回的文件项属性有效时间|
|fuse_dir_entry_timeout | lookup/reddirplus返回的目录项属性有效时间 |
|fuse_attr_timeout | 返回给fuse的文件attribute和dir entry的超时时间，0则不缓存|
|fuse_direct_io | 设置为1强制所有打开的文件为direct_io，设置为0会根据打开文件是否有O_DIRECT flag返回是否使用direct_io|

## 步骤三、挂载状态查看

执行命令```df -h```即可查看挂载文件系统状态。若挂载成功，会看到upfs对于资源ID的条目，以及使用容量，挂载点等信息。
