# Elastic

ElasticSearch是基于Luncene开发的分布式全文搜索引擎框, 使用RESTful API进行封装

> luncene是使用java开发的搜索框架

ES只能对ES内部的数据进行搜索, 因此在使用es搜索前, 必须先将数据存入es中

es基于luncene实现数据存储

> mysql和es的数据储存和操作语句对比

| Mysql      | Elastic        | 对比                                      |
| ---------- | -------------- | ----------------------------------------- |
| table      | index(indices) | 索引是文档的集合                          |
| row        | document       | 文档以json的格式保存, 代表一条数据        |
| column     | field          | 字段                                      |
| constraint | mapping        | 映射相当于数据库中的约束                  |
| sql        | dsl            | es使用(json风格)dsl语句对数据进行增删改查 |

mysql和es的优缺点对比

1. mysql: 基于事务操作数据, 确保数据安全和一致性, 但是大数据查询效率不高
2. elastic: 基于原子性操作数据