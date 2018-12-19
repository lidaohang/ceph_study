# 1. 心跳介绍
心跳是用于节点间检测对方是否故障的，以便及时发现故障节点进入相应的故障处理流程。

**问题：**
 - 故障检测时间和心跳报文带来的负载之间做权衡。
 - 心跳频率太高则过多的心跳报文会影响系统性能。
 - 心跳频率过低则会延长发现故障节点的时间，从而影响系统的可用性。

**故障检测策略应该能够做到：**
 - 及时：节点发生异常如宕机或网络中断时，集群可以在可接受的时间范围内感知。
 - 适当的压力：包括对节点的压力，和对网络的压力。
 - 容忍网络抖动：网络偶尔延迟。
 - 扩散机制：节点存活状态改变导致的元信息变化需要通过某种机制扩散到整个集群。

# 2. Ceph 心跳检测
![ceph_heartbeat_1.png](https://upload-images.jianshu.io/upload_images/2099201-797b8f8c9e2de4d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**OSD节点会监听public、cluster、front和back四个端口**
 - public端口：监听来自Monitor和Client的连接。
 - cluster端口：监听来自OSD Peer的连接。
 - front端口：供客户端连接集群使用的网卡, 这里临时给集群内部之间进行心跳。
 - back端口：供客集群内部使用的网卡。集群内部之间进行心跳。
 - hbclient：发送ping心跳的messenger。

# 3. Ceph OSD之间相互心跳检测
![ceph_heartbeat_osd.png](https://upload-images.jianshu.io/upload_images/2099201-a04c96ba04ec47df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**步骤：**
 - 同一个PG内OSD互相心跳，他们互相发送PING/PONG信息。
 - 每隔6s检测一次(实际会在这个基础上加一个随机时间来避免峰值)。
 - 20s没有检测到心跳回复，加入failure队列。


# 4. Ceph OSD与Mon心跳检测
![ceph_heartbeat_mon.png](https://upload-images.jianshu.io/upload_images/2099201-06fcd181ba5c2671.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**OSD报告给Monitor：**
 - OSD有事件发生时（比如故障、PG变更）。
 - 自身启动5秒内。
 - OSD周期性的上报给Monito
   - OSD检查failure_queue中的伙伴OSD失败信息。
   - 向Monitor发送失效报告，并将失败信息加入failure_pending队列，然后将其从failure_queue移除。
   - 收到来自failure_queue或者failure_pending中的OSD的心跳时，将其从两个队列中移除，并告知Monitor取消之前的失效报告。
   - 当发生与Monitor网络重连时，会将failure_pending中的错误报告加回到failure_queue中，并再次发送给Monitor。
 - Monitor统计下线OSD
   - Monitor收集来自OSD的伙伴失效报告。
   - 当错误报告指向的OSD失效超过一定阈值，且有足够多的OSD报告其失效时，将该OSD下线。


# 5. Ceph心跳检测总结
Ceph通过伙伴OSD汇报失效节点和Monitor统计来自OSD的心跳两种方式判定OSD节点失效。
 - **及时：**伙伴OSD可以在秒级发现节点失效并汇报Monitor，并在几分钟内由Monitor将失效OSD下线。
 - **适当的压力：**由于有伙伴OSD汇报机制，Monitor与OSD之间的心跳统计更像是一种保险措施，因此OSD向Monitor发送心跳的间隔可以长达600秒，Monitor的检测阈值也可以长达900秒。Ceph实际上是将故障检测过程中中心节点的压力分散到所有的OSD上，以此提高中心节点Monitor的可靠性，进而提高整个集群的可扩展性。
 - **容忍网络抖动：**Monitor收到OSD对其伙伴OSD的汇报后，并没有马上将目标OSD下线，而是周期性的等待几个条件：
    - 目标OSD的失效时间大于通过固定量osd_heartbeat_grace和历史网络条件动态确定的阈值。
    - 来自不同主机的汇报达到mon_osd_min_down_reporters。
    - 满足前两个条件前失效汇报没有被源OSD取消。
 - **扩散：**作为中心节点的Monitor并没有在更新OSDMap后尝试广播通知所有的OSD和Client，而是惰性的等待OSD和Client来获取。以此来减少Monitor压力并简化交互逻辑。
