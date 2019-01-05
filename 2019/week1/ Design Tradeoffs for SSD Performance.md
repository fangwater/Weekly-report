# Design Tradeoffs for SSD Performance

## 概述
基于SSD的视角重新讨论了complex system的负载和性能的模拟和分析。经过实验证明， SSD based 的系统的性能对于workload是非常敏感的，在分布式系统，存储系统等都有显著的表现

###  关系到SSD系统性能的主要因素
[1]Data placement 跨片写 FTL并行读的问题和GC时候的颗粒损耗 \
[2]Parallelism 也是FTL的问题 IO上的优化\
[3]Write ordering NAND FLASH 也是SSD构造上先天决定的问题\
[4]workload management  SSD的性能和workload的关系很大，比如对于sequence和非sequence的Transcation，SSD不同的设计可能造成不同的性能特点

###  SSD的基本设计介绍
[1]SLC和MLC的问题 基本上可以看到虽然SLC的寿命好很多MLC基本已经是趋势了\
[2]具体的参数\
[3]Bandwidth和Interleaving\
[4]FTL的一些逻辑结构和机器连接的问题 实际上现在有一些OS已经对这个结构有特定的优化了

### Logical Block Map
[1]LBS存放在volatile memory中,存储虚拟页的和物理页的映射关系\
[2]LBS有动态和静态的不同存储结构设计\
[3]Page size和Page span\
LBS的存在直接影响了Raid(并行),load balance(workload)控制，擦写等多个方面的问题

### Clean
[1]FTL对于flash碎片的收集需要考虑颗粒的寿命和负载 GC过程中逻辑片和物理片之间的冲突关系\
[2]清理过程的效率和颗粒寿命的平衡\
[3]维持一个pool来循环使用 尽可能的使得颗粒的损耗尽可能的均匀，选择采用贪心算法\
[4]LBS可以控制Clean有更细的颗粒度\
[5]FTL设计和Log File 写入方式控制\

### Parallelism and Interconnect Density
[1]Parallel requests 自然并行化的无关数据请求\
[2]Ganging 拼贴实现的并行化\
[3]Interleaving 减少IO cost\
[4]Background cleaning 减少前段总线的IO cost
### Persistence
[1]逻辑映射的损失问题影响了SSD的持久化存储\
[2]SSD可以不提供纠错\
[3]映射可以保存在RAW中
### Industry Trends
[1]Consumer portable storage\
[2]Laptop disk replacements\
[3]Enterprise/database accederasteors\
其核心性能主要取决与SSD中的高性能flash cache的大小和设计
### Simulator
我们已经从产品的宣传中得知 不同种类的SSD设计使得SSD在适合自己的workload下会有比较理想的表现 \
但细节上并不是十分清晰，比如一种SSD适合sequence的读写，这个sequence是针对单个stream还是多个也可以？如果是针对多个，最优的是多少个stream？ 如果是针对随机读写优化的，这个随机的分布假设是怎样的？\
在CMU 的 DISK-Sim的基础上进行了修改:\
[1]添加了针对SSD的并行处理的部分\
[2]用数据结构模拟了LBS，和包含擦写的颗粒单元\
### Workloads
[1]TPC-C\
[2]Exchange\
[3]IOzone\
[4]Postmar\
几种文件系统测评的工具
### Results
[1]Microbenchmarks\
[2]Page Size,Striping ,and Bandwidth和Interleaving\
[3]Gang Performance\
[4]Copy -back vs Inter-plane Transfe\
[5]Cleaning Thresholds\
本质上这些东西主要取决与FTL的具体算法和FS 略
### 相关
#### Solid-State Storage Devices
[1]探讨了资源受限情况下的SSD存储系统设计 一般是限定了特殊的性能需求 和高性能系统更多的关注IO读写有所不同\
[2]需求的不同不仅仅体现在整个SSD系统 也包括FTL的回收和擦写策略算法\
[3]FTL Cache
#### File System Designs
[1]针对Flash base的系统 特定化提出针对擦写延迟层次的文件系统优化JSFS JSFS2\
[2]嵌入式环境的人Flash base系统 减少特定位置的擦写磨损 提供简单的容错和GC YAFFS\
[3]本文的方法适合于存储栈
#### Algorithms and Data-structures
[1]适合Flash的算法主要在于对颗粒寿命的控制\
[2]一方面自上而下的考虑写入的加权 另一方面自下而上的考虑回收\
[3]利用Flash存储的固有并行性质分段(Interesting)
## Summary
[1]SSD系统的性能评估方法和一个基本模型
[2]影响性能的原因讨论
[3]优化wear leveling的算法
## QA
### 论文如何设计SSD性能评测？
[1]选择了特定的NAND-Flash Chip\
[2]通过对running system 的 trace（TPC-C benchmark）\
[3]改变server的workload\
[4]使用不同标准的file system benchmark
### TPC-C 和 TPC-H benchmark？
Transcation的设计和处理有所不同 都面向数据库系统 基于事务
### SSD Wear leveling ?
Also written as wear levelling) is a technique for prolonging the service life of some kinds of erasable computer storage media, such as flash memory, which is used in solid-state drives (SSDs) and USB flash drives, and phase change memory
