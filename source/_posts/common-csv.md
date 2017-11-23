---
title: csv 笔记
date: 2016-12-02 20:40:41
tags: 
- csv
- myslq
categories: 
- 笔记
---
 ## csv初使用
COMMON-CSV**[官网](http://commons.apache.org/proper/commons-csv/)**
> 项目中批量插入数据库使用的mybatis，在做数据导入的时候，觉得是mybatis的批量导入有问题，后来发现其实是没问题的。当时没看时间花费多少，就找到了common-csv来做测试，在知乎上也有很多人说用csv配合load data会很快，后来测试之后，其实感觉是差不多的，我的数据量是1000左右，可能在1w以上的数据就会出现差异，load data 这个东西感觉是效率很高，使用起来也是很方便，在项目中引用common-csv的jar包即可


### 示例
- mybatis xml 文件配置
```
<insert id="createOrdersByCsv"   >
    LOAD  DATA LOCAL INFILE #{filePath}
   INTO TABLE wo_work_order_test
   FIELDS   TERMINATED BY ','
      lines terminated by '\r\n'
(ID, ENTERPRISE_ID, ORDER_NO,CUSTOMER_ID,
STATUS,PRIORITY,SUBJECT,CONTENT,SOURCE,WORKGROUP,AGENT,IS_DELETE,
STRATEGY,HAS_FILES,DISPATCH_STATUS,BATCH_NO,CREATE_TIME,
UPDATE_TIME,OVER_TIME,HANDING_OVER_TIME,OVER_TIME_FLAG,
HANDING_OVER_TIME_FLAG,WG_RECEIVE_TIME,AG_RECEIVE_TIME);
    </insert>
```
- java 代码生成csv文件
```
/**
     * 通过Csv文件导入mySql数据库
     * @param entities 实体list
     * @return boolean 导入结果
     */
    private boolean createOrdersByCsv(List<OrderEntity>entities){
        boolean result=false;
        String filename = "/usr/local/jiaxin_gw_container-1.0/tmp/"+TimeUtils.getCurrentTimeStamp()+"orders-tmp.csv";
        try {     
        CSVFormat csvFileFormat = CSVFormat.RFC4180.withRecordSeparator("\n");// 创建CSVFormat

        OutputStreamWriter write = new OutputStreamWriter(new FileOutputStream(filename),"UTF-8");
        BufferedWriter bufferedWriter = new BufferedWriter(write);
        CSVPrinter csvFilePrinter = new CSVPrinter(bufferedWriter, csvFileFormat);
        StringBuilder recordStr = new StringBuilder();

        for (OrderEntity entity : entities) {
            recordStr.append(entity.getID() + ",");
            recordStr.append(entity.getEnterpriseID() + ",");
            recordStr.append(entity.getOrderNo() + ",");
            if(entity.getCustomerJID()==null){
                recordStr.append("\\N" + ",");
            }else{
                recordStr.append(entity.getCustomerJID() + ",");
            }
        
            recordStr.append(entity.getStatus() + ",");
            recordStr.append(entity.getPriority() + ",");
            if(entity.getSubject()==null){
                recordStr.append("\\N"+ ",");
            }else{
                recordStr.append(entity.getSubject() + ",");
            }
        if(entity.getDescription()==null){
            recordStr.append("\\N" + ",");
        }else{
            recordStr.append(entity.getDescription() + ",");
        }
            
            recordStr.append(entity.getSource() + ",");

            if(entity.getAcceptWkgroupJID()==null){
                recordStr.append( "\\N"+ ",");
            }else{
                recordStr.append(entity.getAcceptWkgroupJID()+ ",");
            }
 
            if(entity.getAcceptAgentJID()==null){
                recordStr.append("\\N"+ ",");
            }else{
                recordStr.append(entity.getAcceptAgentJID() + ",");
            }
        
            recordStr.append(entity.getIsDelete() + ",");
            recordStr.append(entity.getDispatchStrategy() + ",");
            recordStr.append(entity.getHasAttach()+ ",");
            recordStr.append(entity.getDispatchStatus()+ ",");
            
            recordStr.append(entity.getBatchNo()+ ",");
            recordStr.append(entity.getCreateTime()+ ",");
            recordStr.append(entity.getUpdateTime()+ ",");
            if(entity.getTimeoutTime()==null){
                recordStr.append("\\N"+ ",");
            }else{
                recordStr.append(entity.getTimeoutTime()+ ","); 
            }
        if(entity.getHandingOverTime()==null){
            recordStr.append("\\N"+ ",");
        }else{
            recordStr.append(entity.getHandingOverTime()+ ",");
        }
        
            recordStr.append(entity.getOverTimeFlag()+ ",");
            recordStr.append(entity.getHandingOverTimeFlag());
            recordStr.append("\r\n");
        }
        
                 recordStr.deleteCharAt(0);  
                recordStr.deleteCharAt(recordStr.length()-1);  
                csvFilePrinter.printRecord(recordStr.toString());  
                recordStr.delete(0, recordStr.length()-1);  
                csvFilePrinter.close();
        orderDao.createOrdersByCsv(filename);
        result=true;
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return result;
    }
```
上面的\\N是null的String插入到mysql为null，如果不使用\\N，插入到数据库就是null的字符串，显然不对。

### 总结
 在使用过程中，生成csv文件中，产生的头尾双引号，还没有找到解决方法，所以导致插入数据库是有双引号的。
 需要在api文档中找寻相关解决方法。这是一个问题。
 还有就是分隔符的问题，导入的数据要是比较正常的数据还好，如果出现跟分割符相同的数据，会直接导致插入的数据90%的错误，这个是很严重的问题，所以用的时候要注意。