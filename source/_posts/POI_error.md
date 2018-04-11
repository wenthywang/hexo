---
title:  项目中POI导出出现HTML特殊符号的实体--已解决
date: 2016-07-28 22:37:07
tags: 
- Java
- POI
- 特殊符号
categories: 
- Java
- 问题
---
## 问题：
 	导出excel 时出现 类似这样的&gt;符号，大概是存到数据库也是这样，然后jsp解析可以解析出来，但是java不认得，需要个人写出解析方法。

## 废话不说,贴码：
``` bash
  /**
*转换html特殊符号。
* @param content 需要转换的html特殊符号
* @param defaultName 默认返回值
* @return 转化后实际的符号
*/
public static String transferHtml(String content, String defaultName) {
if(content==null) return defaultName; 
String html = content;
html = StringUtils.replace(html, "&quot;", "\"");
html = StringUtils.replace(html, "&lt;", "<");
html = StringUtils.replace(html, "&gt;", ">");
html = StringUtils.replace(html, "&gt;", ">");
html = StringUtils.replace(html, "&sim;", "~");
html = StringUtils.replace(html, "&and;", "^");
html = StringUtils.replace(html, "&hellip;", "...");
return html;
}
```



### 总结
	StringUtils用的是apach的工具类。
另外，我也找过度娘，对比了一下StringUtils的replace和String自带的replaceAll方法。

具体就参考 [**String自带replaceAll和StringUtils工具类replace区别**](http://blog.sina.com.cn/s/blog_8f99a1640102v6q2.html)    这博主分析得挺不错的。

另外我也度了一下html特殊符号的对照表，具体参考 [**HTML 特殊符号编码对照表**](http://tool.chinaz.com/tools/htmlchar.aspx)

	总结：根据个人需要把某些常用的特殊符号解析添加到自己的项目中去。
