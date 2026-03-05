---
tags:
  - MySQL
up:
  - "[[编程/归档/八股文/MySQL]]"
down:
relation:
  - "[[页面置换算法]]"
  - "[[内存池]]"
---
- [[#Buffer Pool|Buffer Pool]]
- [[#Buffer Pool的大小与配置|Buffer Pool的大小与配置]]
- [[#Buffer Pool缓存内容|Buffer Pool缓存内容]]
- [[#管理空闲缓存页|管理空闲缓存页]]
- [[#管理脏页|管理脏页]]
- [[#脏页、抖动|脏页、抖动]]
- [[#提高缓存命中率 LRU|提高缓存命中率 LRU]]
- [[#预读策略|预读策略]]
- [[#Buffer Pool污染|Buffer Pool污染]]


## Buffer Pool

- MySQL 的数据存储在磁盘上，但直接从磁盘读取效率低。Buffer Pool 是 InnoDB 在内存中维护的缓存池，使热数据高效读取。
- 基本工作原理：
    - **读取**：先检查数据是否在 Buffer Pool 中，命中则直接从内存读取；未命中则从磁盘加载到 Buffer Pool 再返回。
    - **更新**：先在 Buffer Pool 中修改数据页并标记为脏页，后台线程异步将脏页刷回磁盘（WAL 机制）。
## Buffer Pool的大小与配置

- Buffer Pool 是在 MySQL启动的时候，向操作系统申请的一片连续的内存空间，默认配置下 Buffer Pool只有 128MB
- 可以通过调整 `innodb_buffer_pool_size` 参数来设置 BufferPool 的大小，一般建议设置成可用物理内存的 60%~80%

## Buffer Pool缓存内容

- InnoDB会把存储的数据划分为若干个「页」，以页作为磁盘和内存交互的基本单位，一个页的默认大小为16KB。InnoDB 会为 Buffer Pool申请一片连续的内存空间，然后按照默认的16KB的大小划分出一个个的页，Buffer Pool 中的页就叫做缓存页。此时这些缓存页都是空闲的

> Buffer Pool 除了缓存「索引页」和「数据页」，还包括了undo页，插入缓存、自适应哈希索引、锁信息等等。

[![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/innodb/bufferpool%E5%86%85%E5%AE%B9.drawio.png#id=IjaQa&originHeight=377&originWidth=812&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/innodb/bufferpool%E5%86%85%E5%AE%B9.drawio.png#id=IjaQa&originHeight=377&originWidth=812&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

- 为了更好的管理 Buffer Pool 中的缓存页，InnoDB为每一个缓存页都创建了一个控制块，控制块信息包括「缓存页的表空间、页号、缓存页地址、链表节点」等等。控制块也是占有内存空间的，它是放在Buffer Pool 的最前面，接着才是缓存页
- 控制块和缓存页之间灰色部分称为碎片空间。
    - 每一个控制块都对应一个缓存页，那在分配足够多的控制块和缓存页后，剩余的空间不够一对控制块和缓存页的大小，称为碎片。
    - 如果把 Buffer Pool 的大小设置的刚刚好的话，也不会产生碎片。
- 当查询一条记录时，InnoDB 是会把整个页的数据加载到 Buffer Pool中，因为，通过索引只能定位到磁盘中的页，而不能定位到页中的一条记录。将页加载到Buffer Pool 后，再通过页里的页目录去定位到某条具体的记录。

## 管理空闲缓存页

- 为了快速找到空闲的缓存页，MySQL使用链表结构，将空闲缓存页的「控制块」作为链表的节点，这个链表称为Free 链表（空闲链表）
- 有了 Free 链表后，每当需要从磁盘中加载一个页到 Buffer Pool中时，就从Free链表中取一个空闲的缓存页，并且把该缓存页对应的控制块的信息填上，然后把该缓存页对应的控制块从  
    Free 链表中移除  
    
- Free链表上除了有控制块，还有一个头节点，该头节点包含链表的头节点地址，尾节点地址，以及当前链表中节点的数量等信息。
- Free链表节点是一个一个的控制块，而每个控制块包含着对应缓存页的地址，所以相当于Free 链表节点都对应一个空闲的缓存页。

## 管理脏页

- 脏页是在内存中被修改但尚未同步到磁盘上的页面。MySQL使用另一种链表结构，称为Flush链表，来快速识别哪些缓存页是脏的。它跟Free 链表类似的，链表的节点也是控制块，区别在于 Flush链表的元素都是脏页。有了 Flush 链表后，后台线程就可以遍历 Flush链表，将脏页写入到磁盘

## 脏页、抖动

- 引入了 Buffer Pool 后，当修改数据时，首先是修改 Buffer Pool中数据所在的页，然后将其页设置为脏页，但是磁盘中还是原数据。脏页需要被刷入磁盘，保证缓存和磁盘数据一致，若每次修改数据都刷入磁盘，则性能会很差，因此一般都会在一定时机进行批量刷盘。
- InnoDB 的更新操作采用的是 **Write Ahead Log**策略，即`先写日志，再写入磁盘`，通过 redo log  
    日志让 MySQL 拥有了崩溃恢复能力。  
    

> 下面几种情况会触发脏页的刷新：
> 
> - 当 redo log日志满了的情况下，会主动触发脏页刷新到磁盘；
> - Buffer Pool空间不足时，需要将一部分数据页淘汰掉，如果淘汰的是脏页，需要先将脏页同步到磁盘；
> - MySQL 认为空闲时，后台线程会定期将适量的脏页刷入到磁盘；
> - MySQL正常关闭之前，会把所有的脏页刷入到磁盘；

- 在我们开启了慢 SQL监控后，如果你发现**偶尔会出现一些用时稍长的SQL**，这可能是因为脏页在刷新到磁盘时可能会给数据库带来性能开销，导致数据库操作抖动。如果经常出现这种现象，就需要调大Buffer Pool 空间或 redo log 日志的大小

## 提高缓存命中率 LRU

- 使用LRU（Least Recently Used）策略，确保最近和频繁访问的数据保持在BufferPool内。该算法的思路是，链表头部的节点是最近使用的，而链表末尾的节点是最久没被使用的。那么，当空间不够了，就淘汰最久没被使用的节点，从而腾出空间。
- 简单的 LRU 算法的实现思路是这样的：
    - 当访问的页在 Buffer Pool 里，就直接把该页对应的 LRU链表节点移动到链表的头部。
    - 当访问的页不在 Buffer Pool 里，除了要把页放入到 LRU链表的头部，还要淘汰 LRU 链表末尾的节点。
- Buffer Pool 里有三种页和链表来管理数据。
    - Free Page（空闲页），表示此页未被使用，位于 Free 链表；
    - Clean Page（干净页），表示此页已被使用，但是页面未发生修改，位于LRU链表。
    - Dirty Page（脏页），表示此页「已被使用」且「已经被修改」，其数据和磁盘上的数据已经不一致。当脏页上的数据写入磁盘后，内存数据和磁盘数据一致，那么该页就变成了干净页。脏页同时存在于LRU 链表和 Flush 链表。

---

- 简单的 LRU 算法无法避免下面这两个问题：预读失效；Buffer Pool污染；

## 预读策略

- 预读：程序是有空间局部性的，靠近当前被访问数据的数据，在未来很大概率会被访问到。所以，MySQL在加载数据页时，会提前把它相邻的数据页一并加载进来，目的是为了减少磁盘IO。但是可能这些被提前加载进来的数据页，并没有被访问，相当于这个预读是白做了，这个就是预读失效。如果使用简单的LRU 算法，就会把预读页放到 LRU 链表头部，而当 BufferPool空间不够的时候，还需要把末尾的页淘汰掉。如果这些预读页如果一直不会被访问到，就会出现一个很奇怪的问题，不会被访问的预读页却占用了LRU链表前排的位置，而末尾淘汰的页，可能是频繁访问的页，这样就大大降低了缓存命中率。

> 怎么解决预读失效而导致缓存命中率降低的问题？

- 改进LRU策略：MySQL采取了一个策略，将LRU链表分为old区域和young区域。预读的数据页首先被加入到old区域，只有在实际被访问时才会移到young区域。这样，预读但未使用的页不会影响热点数据。

## Buffer Pool污染

- 当某一个 SQL 语句扫描了大量的数据时，在 Buffer Pool空间比较有限的情况下，可能会将 Buffer Pool里的所有页都替换出去，导致大量热数据被淘汰了，等这些热数据又被再次访问的时候，由于缓存未命中，就会产生大量的磁盘IO，MySQL 性能就会急剧下降，这个过程被称为 Buffer Pool 污染。
- Buffer Pool污染是指由于某些查询导致BufferPool中的热点数据被替换出去的现象。即使只有少量的查询结果返回，但如果这导致大量数据页被读入BufferPool**例如，由于全表扫描、全表条件扫描**，也可能引起这种情况。这可能导致高性能的缓存数据被低性能的数据替换，从而降低数据库性能。

> 怎么解决出现 Buffer Pool 污染而导致缓存命中率下降的问题？

- **延迟移动至young区域**：MySQL为old区域的页设置了一个门槛，只有在某段时间后的连续访问中，这些页才会被移到young区域。这可以通过innodb_old_blocks_time参数进行控制。
- **young区域的优化**：为了避免频繁地将数据页移动到young区域的顶部，MySQL采取了策略，使young区域前面的1/4部分在被访问时不会立即移到链表头部。
