优化操作系统的参数可以充分利用硬件的性能。
| 组件 | 配置 |
|:---:|:----:|
| CPU | 关闭CPU节能模式 |
| CPU | 使用Cgroup绑定Ceph OSD进程到固定的CPU |
| RAM | 关闭NUMA |
| RAM | 关闭虚拟内存 |
| 网卡 | 设置为大帧模式 |
| SSD  | 分区4k对齐 |
| SSD  | 调度算法为noop |
| SATA/SAS | 调度算法为deadline |
| 文件系统 | 使用XFS |
| 文件系统 | 挂载参数为noatime |
