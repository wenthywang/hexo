---
title: 百度文字识别API接入
date: 2017-07-04 17:30:41
tags: 
- 百度API
- 
categories: 
- 验证码识别
---

 ## First
  一个偶然的链接进去了百度的管理控制台，一扫而过的各种功能真是厉害了。然后看到文字识别，之前帮别人开发一个模拟登陆获取数据（有点像爬虫），有个很关键的点就是验证码了。想了想百度可以文字识别，验证码就不过也如此。
  - 注册百度账号
  - 在文字识别添加一个应用
  - 复制APPid和APPsecret
  - 填写获取token的地址
  - 发送请求
``` 
      String url = "https://aip.baidubce.com/oauth/2.0/token";
              JSONObject obj=new JSONObject();
              obj.put("grant_type", "client_credentials");
              obj.put("client_id", "XXX");
              obj.put("client_secret", "YYY");
              String content=HttpUtil.post(url, "json", obj.toJSONString(), null);
```
请求回来的数据是json字符串，解析一下就能获取token了。

## Second
  获取了token之后就要真正让百度帮我识别验证码了。这时候需要用到百度的sdk，这个包也是在百度的文档中提供，下载之后导入工程即可。
  按照文档说，图片需要进行base64编码，还好sdk提供了工具类。

  ```
         String url="https://aip.baidubce.com/rest/2.0/ocr/v1/webimage?access_token="+access_token;
              AipRequest r=new AipRequest();
              String base64Img= Base64Util.encode( FileCopyUtils.copyToByteArray(file));
              r.setUri(url);
              r.addHeader("Content-Type", "application/x-www-form-urlencoded");
              HashMap<String,Object>params=new HashMap<String,Object>();
              params.put("image", base64Img);
              r.setBody(params);
              AipResponse resp= AipHttpClient.post(r);
              System.out.println(resp.getBodyStr());
``` 
响应后的数据就是有识别的结果了，返回来的数据也是json串，需要进行解析。

## Finally
 看了下token的过期时间，是一个月，所以我就把token转成json文件保存在本机，就不需要每次都要请求获取token了，另外在获取token的时候需要判断是否已经过期了。刚尝试了接触过的两种验证码，较为复杂的，有干扰线的，需要去除掉，然后进行图像处理，再调用百度的API才能识别出来，另外一种是没有干扰线的，这个直接调用就出现结果，准确率估计能达到90%，这个还是很不错的，个人觉得如果有这种需求的话可以使用百度的API，虽然有些次数限制，并发限制，总体来说还是可以的，不过也可以付费放开所有限制。