# flink集群搭建

- 摘自 https://blog.csdn.net/zhou_shaowei/article/details/76258240

## 下载flink
由于我没有使用haddop所以下载无hadoop的flink版本：flink-1.5.0-bin-scala_2.11.tgz
http://www.apache.org/dyn/closer.lua/flink/flink-1.5.0/flink-1.5.0-bin-scala_2.11.tgz

## 环境准备
- 准备三台服务器(192.168.1.60,192.168.1.62,192.168.1.63)
- 搭建zookeeper集群
- 规划一台master(192.168.1.60)和两台slave(192.168.1.62,192.168.1.63)，并且配置master到slave的ssh免密登录

## 配置master节点
选择一个 master节点(JobManager)然后在conf/flink-conf.yaml中设置jobmanager.rpc.address
配置项为该节点的IP 或者主机名。确保所有节点有有一样的jobmanager.rpc.address 配置。

```
192.168.1.60:8081
```
## 配置slaves节点
将所有的 worker 节点 （TaskManager）的IP 或者主机名（一行一个）填入conf/slaves 文件中。

```
1921.68.1.62
192.168.1.63
```
## 配置zookeeper
在conf/zoo.cfg中配置正确的zookeeper地址

```
clientPort=12181

# ZooKeeper quorum peers
server.1=192.168.1.60:12888:13888
server.2=192.168.1.62:12888:13888
server.3=192.168.1.63:12888:13888

```

## 启动集群
- 在master节点的flink目录下运行/bin/start-cluster.sh
