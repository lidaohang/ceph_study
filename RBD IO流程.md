# 1. rbd 读写流程 
## 1.1 客户端使用rbd设备的方式
 - kernel rbd：创建了rbd设备后，把rbd设备map到内核中，形成一个虚拟的 块设备。
 - librbd：创建了rbd设备后，这时可以使用librbd、librados库进行访问管理块设备。

## 1.2 流程介绍
 ![image.png](https://upload-images.jianshu.io/upload_images/2099201-1ad041233e603bf1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 客户端创建 一个pool，需要为这个pool指定pg的数量
2. 在这个pool中创建一个rbd设备rbd0，那么这个rbd0都会保存三份，在创建rbd0时必须指定rbd的size，对于这个rbd0的任何操作不能超过这个size
3. 将这个块设备进行切块，每个块的大小默认为4M，并且每个块都有一个名字，名字就是object+序号
4. 将每个object通过pg进行副本位置的分配
5. pg根据cursh算法会寻找3个osd，把这个object分别保存在这三个osd上
6. osd上实际是把底层的disk进行了格式化操作，一般部署工具会将它格式化为xfs文件系统（这个是可选的，ext4也是可以配置的)
7. object的存储就变成了存储一个文rbd0.object1.file

## 1.3 客户端写数据osd过程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-18be62eb88f3515f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 假设这次采用的是librbd的形式，使用librbd创建一个块设备，这时向这个块设备中写入数据
2. 在客户端本地同过调用librados接口，然后经过pool，rbd，object、pg进行层层映射,在PG这一层中，
可以知道数据保存在哪3个OSD上，这3个OSD分为主从的关系，也就是一个primary OSD，两个replica OSD
3. 客户端与primay OSD建立SOCKET 通信，将要写入的数据传给primary OSD，由primary OSD再将数据发送给其他replica OSD数据节点

## 1.4 rbd client 端的数据请求流程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-abfd8cb6278d77ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.5 osd数据处理流程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-1b595b941f526d7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/2099201-d02b0db5cf9821b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/2099201-13c049c4acc24da9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/2099201-0b4740beb75bc290.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/2099201-067e877e253e0670.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
