# 挂载概述
UPFS目前支持POSIX与NFSv3协议两种接入方式。
POSIX接入支持Linux本地挂载和K8s容器挂载两种挂载方式。可挂载的操作系统限制请查看[产品限制](/upfs/upfs_manual_instruction/limit)章节。
NFSv3接入支持Linux本地与Windows系统挂载。

#### 注意事项
以下注意事项针对POSIX挂载，使用NFSv3挂载请详见[NFS挂载](/upfs_guide/nfs_mount.md)章节。
* 需要预先在控制台上创建好对应业务需要的UPFS文件系统，才可以执行挂载操作。
* 挂载时需要的文件系统URL（URL详细规则请见[主要概念](/upfs/upfs_manual_instruction/concept)中的文件系统URL部分）中的管理服务地址和文件系统资源ID，分别取控制台展示信息列表中的「挂载地址」和 「资源ID」列。
* 挂载前请确保您所挂载的云主机配置最低CPU为2核，内存为8g。
* 不论是Linux方式挂载还是K8s方式挂载，都需要预先安装单独的客户端, 客户端安装步骤请查看[UPFS客户端安装](/upfs/upfs_guide/client_install.md)。
* 需注意Ubuntu系统必须使用sudo命令挂载，再次挂载前需要删除/var/tmp目录下的<resource_id>.sock与<resource_id>.sock.lock文件。

