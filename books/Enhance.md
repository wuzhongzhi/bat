```
说说NIO的实现原理:
Java的NIO主要由三个核心部分组成：Channel、Buffer、Selector。
所有的IO在NIO中都从一个Channel开始，数据可以从Channel读到Buffer中，也可以从Buffer写到Channel中
Buffer本质上是一块可以写入数据，然后可以从中读取数据的内存.
要使用Selector，得向Selector注册Channel，然后调用它的select()方法。这个方法会一直阻塞到某个注册的通道有事件就绪。
一旦这个方法返回，线程就可以处理这些事件，事件例如有新连接进来，数据接收等。
```
```
数据库索引失效了怎么办？
可以采用以下几种方式，来避免索引失效：
使用组合索引时，需要遵循“最左前缀”原则；
不在索引列上做任何操作，例如计算、函数、类型转换，会导致索引失效而转向全表扫描；
尽量使用覆盖索引（之访问索引列的查询），减少 select * 覆盖索引能减少回表次数；
MySQL在使用不等于（!=或者<>）的时候无法使用索引会导致全表扫描；
LIKE以通配符开头（%abc）MySQL索引会失效变成全表扫描的操作；
字符串不加单引号会导致索引失效（可能发生了索引列的隐式转换）；
少用or，用它来连接时会索引失效。
```
```
索引的实现原理
在MySQL中，索引是在存储引擎层实现的，不同存储引擎对索引的实现方式是不同的，下面我们探讨一下MyISAM和InnoDB两个存储引擎的索引实现方式。
MyISAM索引实现 ：
MyISAM引擎使用B+Tree作为索引结构，叶节点的data域存放的是数据记录的地址
第一个重大区别是InnoDB的数据文件本身就是索引文件。从上文知道，MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。
而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。
```
```
MySQL的索引为什么用B+树？
B+树由B树和索引顺序访问方法演化而来，它是为磁盘或其他直接存取辅助设备设计的一种平衡查找树，在B+树中，所有记录节点都是按键值的大小顺序存放在同一层的叶子节点，各叶子节点通过指针进行链接。如下图：
B+树索引在数据库中的一个特点就是高扇出性，例如在InnoDB存储引擎中，每个页的大小为16KB。在数据库中，B+树的高度一般都在2～4层，这意味着查找某一键值最多只需要2到4次IO操作，这还不错。
因为现在一般的磁盘每秒至少可以做100次IO操作，2～4次的IO操作意味着查询时间只需0.02～0.04秒。
```
```
InnoDB存储引擎有3种行锁的算法，其分别是：
Record Lock：单个行记录上的锁。通过给索引上的索引项加锁来实现的。只有通过索引条件检索数据，InnoDB才使用行级锁，否则，InnoDB将使用表锁。
Gap Lock：间隙锁，锁定一个范围，但不包含记录本身。 它的作用是为了阻止多个事务将记录插入到同一范围内，而这会导致幻读问题的产生。
Next-Key Lock∶Gap Lock+Record Lock，锁定一个范围，并且锁定记录本身。
```
```
可以快速构建项目；可以对主流开发框架的无配置集成；项目可独立运行，无需外部依赖Servlet容器；提供运行时的应用监控；可以极大地提高开发、部署效率；可以与云计算天然集成。
```
```
整个自动装配的过程是：Spring Boot通过@EnableAutoConfiguration注解开启自动配置，加载spring.factories中注册的各种AutoConfiguration类，
当某个AutoConfiguration类满足其注解@Conditional指定的生效条件（Starters提供的依赖、配置或Spring容器中是否存在某个Bean等）时，
实例化该AutoConfiguration类中定义的Bean（组件等），并注入Spring容器，就可以完成依赖框架的自动配置。
```
```
这三级缓存的作用分别是：
singletonFactories ： 进入实例化阶段的单例对象工厂的cache （三级缓存）；
earlySingletonObjects ：完成实例化但是尚未初始化的，提前暴光的单例对象的Cache （二级缓存）；
singletonObjects：完成初始化的单例对象的cache（一级缓存）。
```
```
Redis的主从同步是如何实现的？
同步过程分为全量复制和部分复制。
复制偏移量：主节点处理写命令后，会把命令长度做累加记录，从节点在接收到写命令后，也会做累加记录；从节点会每秒钟上报一次自身的复制偏移量给主节点，而主节点则会保存从节点的复制偏移量。
积压缓冲区：保存在主节点上的一个固定长度的队列，默认大小为1M，当主节点有连接的从节点时被创建；主节点处理写命令时，不但会把命令发送给从节点，还会写入积压缓冲区；
缓冲区是先进先出的队列，可以保存最近已复制的数据，用于部分复制和命令丢失的数据补救。
```
```
redis string 命令有：SETNX SETEX MSET MSETNX DECR DECRBY INCR
```
```
三次握手：
第一次握手：建立连接时，客户端发送syn包（syn=x）到服务器，并进入 SYN_SENT 状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。
第二次握手：服务器收到syn包，必须确认客户的SYN（ack=x+1），同时自己也发送一个SYN包（syn=y），即SYN+ACK包，此时服务器进入 SYN_RECV 状态；
第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=y+1），此包发送完毕，客户端和服务器进入 ESTABLISHED （TCP连接成功）状态，完成三次握手。
四次挥手过程
（1）客户端向服务器发送FIN控制报文段（首部中的 FIN 比特被置位）；
（2）服务端收到FIN，回复ACK。服务器进入关闭等待状态，发送FIN;
（3）客户端收到FIN，给服务器回复ACK，客户端进入等待状态（进入“等待”，以确保服务器收到ACK真正关闭连接）;
（4）服务端收到ACK，链接关闭
```
```
处理粘包的方法如下：
（1）发送方通过关闭Nagle算法来解决，使用TCP_NODELAY选项来关闭算法。
（2）接收方没将问题交给应用层来处理。。解决办法：循环处理，应用程序从接收缓存中读取分组时，读完一条数据，就应该循环读取下一条数据，直到所有数据都被处理完成，判断每条数据的长度的方法有两种：
a. 格式化数据：每条数据有固定的格式（开始符，结束符），这种方法简单易行，但是选择开始符和结束符时一定要确保每条数据的内部不包含开始符和结束符。
b. 发送长度：发送每条数据时，将数据的长度一并发送，例如规定数据的前4位是数据的长度，应用层在处理时可以根据长度来判断每个分组的开始和结束位置。
```
```
400 (错误请求)401 (未授权) 403 (已禁止) 服务器拒绝请求。404 (未找到) 服务器找不到请求的网页。405 (方法禁用) 禁用请求中所指定的方法。406 (不接受) 无法使用请求的内容特性来响应请求的网页。
407 (需要代理授权) 此状态代码与 401(未授权)类似，。408 (请求超时) 服务器等候请求时超时。409 (冲突) 服务器在完成请求时发生冲突。410 (已删除) 如果请求的资源已被永久删除。411 (需要有效长度)
412 (未满足前提条件) 413 (请求实体过大) 414 (请求的 URI 过长) 415 (不支持的媒体类型) 416 (请求范围不符合要求) 417 (未满足期望值) 
500 (服务器内部错误) 501 (尚未实施) 服务器不具备完成请求的功能502 (错误网关) 503 (服务不可用) 504 (网关超时) 505 (HTTP 版本不受支持) 
```
```
线程和进程的区别？（1）一个线程从属于一个进程；一个进程可以包含多个线程。
（2）一个线程挂掉，对应的进程挂掉；一个进程挂掉，不会影响其他进程。
（3）进程是系统资源调度的最小单位；线程CPU调度的最小单位。
（5）进程在执行时拥有独立的内存单元，多个线程共享进程的内存，如代码段、数据段、扩展段；但每个线程拥有自己的栈段和寄存器组。
（6）进程切换时需要刷新TLB并获取新的地址空间，然后切换硬件上下文和内核栈，线程切换时只需要切换硬件上下文和内核栈。
（7）通信方式不一样。（8）进程适应于多核、多机分布；线程适用于多核
```
```
进程间通信主要有以下几种方式：匿名管道、命名管道、信号、消息队列、共享内存、信号量、Socket。
```
```
线程之间的通信方式。
锁机制：包括互斥锁、条件变量、读写锁互斥锁提供了以排他方式防止数据结构被并发修改的方法。读写锁允许多个线程同时读共享数据，而对写操作是互斥的。条件变量可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。
信号量机制(Semaphore)：包括无名线程信号量和命名线程信号量
信号机制(Signal)：类似进程间的信号处理线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。
```
```
IO多路复用有三种实现方式:select, poll, epoll
(1)select：时间复杂度O(n)，它仅仅知道了，有I/O事件发生了，只能无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作。
(2)poll：时间复杂度O(n)，poll本质上和select没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态， 但是它没有最大连接数的限制 ，原因是它是基于链表来存储的。
(3)epoll：时间复杂度O(1)， epoll可以理解为event poll ，不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎样的I/O事件通知我们。所以说epoll实际上是 事件驱动（每个事件关联上fd） 的.
```
```
虚拟内存 则是指将硬盘的一块区域划分来作为内存。分别为 栈区、堆区、全局区（静态区）、文字常量区（常量存储区）、程序代码区
内存映射（mmap） 是一种内存映射文件的方法，即将一个文件或者其他对象映射到进程的地址空间，实现文件磁盘地址和应用程序进程虚拟地址空间中一段虚拟地址的一一映射关系。
实现这样的映射关系后，进程就可以采用指针的方式读写操作这一段内存
```
