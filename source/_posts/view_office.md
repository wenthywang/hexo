---
title: 预览office
date: 2017-10-27 10:11:41
tags: 
- 在线预览
- office
categories: 
- Java
---

 ## 各处搜寻最佳方案
   项目中需要在线预览office文档的功能，因此在网络上搜寻大量资料。最终得到以下结论
  -  使用开源openoffice 需要搭建openoffice服务 解析office文档转成pdf 然后通过控制器响应pdf的格式输出文档        再显示到页面
      缺点是 ：效果不好 不推荐使用（简单的office文档或许还行）  

  - 使用第三方在线预览API 这个需要付费 推荐qq邮箱使用的永中第三方
    官网地址：http://dcs.yozosoft.com/  
    注册账号可使用免费试用次数 效果还是很满意的 并且似乎支持所有格式的文档 txt...


  -  使用微软提供url来解析office文档 
  预览地址：https://products.office.com/zh-CN/office-online/view-office-documents-online  
  这种方式是最省钱 省时间 缺点是有大小限制 以及文档类型（只支持office文档word，excel，ppt）官网有写  还有 解析效果不能自定义 但是效果还是蛮不错的
 
  - 总的来说，如果不缺钱就选用第二种，可联系第三方是否可自定义什么的，如果没钱，可选用第三种（前提是部署应用的服务器可外网访问），第一种是备选状态。
  以上仅个人理解。