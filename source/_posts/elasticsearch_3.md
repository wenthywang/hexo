---
title: elasticsearch-sql-5.1.2笔记
date: 2017-03-27 16:43:41
tags: 
- elasticsearch-sql
- group_by
categories: 
- elasticsearch
- 分布式
---
 ## elasticsearch-sql-5.1.2
**[elasticsearch-sql](https://github.com/NLPchina/elasticsearch-sql)**
> ElasticSearch-sql 是es的插件之一，通过类sql查询es。最近更新了最新的5.1.2版本，出现某些异常情况。使用groupby的时候查询的时候只有10条，出现这个问题，尝试了在issue找答案，找到了发送请求的时候要设置size的大小，而sql插件源码包里面写死了200和10，至于怎么找出来的，我也是在别人的博客里面看到的，**[blog](http://blog.csdn.net/wangyang_software/article/details/51791573)**。因此，通过修改源码重新打包即可解决我的问题（源码包就跟打好的jar包放一起，名字也写上了source，多贴心），修改两个位置,如下
line 76:这个位置是最后一次查询条数，改成Integer.MAX_VALUE。
```
      if(select.getRowCount()>0) {
                        ((TermsAggregationBuilder) lastAgg).size(Integer.MAX_VALUE);
                    }
```
line 109:这个位置是重点，每次的子查询都要设置size，改成Integer.MAX_VALUE。
```
    AggregationBuilder subAgg = getGroupAgg(field, select);
                      //ES5.0 termsaggregation with size = 0 not supported anymore
                    if (subAgg instanceof TermsAggregationBuilder && !(field instanceof MethodField)) {
                        ((TermsAggregationBuilder) subAgg).size(Integer.MAX_VALUE);
                    }

```
里面注释也有说到，5.0以上已经不支持size为0的设置，所以要设置值，不设置的话默认是10。这个问题在线上已经出现了，先准备升级修复这个问题。另外，如果数据超出了Integer.MAX_VALUE，则需要加上更多的过滤条件，似乎超过了查询就会出问题了，这个问题并没有深究。



### 总结
> 找这个问题的过程虽然艰辛，学习的过程让我很快乐，同时我也在es讨论网站也发出相关问题，跟相关的es使用者一起讨论，得到他们的回答，我也是想当开心的。Anyaway ，发现看源码真的让自己学习了很多。**[discuss.elastic.co]( https://discuss.elastic.co/top/all)**

   