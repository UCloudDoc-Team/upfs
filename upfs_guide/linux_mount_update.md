# UPFS 客户端升级与多命名空间挂载操作指南

> 适用于：需要在 Linux 主机上定位、升级 UPFS 客户端，并在多 **mount namespace** 场景下查看/汇总 UPFS 挂载点的场合。

---

## 1. 定位当前 UPFS 客户端与挂载参数

### 1.1 获取 UPFS 客户端进程与二进制路径
通过进程列表与 `/proc/<pid>/exe` 可定位到客户端二进制文件，进而查看版本与启动参数（包含挂载点路径、读写权限、`upfs-id` 等）：

```bash
ps aux | grep upfs_client

readlink -f /proc/<pid>/exe
```

> 一般情况下，`ps aux` 的 `CMD` 字段就能看到 UPFS 客户端的启动参数；`readlink` 可以确认实际落地的二进制以便查看版本。

---

## 2. 查看 UPFS 挂载点（单命名空间与全局）

### 2.1 仅查看当前命名空间下的 UPFS 挂载
`df` 仅展示**当前进程所在 mount namespace** 的挂载，因此默认只能看到本命名空间下的 UPFS：

```bash
df -h | grep -i UPFS
```

### 2.2 在多命名空间共享挂载的场景
实际使用时，可能会：
- 创建新的 **mount namespace**；
- 通过 **bind mount** 在这些命名空间下创建新的挂载点；
- **不重复**拉起 `upfs_client` 进程，而是**复用**原命名空间已拉起的客户端，实现**共享挂载**。

此时，用 `df` 在宿主/当前命名空间往往看不到其它命名空间的 UPFS 挂载点。

### 2.3 获取所有命名空间下的 UPFS 挂载点（推荐脚本）
遍历所有 `mnt` 命名空间，读取各自的 `/proc/<pid>/mounts`，并仅输出包含 `UPFS` 的条目，同时打印对应的 `PID`：

```bash
lsns -t mnt | awk 'NR>1 && $4 ~ /^[0-9]+$/ {print $4}' | while read -r pid; do
[ -r "/proc/$pid/mounts" ] || continue
grep "UPFS" "/proc/$pid/mounts" 2>/dev/null | sed "s/^/PID $pid: /"
done
```

输出格式如下
```bash
PID 551864: UPFS:upfs-xxxx /namespace/mnt/ fuse.UPFS:upfs-xxxxx rw,nodev,relatime,user_id=0,group_id=0,default_permissions,allow_other 0 0
```

> 说明：  
> - `lsns -t mnt` 罗列系统中的 mount namespaces；  
> - 上述脚本逐个读取各命名空间代表进程的 `/proc/<pid>/mounts`；  
> - 仅输出包含 `UPFS` 的挂载行，并加上前缀 `PID <pid>:` 方便定位。

---

## 3. 卸载 UPFS 的注意事项

1. **只卸载本地（当前）命名空间的挂载点**时，可能会看到：  
- 本命名空间的挂载点被卸载成功；  
- 其它命名空间的 UPFS 挂载点仍然存在（用上面的“全局查看”脚本能看到）；  
- `upfs_client` 进程**未**退出。

2. 当**其它命名空间的相关进程停止运行**后（不再持有共享挂载），可观察到 `upfs_client` 会**自动退出**。

> 这符合共享挂载的持有/引用语义：只要仍有命名空间使用该挂载，用户态 FUSE 客户端就需要继续存活。

---

## 4. 挂载新版本 UPFS（升级流程）

### 4.1 获取并安装新客户端
安装最新版本客户端，参考[UPFS客户端安装](/upfs/upfs_guide/client_install.md)。

### 4.2 使用原挂载参数启动新版本客户端
按照**原有挂载参数**（挂载点路径、读写权限、`upfs-id` 等）执行相应的 `mount` 命令，启动新版本 `upfs_client`，完成升级与切换。

---

## 附：常见命令速查

```bash
# 进程与二进制定位
ps aux | grep upfs_client
readlink -f /proc/<pid>/exe

# 当前命名空间的 UPFS 挂载
df -h | grep -i UPFS

# 全部命名空间下的 UPFS 挂载（带 PID 前缀）
lsns -t mnt | awk 'NR>1 && $4 ~ /^[0-9]+$/ {print $4}' | while read -r pid; do
[ -r "/proc/$pid/mounts" ] || continue
grep "UPFS" "/proc/$pid/mounts" 2>/dev/null | sed "s/^/PID $pid: /"
done
```
