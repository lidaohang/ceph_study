# 1. 管理存储池
## 1.1  创建存储池
```
#ceph osd pool create {pool-name} {pg-num} [{pgp-num}] [replicated] [crush-ruleset-name]
 
$ ceph osd pool create test_pool 512 512 replicated
pool 'test_pool' created
```
## 1.2  删除存储池
```
#ceph osd pool delete {pool-name} [{pool-name} --yes-i-really-really-mean-it]
 
$ ceph osd pool delete test_pool test_pool --yes-i-really-really-mean-it
pool 'test_pool' removed
```
## 1.3 重命名存储池
```
#ceph osd pool rename {current-pool-name} {new-pool-name}
 
$ ceph osd pool rename test_pool test_new_pool
pool 'test_pool' renamed to 'test_new_pool'
```
## 1.4 查看存储池列表
```
$ ceph osd lspools
1 rbd,2 test_data,3 test_metadata,5 test,6 benmark_test,7 .rgw.root,8 default.rgw.control,9 default.rgw.meta,10 default.rgw.log,11 default.rgw.buckets.index,12 web-services,13 test_pool,
```
# 2. 管理块设备镜像
## 2.1 创建块设备镜像
```
#rbd create --size {megabytes} {pool-name}/{image-name}，如果pool_name不指定，则默认的pool是rbd。 下面的命令将创建一个10GB大小的块设备：
 
$ rbd create --size 10240 test_image -p test_pool
```
## 2.2 删除块设备镜像
```
#rbd rm {pool-name}/{image-name}
 
$ rbd rm test_pool/test_image
```
## 2.3 查看块设备镜像
```
#rbd info {pool-name}/{image-name}
 
$ rbd info test_pool/test_image
rbd image 'test_image':
    size 10240 MB in 2560 objects
    order 22 (4096 kB objects)
    block_name_prefix: rbd_data.172e42ae8944a
    format: 2
    features: layering
    flags:
    create_timestamp: Wed Nov  8 17:50:34 2017
```
## 2.4 将块设备映射到系统内核
```
#sudo rbd map {image name} --name client.admin -m {monitor node ip or hostname} --cluster {cluster name}
 
$ sudo rbd map test_pool/test_image
/dev/rbd1
 
#如果打印：
rbd: sysfs write failed
RBD image feature set mismatch. You can disable features unsupported by the kernel with "rbd feature disable".
In some cases useful info is found in syslog - try "dmesg | tail" or so.
rbd: map failed: (6) No such device or address
 
 
#表示当前系统不支持feature，禁用当前系统内核不支持的feature：
rbd feature disable test_pool/test_image exclusive-lock, object-map, fast-diff, deep-flatten
 
dmesg
image uses unsupported features: 0x40
不支持特性 0x40 = 64，也就是不支持特性 journaling
 
#rbd-nbd用户态
yum install  kmod-nbd
yum  install rbd-nbd
sudo rbd-nbd map test_pool/test_image
```

## RBD特性解析

