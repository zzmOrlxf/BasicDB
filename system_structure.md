# 体系结构

mysql的体系结构

![img](https://img-blog.csdn.net/20150730152113099?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)



连接池组建

```
进行身份验证、线程重用，连接限制，检查内存，数据缓存；管理用户的连接，线程处理等需要缓存的需求。
```

管理服务和工具组件

```
备份和恢复的安全性，复制，集群，管理，配置，迁移和元数据。
```

SQL接口组件

```
进行 DML、DDL，存储过程、视图、触发器等操作和管理；用户通过 SQL 命令来查询所需结果。
数据操纵语言（Data Manipulation Language, DML）是SQL语言中，负责对数据库对象运行数据访问工作的指令集，INSERT、UPDATE、DELETE;
数据库模式定义语言DDL(Data Definition Language)，是用于描述数据库中要存储的现实世界实体的语言。CREATE...
```

查询分析器组件

```
查询翻译对象的特权；SQL 命令传递到解析器的时候会被解析器验证和解析。
```

优化器组件

```
在 MySQL 优化语句过程中，可以通过设置 optimize_switch 控制优化行为。在生产环境上，某时间段 MySQL 服务器压力特别大，load 一度达到了 100，查询发现数据库中有大量的 sql 语句 state 状态 result sorting ，result sorting 这种排序特别消耗 cpu 和内存资源。抽取其中的一条 sql 查看执行计划。
```

缓冲组件

```
全局和引擎特定的缓存和缓冲区;
```

插件式存储引擎

```
MySQL 的 Windows 版本默认存储引擎为 InnoDB，InnoDB 支持事务，并且提供行级的锁定。
```

物理文件

```

```





[堆组织表、索引组织表、索引聚簇表](https://blog.csdn.net/fuwencaho/article/details/32106971)

[堆组织表，索引组织表和索引聚簇表](https://blog.51cto.com/chenxy/752298)



[【MySQL】漫谈MySQL体系结构](https://blog.csdn.net/da_guo_li/article/details/80280289) 文中降到了mysql的查询过程  这个人写的相关文章也可以关注下。

[mysql体系架构](https://blog.csdn.net/rwangnqian/article/details/80021215)

