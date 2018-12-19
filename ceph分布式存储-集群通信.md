# 1. 介绍
网络通信框架三种不同的实现方式：
 - Simple是比较简单，比较稳定的实现。系统默认用于生产环境的方式。特点是：每一个网络链接，都会创建两个线程，一个专门用于接收，一个专门用于发送。这种模式实现比较简单，但是对于大规模的集群部署，大量的链接会产生大量的线程，会消耗CPU资源，影响性能。
 - Async模式使用了基于事件的I/O多路复用模式。这种是目前网络通信中广泛采用的方式。k版默认已经使用Asnyc了。
 - XIO方式使用了开源的网络通信库accelio来实现。这种方式需要依赖第三方的库accelio稳定性，需要对accelio的使用方式以及代码实现都比较熟悉。目前处于试验阶段。

# 2. 模式
**设计模式(Subscribe/Publish)**
订阅发布模式又名观察者模式，它意图是“定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新”。
![image.png](https://upload-images.jianshu.io/upload_images/2099201-7c2975c02feb08e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 - Accepter监听 peer 的请求, 有新请求时, 调用 SimpleMessenger::add_accept_pipe() 创建新的 Pipe 到 SimpleMessenger::pipes 来处理该请求
 - Pipe用于消息的读取和发送。该类主要有两个组件，Pipe::Reader，Pipe::Writer用来处理消息读取和发送
 - Messenger作为消息的发布者, 各个 Dispatcher 子类作为消息的订阅者, Messenger 收到消息之后,通过 Pipe 读取消息,然后转给 Dispatcher 处理
 - Dispatcher是订阅者的基类，具体的订阅后端继承该类,初始化的时候通过Messenger::add_dispatcher_tail/head注册到 Messenger::dispatchers. 收到消息后通知该类处理
 - DispatchQueue该类用来缓存收到的消息, 然后唤醒 DispatchQueue::dispatch_thread 线程找到后端的 Dispatch 处理消息

**SimpleMessenger数据结构：**
![image.png](https://upload-images.jianshu.io/upload_images/2099201-3148e27eb879d7d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**SimpleMessenger详细解析：**
![image.png](https://upload-images.jianshu.io/upload_images/2099201-9eedad8c8ced5582.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



