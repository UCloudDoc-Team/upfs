# 卸载文件系统(K8s)

卸载须知：
  1.若使用的K8s产品为UCloud官方提供的UK8S产品服务，具体卸载方式请参考[UK8S卸载UPFS](https://docs.ucloud.cn/uk8s/volume/upfs)。
  2.若使用的K8s产品为自建K8s服务，具体卸载方式请参考下述卸载步骤。

UPFS系统通过K8s方式挂载文件系统，支持多POD挂载访问使用文件系统，如确定该POD下不再使用文件系统可通过执行下述删除POD指令来达到卸载UPFS文件系统的目的。

```shell
kubectl delete pod <pod名字>
```

 注意事项：
  - 鉴于K8s集群上卸载UPFS文件系统属高危操作，公测期目前删除所有POD，并不能使UPFS文件系统从云主机侧卸载。
  - 如果确认文件系统不再使用，需要从云主机卸载UPFS文件系统，请及时联系UCloud技术支持。

**如果卸载文件系统有问题，请及时联系UCloud技术支持。**