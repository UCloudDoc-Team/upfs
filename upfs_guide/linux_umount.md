# 卸载文件系统(Linux)

## 步骤一、查看挂载信息

1.通过执行 ```df -h``` 获取文件系统对应的挂载路径(挂载点列显示的路径，如下图所示的 ```/mnt``` 即为当前upfs文件系统的挂载路径 ```<mounted_path>```)。

   ![](/images/upfs_guide/linux_umount1.png)

## 步骤二、卸载文件系统

1.执行 ```umount <mounted_path>```, 以上图为例，此时卸载所需执行的命令为 ```umount /mnt```




**如果卸载文件系统有问题，请及时联系UCloud技术支持。**