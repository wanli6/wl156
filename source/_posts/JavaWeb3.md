---
title: Java网络编程相关3—Response相关
date: 2020-07-11 10:10:52
tags:
---
## Response-返回网页内容
在上一片中获取API调用内容代码为`call.execute().body().string()`
而我们通常还需要获得HTTP的状态码，获取状态码代码为`call.execute().code()`，为了避免执行两次请求，优化代码如下：
```
import okhttp3.Response;

// 执行请求
Response rep = call.execute();
// 获取响应状态码
int code = rep.code();
// 获取响应内容
String content = rep.body().string();
```

## Response-返回非文本内容
在请求图片、excel等非字符文本文件时，不能使用`response.body().string();`，而应该使用`response.body().bytes();`来获取二进制编码内容。

## Response-JSON
在JavaWeb2中有将参数对象转化为JSON格式的字符串的方法，也可以将JSON结果转化为对象，方便程序进一步分析。方法为`JSON.parseObject()`

## 解析JSON对象
需要多次取出嵌套的`Map`对象。 例：
```
Map contentObj = JSON.parseObject(content, Map.class);
Map dataObj = (Map)contentObj.get("data");
String city = (String)dataObj.get("city");
```
由于Map可以存储任何对象，所以从Map中`get()`到的对象必须指定其实际的类型： (Map)、(String)