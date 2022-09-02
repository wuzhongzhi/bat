### [《后端架构师技术图谱》 ](https://github.com/xingshaocheng/architect-awesome)  architect-awesome
### [Java 技术书籍大全](https://github.com/sorenduan/awesome-java-books)awesome-java-books
### [技术资讯](https://github.com/GitHubDaily/GitHubDaily)  GitHubDaily
### [互联网 Java 工程师进阶知识完全扫盲](https://github.com/doocs/advanced-java)  advanced-java
### [12-25Kjavaguide](https://github.com/Snailclimb/JavaGuide)  JavaGuide
### [技术心得](https://github.com/aalansehaiyang/technology-talk)  technology-talk


======================以下是好的文章=========================================
#### [全链路灰度在数据库上我们是怎么做的?](https://mp.weixin.qq.com/s/up8MKMVBTDte0mlnOAiJuw)
``` yaml配置灰度字段 微服务判断进行路由， curl -H 去验证```
#### [从业务开发中学习和理解架构设计](https://mp.weixin.qq.com/s/KLSUh7vvaxzlZY5rv9pWUA)
``` 架构设计一定要从业务场景出发,架构设计一定要落到业务场景中去验证
分层架构 六边形架构 洋葱圈架构（整洁架构）
DDD:1.领域：有确定的范围和边界的业务问题域。实际上是我们要解决什么业务问题的抽象描述。比如提供给用户当前位置、目的地位置且提供到达信息是高德地图的问题域。
2.子域：将大的问题域根据业务规则的不同拆分成的小问题域。比如高德地图的问题域太大了，难以解决。我们可以将问题域拆分成定位、POI搜索、路线规划等子问题域。
3.界限上下文：领域之间的抽象边界。封装了领域内的概念、规则和模型。
4.实体：具有唯一标识的、存在生命周期的对象。比如展示给用户可见的POI气泡是一个实体，它有状态和确定的生命周期。
5.值对象：没有唯一标识和生命周期的对象，依附于实体而存在。比如POI信息是值对象，本身没有状态，只能依附于POI气泡这个实体而存在。
6.聚合：领域内一组实体、值对象的集合。封装了集合与外界的交互
SRP 单一职责原则 OCP 开闭原则 LSP 里氏替换原则 ISP 接口隔离原则 DIP 依赖反转原则（依赖倒置） 奥卡姆剃刀原则
```
#### [工程效能CI/CD之流水线引擎的建设实践](https://mp.weixin.qq.com/s/dqsejshVzU7v79BuTSb8RA)
```
关键词：高效持续交付。
存在问题：建设成本搞、水平参差不齐，构建量快速增长，高峰期承受能力不足
发展历程：1.搭建jenkins集群 2.业务线维度拆分集群（造成管理难度增加）3.分布式流水线引擎，收敛业务依赖
主要挑战：1.调度效率低下 2.资源分配没有策略 3.工具差异化（接入形态的不同，shell组件、同期组件、服务组件）4.场景丰富（交互、重试、异步、故障恢复）
解决方案：
1.调度决策（传统的做法是串行，业务线维度拆分集群，存在资源分配不均匀问题）计算出可以调度的作业，主动拉取作业，等待合适的资源来执行，水平拓展较好
2.引入资源池（在作业上打上标签，引入优先级概念，预制公共资源、按需分配资源）
3.组件分层设计（a.系统交互层制定统一流程标准 b.支持多种组件交付形式 c.业务逻辑层采用适配器模式）
核心设计：
作业调度、作业状态流转、作业拉取（作业丢失通过补偿机制、作业防重通过乐观锁）、资源池划分（多队列和标签匹配）、组件分层（不同的sdk结合状态机和适配器（动态注入业务拓展command））
拓展能力：通过事件类型进行终止、回调、补偿
规划：
借助云原生 从弹性资源、启动加速、环境隔离 更优拓展
提供开发者平台，打造良性组件运营生态

```
#### [数据库全量SQL分析与审计系统性能优化之旅](https://mp.weixin.qq.com/s/g2VD9SK0xq8R8biG2HyUfw)
```
背景：对MySQL流量进行全量分析
现状：rds-agent 抓取访问数据 同机房通过Log-agent 将日志写到 kafka storm 分析攻击事件 持久化到hive中
问题：高QPS下丢失率、cpu较高
改进：
1.分析架构找出瓶颈2.worker机制及时下线过期链接3.协议优化可以采用thrift4.go的pprof进行cpu画像分析5.缩短流程链路6.降低cpu切换频率（手动触发）7.垃圾回收可以分配池化池、或者mmap直接申请内存 8.对于空包直接过滤，分析mysql数据包 payload_length 为空的过过滤
特点1：ip[2:2] - ((ip[0] & 0x0f) << 2) - ((tcp[12:1] & 0xf0) >> 2) >= 4
特点2： (dst host {localIP} and dst port 3306 and (tcp[(((tcp[12:1] & 0xf0) >> 2) + 3)] <= 0x01))
规划：
改造优化MySQL 内核方式输出全量SQL 和 sql 端到端 追踪补齐
```
#### [基于AI算法的数据库异常监测系统的设计与实现](https://mp.weixin.qq.com/s/EUPREu-SRGJwqTWWeDlvxw)

#### [Replication（上）：常见的复制模型&分布式系统的挑战](https://mp.weixin.qq.com/s/LB5SR4ypQwDxzueI1ai2Kg)

#### [Replication（下）：事务，一致性与共识](https://mp.weixin.qq.com/s/O9Z5e_BzdxKcULHigYMkRg)

#### [美团搜索粗排优化的探索与实践](https://mp.weixin.qq.com/s/u3sw_PatpwkFC0AtkssmPA)

#### [Kafka在美团数据平台的实践](https://mp.weixin.qq.com/s/waVLtusovUkVDt7rKCcdDg)

#### [日志导致线程Block的这些坑，你不得不防](https://mp.weixin.qq.com/s/nowNpIOHBFHD0pctcKr2UA)

#### [可视化全链路日志追踪](https://mp.weixin.qq.com/s/Er4-X8q5MKZZUgAUHyeLwA)

