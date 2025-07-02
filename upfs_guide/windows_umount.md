# 卸载文件系统(Windows)

## NFS

### 步骤一、查看挂载信息

1. 在windows云主机上使用`mount`命令查看NFS文件系统挂载情况，如下图所示。  
![](/images/upfs_guide/windows_mount14.png)


### 步骤二、卸载文件系统

1. 使用以下命令卸载NFS文件系统。

        umount Z:
    
    请根据实际挂载的文件路径替换实例中的盘符Z:。

2. 如果有设置自动挂载任务，还需要删除对应的脚本和挂载任务。  
    a.删除挂载脚本，脚本默认路径：`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\auto_mount.bat`。   

    b.删除挂载任务，打开控制面板，点击`系统和安全`，并选择`计划任务`，点击`任务计划程序库`，选中自动挂载NFS文件系统的计划，点击删除即可。

