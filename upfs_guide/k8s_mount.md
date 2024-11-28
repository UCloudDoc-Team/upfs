# 挂载文件系统(K8s)

挂载须知：
   1.若使用的K8s产品为UCloud官方提供的UK8S产品服务，具体挂载方式请参考[UK8S挂载UPFS](https://docs.ucloud.cn/uk8s/volume/upfs)。
   2.若使用的K8s产品为自建K8s服务，具体挂载方式请参考下述挂载步骤。

## 步骤一、安装客户端

K8s方式挂载时，需要先在K8s所有的 work node上安装好UPFS的客户端，客户端安装步骤请查看[UPFS客户端安装](/upfs/upfs_guide/client_install.md)。


## 步骤二、安装csi工具包
  
  需要在master节点安装csi工具包，详细步骤请见下文。

  1. 获取csi工具包，在获取csi工具包前，先确认当前K8s集群配置了弹性外网Eip，确认无误后执行以下命令获取csi工具包。

```shell
wget https://upfs-public.cn-bj.ufileos.com/csi-upfs.tar.gz
```

  2. 执行以下命令解压csi工具包。

```shell
tar zxvf csi-upfs.tar.gz
```
  3. 在安装kubectl的node下执行以下命令安装csi工具包。

```shell
kubectl apply -f rbac-controller.yml
kubectl apply -f rbac-node.yml
kubectl apply -f csi-controller.yml
kubectl apply -f csi-node.yml
```
  4. 执行以下命令验证是否成功.

```shell
kubectl get po -A | grep upfs
```

   如果有如下图片展示POD的信息，且状态为 Running，说明安装成功。

   ![](/images/upfs_guide/k8s_mount1.png)

## 步骤三、创建UPFS文件系统K8s集群配置文件

  需要在master节点创建UPFS文件系统K8s集群配置文件，详细步骤请见下文。

  1. 这里需要创建StorageClass和PVC，配置文件参考下面的内容，并为配置文件命名 ```storageclass.yaml```（文件名可自定义），需要注意以下几点：
  
     - uri：文件系统URL（URL详细规则请见[主要概念](/upfs/upfs_manual_instruction/concept)中的文件系统URL部分）
     - path：容器挂载UPFS文件系统的子目录，默认为 ```/```


```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: csi-upfs
provisioner: upfs.csi.ucloud.cn
parameters:
  uri: 100.64.240.97:10109,100.64.240.95:10109/upfs-y0gprzs5a12
  path: /mnt
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: logupfs-claim
spec:
  storageClassName: csi-upfs
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi  # UPFS存储空间理论上线较高，所以PV和PVC中的容量参数没有实际意义，这里的requests信息不会真实生效
```

  2. 执行以下命令创建StorageClass和PVC，

```shell
kubectl apply -f storageclass.yaml
```

## 步骤四、创建POD使用UPFS文件系统

 需要在master节点创建POD使用UPFS文件系统，详细步骤请见下文。

 1. 创建一个POD来使用UPFS文件系统，配置文件内容参考如下，并为配置文件命名```pod.yaml```（文件名可自定义），需要注意以下几点：

   - containers: 配置信息，根据实际情况配置
   - mountPath: containers⾥挂载UPFS的路径
   - claimName: 这⾥需要指定为步骤三中创建的UPFS PVC的名称

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-upfs
spec:
  containers:
    - name: nginx
      image: uhub.service.ucloud.cn/ucloud/nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: test
          mountPath: /data
  volumes:
    - name: test
      persistentVolumeClaim:
        claimName: logupfs-claim
```

 2. 执行以下命令创建POD：

```shell
kubectl apply -f pod.yaml
```
UPFS支持多POD挂载访问UPFS文件系统，同理创建多POD配置文件如上。

## 步骤五、验证POD是否正常运行

 1. 执行以下命令进入上一步创建的POD。

```shell
kubectl exec -it <POD名字> /bin/bash
```
 2. 执行以下命令查看文件系统是否挂载，如下图，如果图中出现步骤三中需要挂载的文件系统资源ID表示挂载成功。

```shell
kubectl apply -f pod.yaml
```
![](/images/upfs_guide/k8s_mount2.png)

**如果挂载文件系统有问题，请及时联系UCloud技术支持。**