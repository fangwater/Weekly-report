## 很有意思的reddit文章
https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=auto&sp=nmt4&tl=zh-CN&u=http://codecapsule.com/2014/02/12/coding-for-ssds-part-6-a-summary-what-every-programmer-should-know-about-solid-state-drives/&xid=17259,15700002,15700021,15700124,15700186,15700190,15700201,15700237,15700242,15700248&usg=ALkJrhgEtRlT-ZFF8Xz-K42UJUB5pCnRaw

##F2FS 是什么?
[1]Linux3.8 kernel已合并\
[2]Flash 友好\
[3]相对ssd提升显著\
[4]BTRFS等文件系统并未很好的考虑闪存的性能\


## F2FS好在哪?
[1]A flash-friendly on-disk layout that aligns with the underlying FTL’s operational units to avoid unnecessary data copying.
[2]A cost-effective index structure that addresses the ‘wandering tree’ problem

## F2FS的核心设计思路?
LFS的基础上引入了新的对ssd友好的处理\
In the traditional LFS design, if a leaf data node is updated, its direct and indirect pointer blocks are updated recursively. F2FS, however, only updates one direct node block and its NAT (Node Address Table) entery, effectively addressing the wandering tree problem. 

## 具体一些?
[1]Multi-head logging – which separates hot, warm, and cold data
[2]Adaptive logging – F2FS fundamentally builds on append-only logging to turn random writes into sequential ones. But at high storage utilization it can also change to a threaded logging strategy to avoid long write latencies.
[3]Roll-forward recovery for fsync acceleration


