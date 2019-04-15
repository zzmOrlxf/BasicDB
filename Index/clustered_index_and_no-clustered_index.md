# 聚集索引和非聚集索引



## 聚集索引  Clustered

聚集（clustered）索引，也叫聚簇索引。

数据物理存储顺序与索引列的顺序相同，所以一个表只能拥有一个聚集索引。聚簇索引的叶节点包含数据页。There are clustered and nonclustered indexes. A clustered index is a special type of index that reorders the way records in the table are physically stored. Therefore table can have only one clustered index. The leaf nodes of a clustered index contain the data pages.

## 非聚集索引

数据物理存储顺序与索引顺序无关，非聚集索引的叶节点不包含数据页。相反，叶节点包含索引行。

A nonclustered index is a special type of index in which the logical order of the index does not match the physical stored order of the rows on disk. The leaf node of a nonclustered index does not consist of the data pages. Instead, the leaf nodes contain index rows.

**非聚集索引和聚集索引的区别** 在于， 通过聚集索引可以查到需要查找的数据， 而通过非聚集索引可以查到记录对应的主键值 ， 再使用主键的值通过聚集索引查找到需要的数据 

**不管以任何方式查询表， 最终都会利用主键通过聚集索引来定位到数据， 聚集索引（主键）是通往真实数据所在的唯一路径。**

```mermaid
graph TD
subgraph 非聚集索引
A[index] -->B[index]
A-->C[index]
    B -->|获取索引| D[index] 
    B --> E[index]
    C --> F[index]
    C --> G[index]
end

subgraph 聚集索引
A1[index] -->B1[index]
A1-->C1[index]
    B1 -->|获取数据| D1[Result 1]
    B1 --> E1[Result 2]
    C1 --> F1[Result 3]
    C1 --> G1[Result 4]
end
   
    
    
```

## 主键和聚集索引的关系

聚集索引并不一定是主键，也不一定是唯一索引。

通常，我们会在每个表中都建立一个自增的ID列区分每条数据。此时，如果我们将这个列设为主键，SQL SERVER会将此列默认为聚集索引。这样数据在数据库中会按照ID进行物理排序。  

## 有效利用聚集索引          

聚集索引的优势是很明显的，而每个表中只能有一个聚集索引的规则，这使得聚集索引变得更加珍贵。      

聚集索引的定义，使用聚集索引的最大好处就是能够根据查询要求，迅速缩小查询范围，避免全表扫描。在实际应用中，因为ID号是自动生成的，我们并不知道每条记录的ID号，所以我们很难在实践中用ID号来进行查询。这就使让ID号这个主键作为聚集索引成为一种资源浪费。其次，让每个ID号都不同的字段作为聚集索引也不符合"大数目的不同值情况下不应建立聚合索引"规则；当然，这种情况只是针对用户经常修改记录内容，特别是索引项的时候会负作用，但对于查询速度并没有影响。      

假设，业务上每次每个用户打开首页查询历史订单的时候都进行一次全表扫描，这样做意义是不大的，绝大多数的用户1个月前的文件都已经浏览过了，这样做只能徒增数据库的开销而已。事实上，我们完全可以让用户打开系统首页时，数据库仅仅查询这个用户近3个月来未阅览的文件，通过"日期"这个字段来限制表扫描，提高查询速度。如果系统已经建立的2年，那么您的首页显示速度理论上将是原来速度8倍，甚至更快。"理论上"三字，是因为如果您的聚集索引还是盲目地建在ID这个主键上时，您的查询速度是没有这么高的，即使您在"日期"这个字段上建立的索引（非聚合索引）。

看一下在1000万条数据量的情况下各种查询的速度表现（3个月内的数据为25万条）：      

（1）仅在主键上建立聚集索引，并且不划分时间段：

Select gid,fariqi,neibuyonghu,title from tgongwen  用时：128470毫秒（即：128秒）      

（2）在主键上建立聚集索引，在fariq上建立非聚集索引：     

select gid,fariqi,neibuyonghu,title from Tgongwen where  fariqi> dateadd(day,-90,getdate())   用时：53763毫秒（54秒）      

（3）将聚合索引建立在日期列（fariqi）上：     

select gid,fariqi,neibuyonghu,title from Tgongwen where  fariqi> dateadd(day,-90,getdate()) 用时：2423毫秒（2秒）      

虽然每条语句提取出来的都是25万条数据，各种情况的差异却是巨大的，特别是将聚集索引建立在日期列时的差异。事实上，如果您的数据库真的有1000万容量的话，把主键建立在ID列上，就像以上的第1、2种情况，在网页上的表现就是超时，根本就无法显示。这也是我摒弃ID列作为聚集索引的一个最重要的因素。     

得出以上速度的方法是：在各个select语句前加：declare @d datetime set @d=getdate()      

并在select语句后加：     

select [语句执行花费时间(毫秒)]=datediff(ms,@d,getdate())   

## 聚集索引带来的问题

（1）聚集索引适合用于需要进行范围查找的列，因为聚集索引的叶子节点存放的是有序的数据行，查询引擎可根据WHERE中给出的范围，直接定位到两端的叶子节点，将这部分节点页的数据根据链表顺序取出即可；

（2）聚集索引尽量建立在值不会发生变更的列上，否则会带来非聚集索引的维护；

（3）尽量在建立非聚集索引之前建立聚集索引，否则会导致表上所有非聚集索引的重建；

（4）聚集索引应该避免建立在数值单调的列上，否则可能会造成IO的竞争，以及B树的不平衡，从而导致数据库系统频繁的维护B树的平衡性。聚集索引的列值最好能够在表中均匀分布。

## 参考：

https://blog.csdn.net/HkEndless/article/details/39177617

https://blog.csdn.net/ak913/article/details/8026743 文中对聚集和非聚集进行了分析

