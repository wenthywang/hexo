---
title: elasticsearch 笔记-插件
date: 2016-12-30 15:07:41
tags: 
- elasticsearch-插件
- 分布式
categories: 
- elasticsearch
- 分布式
---
 ## delete-by-query插件使用
```
TransportClient.builder().settings(settings).addPlugin(DeleteByQueryPlugin.class).build();  

```

deleteByQuery插件是通过按照查询出来的doc 来进行删除操作。
- es的集群都需要安装deleteByQuery 插件，到es的安装目录里执行plugin install deletebyQuery命令则可安装
- es安装完需要重启，重新加载插件
- 客户端创建时需要添加plugin的支持如上面代码
- eg：  

```
/**
 * es 查询删除操作
 * @param index 索引
 * @param type 类型
 * @param column 删除字段的列名
 * @param deleteValue 删除字段值
 * @return long 删除结果
 */
    public  long deleteByQuery (String index,String type,String column,String deleteValue) {
        String deleteByQuery="{\"query\":{\"bool\":{\"must\":[{\"term\":{\""+column+"\":\""+deleteValue+"\"}}]}}}";
        LOG.info("[EsJdbcDaoSupport.deleteByQuery] request json-> "+deleteByQuery);
        DeleteByQueryResponse response =  new DeleteByQueryRequestBuilder(EsConnectionFactory.transportClient,   
                                          DeleteByQueryAction.INSTANCE)
                                          .setIndices(index)
                                          .setTypes(type)
                                          .setSource(deleteByQuery)
                                          .execute()
                                          .actionGet();  
        return response.getTotalFound();
    }
```
这种方式是通过拼装请求json 来获得查询条件，然后es发送json查询。  

```
    /**
     * 查询es记录
     * 
     * @param index 数据库
     * @param type 表
     * @param dataMap 字段对应的数据
     * @return  boolean
     */
    public  long delete (String index,String type,String deleteId) {
        TransportClient client =EsConnectionFactory.createEsClient();
        DeleteByQueryResponse response =  new DeleteByQueryRequestBuilder(client,   
                                          DeleteByQueryAction.INSTANCE)
                                          .setIndices(index)
                                          .setTypes(type)
                                         .setQuery(QueryBuilders.boolQuery().must(QueryBuilders.termQuery("QS_ENTERPRISE_ID", deleteId)))
                                          .execute()
                                          .actionGet();  
        return response.getTotalFound();
    }
```
这种方式是通过原生es的查询查询数据，两种效果一样，不过我还是比较喜欢第二种。第一种可以动态生成查询条件，第二种动态生成好像是不行。

## update-by-query 插件使用  

```
TransportClient.builder().settings(settings).addPlugin(ReindexPlugin.class).build();
```

- update-by-query插件是通过按照查询出来的doc 来进行更新操作，主要原理应该是：es不支持直接操作es单条记录，所以要把更新的记录拿出来，然后删除原来的，把更新后插入进去。
- es 不需要安装，不需要重启
- 工程中添加reindex.jar，jar包在下载es里面的lib中可以找到
- 客户端创建时需要添加plugin的支持如上面代码
- eg：  

```
        UpdateByQueryRequestBuilder ubqrb=UpdateByQueryAction.INSTANCE.newRequestBuilder(client);
        BulkIndexByScrollResponse r=ubqrb.source(INDEX_ST_SESSION_TAG_SDR)
                              .script(new Script("[ctx._source.ST_TAG_ID2=0,ctx._source.ST_TAG_ID3=0]"))
                              .filter(QueryBuilders.boolQuery().must(QueryBuilders.termQuery("ST_ENTERPRISE_ID", orgName)))
                             .filter(QueryBuilders.boolQuery().must(QueryBuilders.termQuery("ST_TAG_ID2", tagid)))
                             .get();
```

update-by-query 的难点在于一开始的script的写法，一开始不懂如何写script，后来看了各种文档api，才发现ctx.source是最重要的一个参数。后面的才是es的对应的列名（mapping），个人感觉ctx代表了所有数据的所有属性。

## 总结
通过学习两种插件的使用，更加了解了es的运作，现在用的是2.4.1的版本。没有升级，好像5的版本已经自带插件了，不需要再手动添加插件了。不过，还是觉得es配合sql的操作还是不让人放心。复杂的业务操作还是用原生的es，不需要用sql插件，不然会出现一些不可预知的情况。