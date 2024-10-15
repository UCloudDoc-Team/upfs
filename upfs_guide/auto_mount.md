# 自动挂载文件系统
## 方式一：配置 /etc/fstab 文件设置自动挂载

### 步骤一，安装客户端

客户端安装步骤请查看[UPFS客户端安装](/upfs/upfs_guide/client_install.md)。


### 步骤二，开机自动挂载配置

编辑/etc/fstab文件，添加配置：

```
<mount_address1>,<mount_address2>/<resource_id>  /path/to/mount upfs _netdev 0 0
```

例如：

```
10.72.141.213:10109,10.72.141.215:10109/upfs /upfs_data upfs _netdev 0 0
```

|                   命令行字段                    |                             解释                             |
| :---------------------------------------------: | :----------------------------------------------------------: |
| <mount_address1>,<mount_address2>/<resource_id> | 从UCloud控制台页面处获取的文件系统URL （URL详细规则请见[主要概念](/upfs/upfs_manual_instruction/concept)中的文件系统URL部分） |
|                            /path/to/mount                    |                要挂载到的本地路径，请确保该目录没有被其它文件系统挂载                |
|              upfs                 |     表明挂载的文件系统类型是upfs       |          
|                  _netdev                   | 文件系统挂载选项，因为是网络文件系统，需要添加 _netdev，还支持 ro,rw |
|                     第一个0                     |    表示该文件系统是否会被 dump工具备份数据，请保持配置为0    |
|                     第二个0                     |      表示该文件系统是否会被 fsck命令检查，请保持配置为0      |

### 步骤三，挂载状态查看

配置文件后，执行命令`mount -a`,检查挂载状态
![](/images/upfs_guide/auto_mount1.png)

## 方式二：使用 systemd.mount 实现自动挂载

### 步骤一

参考方式一

### 步骤二，配置文件

创建文件 `/etc/systemd/system/upfs_data.mount`，并添加以下内容：

```
[Unit]
Description=Mount upfs at boot

[Mount]
What=10.72.141.213:10109,10.72.141.215:10109/upfs
Where=/upfs_data
Type=upfs
Options=_netdev
TimeoutSec=30

[Install]
WantedBy=multi-user.target
```
注意：mount 单元的名称必须根据其封装的文件系统挂载点路径命名。 例如 /upfs_data 挂载点对应的单元名称必须是 upfs_data.mount
部分字段解释：



|            字段            |                             解释                             |
| :------------------------: | :----------------------------------------------------------: |
|            What            |            从UCloud控制台页面处获取的文件系统URL             |
|           Where            |    要挂载到的本地路径，请确保该目录没有被其它文件系统挂载    |
|            Type            |                 表明挂载的文件系统类型是upfs                 |
|          Options           |    文件系统挂载选项，因为是网络文件系统，需要添加 _netdev    |
|         TimeoutSec         |              指定挂载操作的超时时间，单位为秒。              |
| WantedBy=multi-user.target | 指定该服务单元应该在哪个目标（target）下启动。`multi-user.target`表示在多用户模式下启动该服务 |

**自动挂载命令：**

```
systemctl enable upfs_data.mount
systemctl start upfs_data.mount
```

**如果挂载文件系统有问题，请及时联系UCloud技术支持。**
