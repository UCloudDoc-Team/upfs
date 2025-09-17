# 挂载文件系统(K8s)

挂载须知：

   1.若使用的K8s产品为UCloud官方提供的UK8S产品服务，具体挂载方式请参考[UK8S挂载UPFS](https://docs.ucloud.cn/uk8s/volume/upfs)。

   2.若使用的K8s产品为自建K8s服务， 请阅读以下注意事项，其他步骤同UK8s。

## 自建组件安装注意点

- Pod网段，Service网段不能与公共服务网段重叠，各地域公共服务网段请参阅 [公共服务网段](https://docs.ucloud.cn/vpc/limit)
- 如果节点上的kubelet目录与默认（`/var/lib/kubelet`）不一致，需要替换以下文件中的 `/var/lib/kubelet` 部分为实际kubelet目录
  - uk8s-plugin/upfs/csi-node.yml
  - uk8s-plugin/upfs/csi-controller.yml
- 为了下载UPFS客户端，集群需要访问公网。若集群无法访问公网，需要修改`upfs/csi-node.yaml`文件，修改变量`.spec.template.spec.initContainers.env.  UPFS_CLIENT_DOWNLOAD_URL` 对应的value值为当前集群可下载的地址。譬如在 uhost 上的自建集群，可通过[UPFS文档](https://docs.ucloud.cn/upfs/upfs_guide/client_install)获取内网下载地址；
```
spec:
  template:
    spec:
      initContainers:
        - name: upfslauncher-installer
          env:
            - name: UPFS_CLIENT_DOWNLOAD_URL
              value: https://upfs.cn-sh2.ufileos.com/upfs_client-xxx.tar.gz #修改为集群可访问的下载地址
```

**如果挂载文件系统有问题，请及时联系UCloud技术支持。**