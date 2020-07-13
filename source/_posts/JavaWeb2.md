---
title: Java网络编程相关2—POST表单
date: 2020-07-10 13:35:11
tags:
---
## post() 方法
POST操作与API调用不同，POST时数据不是放在URL中，而是在表单中提交的。
因此程序实现需要构建一个`FormBody`表单对象，用于放置表单数据。代码如下：
```
Builder builder = new FormBody.Builder();
// 设置数据，第一个参数是数据名，第二个参数是数据值builder.add("", "");
FormBody formBody = builder.build();
Request request = new Request.Builder().url(url).post(formBody).build();
```

## 使用POST方法提交JSON数据
添加依赖：
```
<!-- JSON 操作库 -->
    <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
    </dependency>
```
详细步骤：
1. 将数据转化为JSON格式的字符串（调用`JSON.toJSONString()`方法。
2. 创建`RequestBody`实例，注意提交类型是`application/json; charset=utf-8`。
3. 构建`Request`实例对象时，调用 `.post(requestBody)`即表示使用JSON的方式提交数据。