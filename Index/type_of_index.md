# 索引类型

**索引在mysql之中也被称为“键(key)”是存储引擎用于快速找到记录的一种数据结构。**

索引有很多类型，可以为不同的场景提供更好的性能。在mysql中，索引是在存储引擎层而不是服务器层实现的。所以不同的存储引擎的索引的工作方式是不同的，也不是所有的存储引擎都支持所有类型的索引。即使多个存储引擎支持同一种类型的索引，其底层的实现也是不同的。

### **mysql支持的索引：**

#### ***1.B-TREE索引（ARCHIVE引擎不支持）***

​        这里引用维基百科的定义：**B树**（英语：B-tree）是一种自平衡的[树](https://link.jianshu.com?t=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E6%25A0%2591_%28%25E6%2595%25B0%25E6%258D%25AE%25E7%25BB%2593%25E6%259E%2584%29)，能够保持数据有序。这种数据结构能够让查找数据、顺序访问、插入数据及删除的动作，都在[对数时间](https://link.jianshu.com?t=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E6%2597%25B6%25E9%2597%25B4%25E5%25A4%258D%25E6%259D%2582%25E5%25BA%25A6%23%25E5%25AF%25B9%25E6%2595%25B0%25E6%2597%25B6%25E9%2597%25B4)内完成。B树，概括来说是一个一般化的[二叉查找树](https://link.jianshu.com?t=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E4%25BA%258C%25E5%2585%2583%25E6%2590%259C%25E5%25B0%258B%25E6%25A8%25B9)（binary search tree），可以拥有多于2个子节点。与[自平衡二叉查找    树](https://link.jianshu.com?t=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E8%2587%25AA%25E5%25B9%25B3%25E8%25A1%25A1%25E4%25BA%258C%25E5%258F%2589%25E6%259F%25A5%25E6%2589%25BE%25E6%25A0%2591)不同，B树为系统大块数据的读写操作做了优化。B树可以减少定位记录时所经历的中间过程，从而加快存取速度。

​         大多数时候人们在提到索引但如果特别指明索引的类型的时候，人们指的是**B-TREE**索引。

​        ***B-TREE***通常意味着所有的值都是按顺序存储的，并且每一个叶子页到根的距离都相同

​        **B-TREE**索引能够加快访问数据的速度，是因为存储引擎不再需要进行全表扫描来获取需要的数据，取而代之的是从索引的根节点开始搜索。

​        **B-TREE**对索引列是顺序组织存储的，所以很适合查找范围数据

####  **2.哈希索引（hash-index只有Memory引擎支持）** 

​        哈希索引是基于哈希表实现，只有精确匹配索引所有列的查询才有效。对于每一行数据，存储引擎都会对所有的索引列计算一个哈希码（**hash code**） 哈希索引将所有的哈希码存储在索引中，同时在哈希表中保存指向每个数据行的指针。

#### 哈希索引的缺点

哈希表是无顺的数据结构，对于很多类型的查询语句哈希索引都无能为力。举例来说，假如你想要找出所有小于40岁的员工。你怎么使用使用哈希索引进行查询？这不可行，因为哈希表只适合查询键值对-也就是说查询相等的查询（例：like “WHERE name = ‘Jesus’）。哈希表的键值映射也暗示其键的存储是无序的。这就是为什么哈希索引通常不是数据库索引的默认数据结构-**因为在作为索引的数据结构时，其不像B-Tree那么灵活**

#### **3.空间数据索引(spatitial index  R-tree MyISAM引擎和Innodb引擎支持)**

​        和B-Tree索引不同，这类索引无须前缀查询。空间索引会从所有维度来索引数据。查询时，可以有效地使用任意维度来组合查询。必须使用**MySQL**的**GIS**相关函数如**MBRCONTAINS()**等来维护数据。但mysql对GIS的支持目前还不完善，不建议使用。

#### **4.全文索引**

   全文索引是一种特殊类型的索引，它查找的是文本中的关键词，而不是直接比较索引中的值。全文索引通常用刀拍索引来实现。全文搜索和其他几类索引的匹配方式完全不一样。它有许多需要注意的细节：如停用词，词干，和复数，布尔搜索等。全文索引更类似于搜索引擎做的事情，而不是简单的where条件匹配。全文索引适用于MATCH AGAINST操作。

参考：

https://www.jianshu.com/p/d1cb20c4a2c0