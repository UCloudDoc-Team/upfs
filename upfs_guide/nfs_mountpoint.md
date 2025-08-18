# NFS挂载点管理

UPFS仅支持Version 3版本的NFS协议，以下NFS均代指NFSv3。

## 前置条件

确认创建的文件系统支持NFSv3协议挂载，如果支持，在UPFS产品页面的协议类型这一列会显示为"POSIX/NFS"。

![](/images/upfs_guide/nfs_support.png)

## 进入挂载点管理页面

点击操作栏的详情按钮，进入详情页面后，点击NFS挂载信息选项卡。

![](/images/upfs_guide/nfs_mount_info1.png)
![](/images/upfs_guide/nfs_mount_info2.png)

## 添加挂载点

选择需要访问该文件系统的主机所在的 VPC和子网(同时支持物理云和私有专区的 VPC)，点击『确定』即完成挂载点设置。默认被选中的 VPC 下所有子网内的主机都可以访问该文件系统。单个文件系统允许创建5个挂载点，同一个VPC和子网下仅能创建一个挂载点。

![](/images/upfs_guide/add_nfs_mountpoint.png)

