# 图数据库

图数据库用于解决的问题：解决关系数据库连接效率低下的问题



## 图数据库基本知识

无模式的数据库

#### 基本概念

图数据库存储两种数据：结点(node)和边(edge)，结点和边都可拥有可搜索的属性



#### 分类

**图数据库**：在线的图数据库管理系统，支持对图数据模型的增、删、查、改，一般应用于事务（OLTP）系统。

特性：

+ 底层存储：原生，为存储和管理图而专门设计的（Neo4j）；非原生，将图序列化然后存储到其他数据库中（Titan）。

+ 处理引擎：一些图数据库使用“免索引邻接”，可提升性能

应用场景：主要用于联机事务图的持久化技术，通常直接实时地被应用程序访问，和常见的联机事务处理（online transactional processing, OLTP）数据库是一样的。



**图计算引擎**：偏向于全局查询，通常对大规模批处理数据做优化。只有一部分图计算引擎有自己的存储层，其他都只关注如何对接外部传入数据，然后返回其他地方保存。

应用场景：主要用于离线图分析技术，通常按照一些列步骤执行，它们可以和其他大数据分析技术看作一类，比如数据挖掘和联机事务分析（online analytical processing，OLAP）。



参考：

[图数据库基础](https://zhuanlan.zhihu.com/p/28155651)

[图数据库介绍](https://zhuanlan.zhihu.com/p/32857155)

[互联网金融，如何用知识图谱识别欺诈行为](http://www.dataguru.cn/article-8737-1.html)



## 性能对比

#### 评估标准

+ 装载能力（数据加载能力）

+ 是否支持实时更新

+ 磁盘存储方式（原生图格式or非原生图格式，非原生图以关系表或RDF等方式存储）

+ 查询语言表达能力

+ 是否支持计算/存储的横向/纵向扩展（增加机器存储，多核运行）

+ 支持OLTP（联机事务处理），OLAP，HTAP（混合事务分析处理）

+ 是否支持多图（TigerGraph）

+ 是否支持实时深层链接分析查询

+ 是否支持内置图可视化



#### 一些图数据库对比

**图数据库：**

==neo4j==：开源，单机，内存上限，使用最广，用户最多，商业化最好，cypher查询语言，使用直观

titan：在服务TP类请求时，进行单机计算，在服务AP类的请求时，进行分布式计算（GraphX/Graph），已经停止更新

JanusGraph：titan的分支，分布式

GraphSQL：定制存储、存储计算一体化

Cayley：开源，go语言编写，可用JS或MQL语言查询，需要基于其他数据库存储，[中文官网](https://cayley.org/)

==TigerGraph==: 闭源（免费的只有社区版），性能优于neo4j，免费版只能单机，不能分布式，提供API，OS为linux

FlockDB：开源，分布式，twitter使用，[参考](http://hao.jobbole.com/flockdb/)

==ArangoDB==：开源，可以存三种类型数据：document / graph db / key-value，和其他数据库[对比](https://www.v2ex.com/t/113232)，官网[对比](https://www.arangodb.com/2018/02/nosql-performance-benchmark-2018-mongodb-postgresql-orientdb-neo4j-arangodb/)

HugeGraph：百度，开源，分布式存储，主要解决安全事业部所面对的反欺诈、威胁情报、黑产打击

InfiniteGraph：部分开源，只支持存储100W结点，Java实现

HyperGraphDB：开源，Java实现，用于存储超图



**面向知识图谱的：**

==AllegroGraph==：半开源，可支持数亿结点，性能高，免费版支持5000W个三元组，支持以RDF存储

Virtuoso：没有太多资料

Blazegraph：半开源，java编写，为开源数据库提供GPU加速

Stardog：企业级

GraphDB：性能一般



**图计算引擎：**

MapReduce、PEGASUS、Pregel、Giraph、GraphLab、GraphChi、GraphX

Graph Engine：////



参考：

[图数据库行业概览](https://www.tigergraph.com.cn/2018/08/31/%e4%bf%a1%e6%81%af%e5%9b%be%ef%bc%9a%e5%9b%be%e6%95%b0%e6%8d%ae%e5%ba%93%e8%a1%8c%e4%b8%9a%e6%a6%82%e8%a7%88/)

[数据库计算引擎实时排名](https://db-engines.com/en/ranking/graph+dbms)

[图数据库对比基准报告](https://info.tigergraph.com/benchmark)

[区分图数据库的标准(需要注意的性能)](https://zhuanlan.zhihu.com/p/44318332)





## OLTP，OLAP

#### OLTP

概念：联机事务处理，表示事务性非常高的系统，要求在线，评估标准为每秒其执行的事务数或SQL数量。例如：电子商务系统

缺陷：瓶颈受限于读取总量（读取次数过多）与计算性函数

#### OLAP

概念：联机分析处理，语句的执行量（一条语句执行时间可能会非常久，读取数据也很多）不是考核标准，考量这种系统的性能用磁盘子系统的吞吐量（带宽），取决于磁盘的个数

技术：分区技术，并行技术

|          | OLTP                         | OLAP                         |
| -------- | ---------------------------- | ---------------------------- |
| 用户     | 操作人员，用户并发数多       | 决策人员，用户并发数少       |
| 功能     | 日常操作处理                 | 分析决策                     |
| 数据     | 当前、最新、细节、二维、分立 | 历史、聚集、多维、集成、统一 |
| 时间要求 | 具有实时性                   | 对时间要求不严格             |
| 主要应用 | 数据库                       | 数据仓库                     |

参考：

[OLAP、OLTP的介绍和比较](https://blog.csdn.net/zhangzheng0413/article/details/8271322/)

[一张图看清楚OLTP和OLAP的区别](https://blog.csdn.net/qq_33414271/article/details/81149966)
