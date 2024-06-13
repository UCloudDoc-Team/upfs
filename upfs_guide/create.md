

# 创建文件系统

## 步骤一、进入文件存储 UPFS 产品

![](/images/upfs_guide/create1.png)

## 步骤二、在控制台页面选择创建文件存储的目标地域和项目组

目前文件存储在各地域的支持详情请参见[产品限制](/upfs/upfs_manual_instruction/limit)章节或咨询技术支持。由于文件系统的访问是基于 VPC 的，并且目前在控制台上只允许选择和文件系统实例同一个项目组下的 VPC 进行访问，请您确定需要访问文件系统的主机所在项目组，并将文件系统实例创建在同项目中。

![](/images/upfs_guide/create2.png)


## 步骤三、创建文件系统

在创建页面，选择业务所需要容量的文件系统，并命名，确认无误后可点击「立即购买」完成创建。

![](/images/upfs_guide/create3.png)

 创建文件系统注意以下几点：

   1. 目前 UPFS 仅支持POSIX协议，应用场景详情可参考[应用场景](/upfs/upfs_manual_instruction/application)章节。确认产品细节后点击『立即购买』并进行支付确认。
   2. 为保证 UPFS 达到最佳的性能体验，推荐UPFS和云主机UHost部署在同可用区。
   3. 创建时的容量限制可参考[产品限制](/upfs/upfs_manual_instruction/limit)章节。

## 步骤四、创建完成后展示实例列表

![](/images/upfs_guide/create4.png)

**如果创建文件系统有问题，请及时联系UCloud技术支持。**
