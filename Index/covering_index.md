# 覆盖索引

**一、覆盖索引**
覆盖索引（covering index）指一个查询语句的执行只用从索引中就能够取得，不必从数据表中读取。也可以称之为实现了索引覆盖。
如果一个索引包含了（或覆盖了）满足查询语句中字段与条件的数据就叫做覆盖索引。
**二、覆盖索引实例**
1.未建立索引前

```
select SQL_NO_CACHE count(name) from index_test where name = '小明' and id_card = '142701199999999999' and type = '2' and hobby = '篮球' and timeline = '1505270575';
```


![img](https:////upload-images.jianshu.io/upload_images/7819864-ae8bc95fc1d702e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/716/format/webp)

添加覆盖索引前

```
explain select SQL_NO_CACHE count(name) from index_test where name = '小明' and id_card = '142701199999999999' and type = '2' and hobby = '篮球' and timeline = '1505270575';
```

2.添加覆盖索引

```
alter table index_test add index index_all(name,id_card,type,hobby,timeline);
```



![img](https:////upload-images.jianshu.io/upload_images/7819864-1375996f27a114ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/716/format/webp)

添加覆盖索引后



![img](https:////upload-images.jianshu.io/upload_images/7819864-7c0ac40d3e4a779e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/742/format/webp)

explain图

**三、总结**
 当一条查询语句符合覆盖索引条件时，sql只需要通过索引就可以返回查询所需要的数据，这样避免了查到索引后再返回表操作，减少I/O提高效率。
 使用覆盖索引Innodb比MyISAM效果更好----InnoDB使用聚集索引组织数据，如果二级索引中包含查询所需的数据，就不再需要在聚集索引中查找了

注：遇到以下情况，执行计划不会选择覆盖查询
 1.select选择的字段中含有不在索引中的字段 ，即索引没有覆盖全部的列。
 2.where条件中不能含有对索引进行like的操作。

作者：wuxuan94

链接：https://www.jianshu.com/p/77eaad62f974

来源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

https://www.jianshu.com/p/f6e4091be10e



[mysql高效索引之覆盖索引](https://www.cnblogs.com/chenpingzhao/p/4776981.html)