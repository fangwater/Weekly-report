# McDipper: A key-value cache for Flash storage
## 概述
我们为什么要使用SSD cache？
[1]Raw的价格
[2]互联网服务中 对于capacity的要求很高 但请求相对数据量而言是相当有限的 根据叙述 和内存相比 flash存储有超过20倍的capacity，并且workload也很大
[3]更多的时候 延迟本身不在于SSD的io瓶颈 而是网络延迟等
因此SSD 的 flash cache在特定的业务环境下有其作用
### 目的
为了更好的利用Flash存储\
[1]所有数据有保护 CheckSum之类的验证方式
[2]Cache策略可替换
可靠性的检测依赖于Cache一致性 写入等也是一样的 需要
## Summary
SSD cache一方面可以实现更大的低成本cache而不是DRAM，并且实际上在分布式的应用场景下不会对于性能和RAW cache产生很大的区别 需要结局的不同数据的storage的问题
## Q
### 考虑6.824的lab
[1]真实系统下我们需要考虑存储对象的不规则性质 在6.824的lab中数据只有标准的message 所以无论是cache的查找策略 后期的对象存储(sharding)等都比较容易\
[2]6.824用了一个简单的发布租约协议来维持cache一致性，这里采用的协议需要阅读源代码，这个可靠性的保证有多高？容错协议使用的什么样的策略？
#### SSD的并行性
单机多个SSD cache的部署可以利用Flash的特性

# Optimizing Flash-based Key-value Cache Systems

### Reduntant Map
考虑在SSD FLASH Cache的架构下
[1]application level\
In-memory hash table 中 key 映射到 Slab中 然后offset\
[2]FTL levele\
LBA映射到PBA\
两层映射是不必要的？为什么？
DRAW的开销大确实是问题
### Double Garbage Collection
和上一个问题上相似的，重点在于冗余问题？
### Optimizing
优化FTL和文件系统的中间层 并且对SSD cache的并行化提供支持
（Twitter给出的思路）
[1]直接吧KEY映射到Flash的channel中
[2]用轮转算法解决颗粒损耗平衡的问题
[3]把GC做在第一层
[4]Channel的空间管理也做在第一层
好处是显然的\
[1]DRAW节约\
[2]时间加速\
[3]More

## DIDACache: A Deep Integration of Device and Application for Flash based Key-value Caching
### 概述
[1]因为RAW的价格 和 Web系统的实际情况 实现了弱一致性的SSD cache结构\
[2]因为多级映射带来raw和空间上的浪费 还有GC的麻烦 实现直接从KV到Flash channel的映射\
[3]从应用层开始的深度相连的Cache\
