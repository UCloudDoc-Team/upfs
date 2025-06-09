# 挂载文件系统(Linux)

## 步骤一、安装客户端

客户端安装步骤请查看[UPFS客户端安装](/upfs/upfs_guide/client_install.md)。

## 步骤二、挂载文件系统

 - 若挂载的云主机操作系统为Ubuntu系统，必须使用sudo命令挂载，再次挂载前需要删除/var/tmp目录下的<resource_id>.sock与<resource_id>.sock.lock文件。

    ```shell
    sudo mount -t upfs <mount_address1>,<mount_address2>/<resource_id>  /path/to/mount 
    ```

 - 若挂载的云主机操作系统非Ubuntu系统，执行以下命令即可挂载文件系统： 

    ```shell
    mount -t upfs <mount_address1>,<mount_address2>/<resource_id>  /path/to/mount 
    ```
    命令行字段含义解析如下：
    
    | 命令行字段 | 解释                       |
    |--------------|--------------------------|
    | ```mount```             | 挂载命令关键字                  |
    | ```-t upfs```             | 表明挂载的文件系统类型是upfs         |
    | ```<mount_address1>,<mount_address2>/<resource_id>``` | 从UCloud控制台页面处获取的文件系统URL （URL详细规则请见[主要概念](/upfs/upfs_manual_instruction/concept)中的文件系统URL部分） |
    | ```/path/to/mount```           | 要挂载到的本地路径，请确保该目录没有被其它文件系统挂载        |

    除此之外，还可在挂载时通过 ```-o``` 添加额外选项（选项之间使用逗号分割），以下列出所有的可选项：
    
    | 选项名称            | 作用描述                                |
    |-----------------|-------------------------------------|
    | ```ro```              | 只读模式                                |
    | ```rw```              | 读写模式      |
    | ```level=mini``` | 最低性能规格挂载，适合CPU1核，内存2GB的虚机使用 |
    | ```level=max``` | 标准性能规格挂载 | 
    | ```level=ultra``` | 增强性能规格挂载 |  

    示例：
    
    ```shell
    mount -t upfs 100.64.240.95:10109,100.64.240.97:10109/upfs-yc3ae1gwpwg /mnt  -o ro,level=mini 
    ```

  - 如果挂载成功，最后一行会有如下的成功日志和客户端的版本信息：  
   
   ```shell
   Mount success! fs_name:upfs-fsname version tag:public-v10.0-x86_64-skylake
   ```

## 步骤三、挂载状态查看

执行命令 ```df -h``` 即可查看挂载文件系统状态。若挂载成功，会看到upfs对于资源ID的条目，以及使用容量，挂载点等信息。

![](/images/upfs_guide/linux_mount1.png)




**如果挂载文件系统有问题，请及时联系UCloud技术支持。**
    