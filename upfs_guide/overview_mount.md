# 挂载概述
UPFS目前仅支持Linux和K8s两种挂载方式，Windows系统暂不支持挂载，可挂载的操作系统限制请查看[产品限制](/upfs/upfs_manual_instruction/limit)章节。

#### 注意事项
* 需要预先在控制台上创建好对应业务需要的UPFS文件系统，才可以执行挂载操作。
* 挂载时需要的文件系统URL（URL详细规则请见[主要概念](/upfs/upfs_manual_instruction/concept)中的文件系统URL部分）中的管理服务地址和文件系统资源ID，分别取控制台展示信息列表中的「挂载地址」和 「资源ID」列。
* 挂载前请确保您所挂载的云主机配置最低为2c8g。
* 不论是Linux方式挂载还是K8s方式挂载，都需要预先安装单独的客户端, 客户端安装步骤请查看[UPFS客户端安装](/upfs/upfs_guide/client_install.md)。
