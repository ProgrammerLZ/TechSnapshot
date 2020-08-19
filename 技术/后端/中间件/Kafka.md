## 相关问题

* 卡夫卡把消息写进了分区，然后顺序读写，这个分区是什么？
* 软连接的本质是什么？
* 





## 关键词

kafka的特点：**高吞吐量**





## Kafka集群搭建（虚拟机搭建）

#### 准备

macos的hosts文件的位置

> /pricate/etc/hosts

centos7的网络配置文件

> /etc/sysconfig/network-scripts/ifcfg-ens33

设置静态IP

> ```
> ONBOOT=yes
> BOOTPROTO=static
> IPADDR=192.168.1.111
> NETMASK=255.255.255.0
> GATEWAY=192.168.1.1
> ```

**在配置centos7静态IP的过程当中，发现自己的计算机网络知识相对较薄弱，在完成主线知识的学习之后，需要对这部分知识进行补充，在Nigix学习笔记当中，存放了一篇计算机网络的帖子的网络链接，之后可以到那里去查。**





​	