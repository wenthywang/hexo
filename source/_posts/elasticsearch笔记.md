---
title: elasticsearch 笔记
date: 2016-11-18 01:07:41
tags: 
- elasticsearch
- 分布式
categories: 
- elasticsearch
- 分布式
---
 ## 初学elasticsearch
elasticsearch，简称ES。**[百度百科](http://baike.baidu.com/link?url=dbviqnoOFYZYYSxYswZubLVUk-7WFiuR9tLb-DOC0sv0pHRHEkfsSw0--EdarpYBLdaqrAPw9YPHYl0mQA6TxoIMUIj7i3NDVMtGCKsjxYG)**
> ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。
我们建立一个网站或应用程序，并要添加搜索功能，令我们受打击的是：搜索工作是很难的。我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一> 个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。


### elasticsearch 安装
- 安装elasticsearch简单，容易。我使用的es是2.4.1版本的，[elasticsearch下载](https://www.elastic.co/downloads/elasticsearch)。另外Java也是必须的，最好是1.7以上的版本，我用的是1.7的java版本的。
- 下载好之后，打开根目录下的bin目录下的elasticsearch.bat，如下：
![打开bat](/img/es1.png)![打开bat](/img/es2.png)  
其实就是本地开了服务，跟web工程差不多。

- 打开之后，就可以本地玩耍了。es默认使用9200，9300端口的，详细配置可以参考官方文档，也可以自己参详参详。在浏览器上输入``` localhost：9200 ``` 可见配置信息，以及开启信息，如下图：
![打开bat](/img/es3.png)

### elasticsearch插件安装
- es有两个插件，一个是head的插件，可以看到es的集群状态。一个是sql插件，可以用类似sql的语句查询数据。具体的安装可以百度或者google查找哦。

### 强大的Elasticsearch-sql
- Elasticsearch-sql是一个强大的插件，界面上可以用sql查询，代码上可以用sql执行sql语句，相对于es的原生态，我还是比较喜欢这种sql开发的，因为一开始我是使用原生态的查询聚合查询，写得也DT的。后来加了sql插件，开发效率都提高了不少，但是使用起来也是有一定的限制。
- group by 后面的字段不能为空
- 字段类型需要groupby的需要设置not_analyzed 保证这个字段不分析，当出现一些特殊字符的时候，不加的话es会默认使用分析，分词功能，就达不到要的效果，当时也是纠结了好久才发现的。
- 创建es链接麻烦，每次查询需要创建链接，没有连接池，虽然后面用了druid连接，似乎会出问题。
- es put连接创建一次就好了，后来就使用了类加载，一次连接即可，减少多次连接造成的消耗。

### 总结
    使用还是很方便的，特别是用sql。不过对于业务复杂的似乎不怎么样，另外同时使用redis做缓存是最好不过了。
    查询效率是很高，后期维护也是需要挺高的成本的。总体来说实时性还是挺强的，苦逼的我现在还在加班...
    之前写了一半，今天就写完它吧，也是写得很急，有什么要交流的可以留言。或者加微信详聊哦。