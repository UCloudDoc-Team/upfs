# 挂载文件系统(Windows)

## NFS

### 步骤一、安装NFS客户端  

1. 登录Windows云主机。
2. 点击左下角`开始`按钮，打开`服务器管理器`。  
    ![](/images/upfs_guide/windows_mount1.png)

3. 选择`添加角色和功能`。  
    ![](/images/upfs_guide/windows_mount2.png)

4. 根据提示安装NFS客户端。
    a.`开始之前`、`安装类型`、`服务器选择`保持默认选择，点击下一步即可。  
    b.在`服务器角色`选项卡中勾选`文件和存储服务->文件和iSCSI服务-> NFS服务器`，点击下一步。  
    c.在`功能`选项卡中勾选`NFS客户端`，点击下一步。  
    d.点击安装。  

5. 重启云主机。

6. 打开`命令提示符`窗口，输入命令`mount`，如果输出以下信息，则说明NFS客户端安装完成。  
    ![](/images/upfs_guide/windows_mount3.png)

<br/>

### 步骤二、挂载UPFS文件系统  
1. 打开`命令提示符`窗口，输入以下命令挂载UPFS文件系统。

        mount -o nolock -o mtype=hard -o timeout=10 \\10.0.8.1\! Z:

    请根据UPFS文件系统控制台`NFS挂载信息`页面的实际`挂载地址`，替换命令中的`10.0.8.1(只需替换ip)`，同时挂载到本地的盘符Z也可以根据实际需求替换。

2. 挂载成功后，再次执行`mount`命令检查挂载结果。  
    ![](/images/upfs_guide/windows_mount4.png)

    检查属性中`mount`类型是否为`hard`、`timeout`时间是否大于等于`10.0`、`locking`值是否为`no`。如果不是，说明挂载异常，请使用`umount Z:`(盘符根据实际情况修改)命令卸载文件系统，并参考上述步骤重新挂载。

3. 双击`此电脑`图标，查看共享文件系统。在共享文件系统中创建、删除文件\文件夹，确认能否正常使用。
    ![](/images/upfs_guide/windows_mount5.png)

<br/>

### 步骤三、设置自动挂载UPFS文件系统  

1. 在云主机的`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`目录下创建挂载脚本`auto_mount.bat`，脚本内容如下。

        mount -o nolock -o mtype=hard -o timeout=10 \\mountpoint_ip\! Z:

    请根据UPFS文件系统控制台`NFS挂载信息`页面的实际`挂载地址`，替换命令中的`10.0.8.1(只需替换ip)`，同时挂载到本地的盘符Z也可以根据实际需求替换。

2. 创建挂载任务。  
    a.点击左下角`开始`按钮，打开`控制面板`。  
    ![](/images/upfs_guide/windows_mount6.png)

    b.点击`系统和安全`，并选择`计划任务`。  
    ![](/images/upfs_guide/windows_mount7.png)

    c.在操作选项卡点击`创建任务`。  
    在`常规`选项卡中，设置`名称`，并勾选`不管用户是否登录都要运行`(如果使用的系统是Windows Server 2016，则勾选`只在用户登录时运行`)，勾选`使用最高权限运行`。 
    ![](/images/upfs_guide/windows_mount8.png)

    在`触发器`选项卡中，点击`新建`，将`开始任务`设置为`登陆时`，并勾选`高级设置`里的`已启用`。  
    ![](/images/upfs_guide/windows_mount9.png)

    在`操作`选项卡中，点击`新建`，将`操作`设置为`启动程序`，在`程序或脚本`一栏选中步骤1创建的脚本`auto_mount.bat`，点击确定。  
    ![](/images/upfs_guide/windows_mount10.png)

    在`条件`选项卡中，勾选`只有在以下网络连接可用时才启动`，并在下拉选项中选择`任何连接`。  
    ![](/images/upfs_guide/windows_mount11.png)

    在`设置`选项卡中，取消勾选`允许按需运行任务`、`如果任务运行时间超过以下时间，停止任务`。勾选`如果请求后任务还在运行，强行将其停止`。在`如果此任务已经运行，以下规则适用`下拉框中选择`请勿启动新实例`。点击确定。
    ![](/images/upfs_guide/windows_mount12.png)

3. 重启云主机，验证结果。  
     a.查看计划任务状态，如果显示如下信息，则表示计划正常执行。  
     ![](/images/upfs_guide/windows_mount13.png)

     b.在`命令提示符`窗口执行`mount`命令，查看挂载信息。确认`mount`类型是否为`hard`、`locking`是否为`no`、`timeout`是否大于等于`10`。如果挂载正常，则参数应该如下图所示。  
    ![](/images/upfs_guide/windows_mount14.png)