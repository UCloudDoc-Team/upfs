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
命令行解析如下：

| 命令行字段 | 解释                       |
|--------------|--------------------------|
| mount             | 挂载命令关键字                  |
| -t upfs             | 表明挂载的文件系统类型是upfs         |
| <mount_address1>,<mount_address2>/<resource_id>  | 从UCloud控制台页面处获取的文件系统URL （URL详细规则请见[主要概念](/upfs/upfs_manual_instruction/concept)中的文件系统URL部分） |
| /path/to/mount            | 要挂载到的本地路径，请确保该目录没有被其它文件系统挂载        |



除此之外，还可在挂载时通过-o添加额外选项（选项之间使用逗号分割），以下列出所有的可选项：

| 选项名称            | 作用描述                                |
|-----------------|-------------------------------------|
| ro              | 只读模式                                |
| rw              | 读写模式      |

示例：

```shell
mount -t upfs 100.64.240.95:10109,100.64.240.97:10109:/upfs-yc3ae1gwpwg /mnt  -o ro
```

## 步骤三、挂载状态查看

执行命令```df -h```即可查看挂载文件系统状态。若挂载成功，会看到upfs对于资源ID的条目，以及使用容量，挂载点等信息。

![](/images/upfs_guide/linux_mount1.png)




**如果挂载有问题，请及时联系UCloud技术支持。**
    