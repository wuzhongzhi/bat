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
