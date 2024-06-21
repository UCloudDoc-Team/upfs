# UPFS客户端安装


UPFS是支持POSIX文件语义的分布式并行文件系统，使用基于FUSE的用户态文件系统访问。您需要在使用产品前先安装客户端，UCloud支持通过包管理器和客户端安装包两种方式安装。

### 方式一、通过包管理器安装

通过包管理器安装时请确保您的安装源为UCloud提供的官方安装源。

对于使用CentOS8/7, Rocky8的用户，推荐使用该方式安装客户端，请注意公测期支持通过包管理器安装客户端的云主机操作系统镜像版本限制如下表，如果您的业务云主机操作系统版本不在下表之中，我们推荐您使用方式二安装客户端。

| 操作系统类型 | 已支持包管理器安装的操作系统版本 |        
|--------|------------------|
| CentOS | CentOS 7.2 64位   |
| CentOS | CentOS 7.3 64位   |
| CentOS | CentOS 7.4 64位   |
| CentOS | CentOS 7.6 64位   |
| CentOS | CentOS 7.8 64位   |
| CentOS | CentOS 7.9 64位   |
| CentOS | CentOS 8.0 64位   |
| Rocky| Rocky 8.5 64位    |

如果是 CentOS 系统的主机，请执行以下命令安装：

```shell
yum install upfs_client_v1.0
```

如果是 Rocky 系统的主机，请执行以下命令安装：

```shell
dnf install upfs_client_v1.0
```

### 方式二、通过客户端安装包安装（推荐）

当系统安装源缺失或系统发现版本较低，无法通过包管理器安装依赖时，也可以通过下载提供的安装包手动安装客户端。

1.下载安装包。

```shell
wget https://upfs-public.cn-bj.ufileos.com/upfs_client-public-v1.tar.gz
```

2.解压安装包。

```shell
tar zxvf upfs_client-public-v1.tar.gz
```

3.执行安装脚本，请注意如果操作系统类型为Ubuntu，您首先需要获取root权限。

```shell
cd upfs_client-public-v1 & ./install.sh
```

注意事项：
  - 通过Linux方式挂载或者K8s挂载时，在安装客户端时，可参考以上两种方式。
  - 通过K8s挂载时安装客户端时，需要在所有的K8s work node 上安装好UPFS客户端。