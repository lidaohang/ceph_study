## 1.背景说明
用户需要从一个文件从检索出一行数据， 并且对延迟和性能要求比较高。

### 1.1 现在做法 
普通的做法是A用户上传2G的文件到Ceph集群， B用户拉取该2G的文件到本地，然后根据offset检索这行数据。

这其中的做法是用户检索量比较大的时候，都需要拉取不同文件的2G数据，延迟比较高，严重影响用户的体验。

**缺点：**
- 用户端检索延迟大，影响用户体验
- 客户端端集群网卡带宽有限
- 量大对Ceph集群负载影响严重
 

### 1.2 优化方案
能不能只拉取我需要的信息，不用全量拉取到本地，答案是肯定的。

**思路：**
- 根据文件信息查找所有的object信息
- 根据offset找到需要检索的object信息
- 找到对应的object 检索一行的数据（一行数据可能会拆分多个object)

**优点：**
- 提升用户体验
- 客户端网络网卡带宽可用率得到提升
- 减少对ceph集群的冲击影响

## 2. 方案逻辑
### 2.1查看文件映射object信息
```
root@xxx$ cephfs /mnt/kernel_log_push.log map
WARNING: This tool is deprecated.  Use the layout.* xattrs to query and modify layouts.
    FILE OFFSET                    OBJECT        OFFSET        LENGTH  OSD
              0      10002b63282.00000000             0       4194304  61
        4194304      10002b63282.00000001             0       4194304  67
        8388608      10002b63282.00000002             0       4194304  70
       12582912      10002b63282.00000003             0       4194304  68
```
### 2.2根据offset查找object信息
```
root@xxx$ cephfs /mnt/kernel_log_push.log show_location -l 12582912
WARNING: This tool is deprecated.  Use the layout.* xattrs to query and modify layouts.
location.file_offset:  12582912
location.object_offset:0
location.object_no:    3
location.object_size:  4194304
location.object_name:  10002b63282.00000003
location.block_offset: 0
location.block_size:   4194304
location.osd:          68
```
### 2.3获取这个对象信息
```
#这是整个4M读取，有点浪费资源，不推荐
root@xxx$ rados -p cephfs_data get 10002b63282.00000003 test
```

### 2.4获取这个对象的某行数据
```
/*
问题点：
1. 一行数据可能会拆分为两个对象
解决方案：
用户给的offset属于这一行的结尾， 只需要读取上一行是否存在\n，
如果存在\n证明该行，属于完整的行。
否则不存在\n证明该行，被拆分为两个对象，在上一个对象的结尾offset往上读，直到遇到\n 合并两个对象读取的数据为完整的行。
*/
#Contents of object 'hw'
print ioctx.read("hw",length=int,offset=int) #length 默认是对象长度，offset默认是0
```
