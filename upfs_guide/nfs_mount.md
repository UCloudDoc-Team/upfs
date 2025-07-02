# NFS挂载

UPFS仅支持Version 3版本的NFS协议，一下NFS均代指NFSv3。
挂载之前需要先创建NFS挂载点，具体见[NFS挂载点](/upfs_guide/nfs_mountpoint.md)
Windows系统的NFS挂载方式见[Windows挂载](/upfs_guide/windows_mount.md)

## 步骤一、安装 NFS 客户端
如果是 CentOS/Rocky 系统的主机，请执行以下命令安装 NFS 客户端：

```
sudo yum install nfs-utils
```

如果是 Ubuntu/Debian 系统的主机，请执行以下命令安装 NFS 客户端：

```
 sudo apt-get install nfs-common
```

## 步骤二、挂载文件系统

请您根据[NFS挂载点管理](/upfs_guide/nfs_mountpoint.md)操作中显示的挂载点信息进行操作。下面以文件系统 ID 为 upfs-1djk2eyon69i 的文件系统为例展示挂载操作，假设其挂载点 IP 为 10.59.105.106(实际操作时请用您要使用的文件系统 ID 和挂载点 IP 替代示例中的相关参数)。

执行以下命令进行挂载
```
 sudo mount -t nfs -o rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport,nfsvers=3,proto=tcp,mountproto=tcp,nolock,noacl 10.59.105.106:/ /mnt
``` 
挂载命令中各参数含义介绍如下：

| 参数/选项名称 | 作用描述 |
|--------------|----------|
| **挂载点** | 访问文件系统的入口，挂载点由一个 IPv4 地址表示 |
| **rsize,wsize** | 指定与文件存储服务端交互时的数据块大小（单位Byte），建议设置为 1048576 |
| **hard** | 指示 NFS 客户端在远端文件存储临时不可服务时继续重试操作（而非返回错误），建议开启以提升对网络故障的容错性，特别适合长时间运行的任务 |
| **timeo** | NFS 客户端重试请求前的等待时间（单位：0.1秒），建议设置值为 600 |
| **retrans** | NFS 客户端对单次请求的重试次数，建议设置为 2 |
| **noresvport** | 网络中断重连时使用新端口，可降低重连失败概率，建议开启 |

## 步骤三、查看挂载结果

挂载命令执行完成后，可以通过执行 mount -l 或者 nfsstat -m 列出当前挂载的文件系统列表，如果有包含需要挂载的文件系统则表示挂载成功。

### 步骤四、设置自动挂载

我们可以通过在 Linux 系统中配置 /etc/fstab 文件的方式来实现 NFS 文件系统的自动挂载。
```
    10.59.105.106:/ /mnt nfs rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,nfsvers=3,proto=tcp,mountproto=tcp,nolock,noacl,_netdev,noresvport 0 0
```

#### 命令中各参数含义介绍如下

|参数/选项名称          |含义描述     |
|---------|-----------------------------------------------------------------|
|_netdev	|表示待挂载的文件系统是网络文件系统，防止在网络未准备好之前进行挂载操作导致主机启动卡死。|
|0(noresvport后第一项)	|非 0 值表示文件系统应由 dump 备份，对于 NAS 服务来说该项为 0。|
|0(noresvport后第二项)	|表示在开机后是否执行 fsck 操作，由于 UPFS 产品由服务端保证数据的持久化和一致性，该选项无须打开。|