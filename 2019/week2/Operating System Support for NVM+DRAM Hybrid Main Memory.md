# Operating System Support for NVM+DRAM Hybrid Main Memory
# 提出了NVM+DRAM中间层的思路
[1]
在一些针对nane flash 和 NOR FLAS的读写中.需要dram buffer来作为时序一致性、
的接口。这个缓存对于os是可见的一层，需要才操作系统实现的时候就进行考虑(page)\
[2]
什么样的buffer page需要被迁移到nvm中的算法应该在os层实现比较适合\
[3]
NOR flash尤其需要os层提供这样的page处理机制\

#Intro
[1]DRAM受限于价格和性能，无法放在“接近cpu”的位置上，这个问题放在服务器的环境下更加严重，因为
物理原则的制约
[2]对比FLASH拥有更好的节能效果，不必受热量的过多限制
[3]
