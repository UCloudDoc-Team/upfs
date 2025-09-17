# UPFS客户端安装


UPFS是支持POSIX文件语义的分布式并行文件系统，使用基于FUSE的用户态文件系统访问。您需要在使用产品前先安装客户端，UCloud支持通过客户端安装包安装。  

### 下载安装包安装

1.下载安装包。

```shell
# 从外网下载
wget https://upfs-public.cn-bj.ufileos.com/upfs_client-public-latest.tar.gz
# 华北二地域从内网下载
wget http://upfs-public-wlcb.internal-cn-wlcb.ufileos.com/upfs_client-public-latest.tar.gz
# 上海二地域从内网下载
wget http://upfs-public-sh2-01.internal-cn-sh2-01.ufileos.com/upfs_client-public-latest.tar.gz
```

2.解压安装包。

```shell
tar zxvf upfs_client-public-latest.tar.gz
```

3.执行安装脚本。

```shell
cd ./upfs_client-public-latest && ./install.sh
```

  请注意如果操作系统类型为Ubuntu，需要使用root权限执行，请参考如下命令：

```shell
cd ./upfs_client-public-latest && sudo ./install.sh
```

```shell
install.sh 程序支持以下选项
install.sh max  # 安装标准性能规格客户端
install.sh mini # 安装最低性能规格客户端，能够在CPU 1核，内存2GB的虚机中挂载访问文件系统
install.sh ultra # 安装增强性能规格客户端，能够提供更高的IOPS能力
```

### 安装客户端会对当前系统执行以下修改
* 安装依赖包
  - Centos/Rocky会调用yum安装libstdc++，libibverbs，libnl3，librdmacm，libuuid，numactl-libs  
  - Ubuntu/Debian会调用apt安装libstdc++6，libibverbs1，libnl-3-200，librdmacm1，libuuid1，numactl，cron  
* 安装程序
  - /usr/local/bin目录下会创建pfs_client前缀的可执行文件和upfs_client软链文件  
  - /usr/local/bin目录下会创建upfsiostat工具，用于观测文件系统实时性能  
  - /usr/local/bin目录下会创建limit_upfs_client_log.sh脚本，用于清理程序的日志    
  - /sbin目录下会创建mount.upfs工具，用于处理upfs类型文件系统的挂载  
  - /etc目录下会创建以upfs_client前缀的配置文件  
  - 创建/var/log/upfs/client目录，用于保存运行时的日志  
* 安装日志清理定时任务
  - 会在crontab中添加定时任务，用于日志的清理  
```shell
0 3 * * * /usr/local/bin/limit_upfs_client_log.sh 2>/dev/null >&2
```

注意事项：
  - 在挂载文件系统之前，请参考以上方式安装客户端。 
  - 安装客户端需要root权限。  
  - 安装程序会对当前系统进行修改。  