# 其他功能

### Upfsiostat监控工具

为更好的监控客户端和UPFS文件系统之间的I/O和MetaData的数据传输速率、延时、操作数量等信息，UCLOUD提供了Upfsiostat性能监控工具，Upfsiostat是一个用于监视和报告UPFS文件系统I/O和MetaData性能的工具。通过Upfsiostat工具，用户可以直观了解到UPFS文件系统的性能状况。

如需要提供其他客户端监控功能请联系技术支持。

#### 使用方式

Upfsiostat工具随客户端一起被安装，可以直接在目标机器上运行。
可在目标机器上运行如下命令：

       upfsiostat [options] [ mountpoint ]

mountpoint为UPFS文件系统资源ID

options可支持如下参数

| 参数列表    | 含义     |
|---------|--------|
| -f 或者 --file	 | 显示文件操作的统计	   |
| -d 或者 --dir	 | 显示目录操作的统计	 |
| -a 或者 --all	 | 显示文件和目录的操作统计	  |
| -t 或者 --time	 | 显示获取统计的时间	   |
| -h 或者 --help	 | 显示帮助信息	 |
| --version	 | 显示工具的版本	  |

#### 信息说明

运行相应命令，通过Upfsiostat性能监控工具进行查询，可得到如下图信息

![](/images/other1.png)


其中每项指标项含义说明请查看下表

| 指标名称          | 含义     |
|---------------|--------|
| idx/s	 | 每秒元数据的调用次数	   |
| idx-retry/s	  | 每秒元数据重试的次数	 |
| io-retry/s	  | 每秒io重试的次数	  |
| ops/s	 | 每秒操作的次数	   |
| kB/s	 | 每秒读/写操作的io数据量	 |
| avg RTT (ms)	    | 读/写操作存储服务处理的时延(ms)	  |
| avg LAT (ms)    | 操作处理的总时延(ms)  |