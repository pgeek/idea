### 问题

- Why producer need to use broker or brokers ?

我感觉应该kafka的设计者不想让用户直接和zookeeper打交道，毕竟zookeeper是用来管理kafka集群的，将来有可能不采用zookeeper，zookeeper竟来对用户来说是透明的。producer指定broker or brokers，这个broker会作为master，用来和客户端交互。

这个应该软件设计的角度去看，和hbase的想法不同，毕竟kafka当初由linkln开发

- Why consumer need to use zookeeper ?

目前两个原因：

（1）消费者以group为单位，本身需要管理这个消费集群

（2）kafka默认将消费者对partition的offset存到了zookeeper
