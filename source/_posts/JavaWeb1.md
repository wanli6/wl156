---
title: Java网络编程相关1—简单的API调用
date: 2020-07-10 12:55:04
tags:
---

### 一、无参数的Get请求

##### 安装依赖库

在`pom.xml文件`（定义Java文件需要用到的依赖）中添加依赖：

```
<!-- https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp -->
<dependency>
    <groupId>com.squareup.okhttp3</groupId> <artifactId>okhttp</artifactId> 
    <version>4.1.0</version>
</dependency>
```

##### 使用Okhttp3完成页面请求的三个步骤

1. 实例化`OkHttpClient`。代码`OkHttpClient okHttpClient = new OkHttpClient();`
2. 执行调用 
    * i: 实例化`Request`对象，定义请求的各种参数。`Request request = new Request.Builder().url(url).build();`
    * ii: 构建调用对象`Call call = okHttpClient.newCall(request);`
    * iii: 最后执行调用`call.execute()`调用失败就有可能抛出异常，所以必须抓取异常。
3. `call.execute`实际上返回的是一个结果**对象**，调用这个对象的方法就可以得到返回的字符串内容。`call.execute().body().string();`
ps: 记得import用到的类。
`import okhttp3.Call;`
`import okhttp3.OkHttpClient;`
`import okhttp3.Request;`
`import java.io.IOException;`(异常)

## 二、调用API

#### 什么是API

API(Application Programming Interface): 应用程序接口，一般是指一些预先定义的函数，以方便开发人员快速访问某一程序，而无需了解源码。

#### 调用API

API本质就是一个URL，但是其返回内容与URL有明显的区别，没有大量多余的字符。

###### 有参数的Get请求需要把包含参数的完整URL传入方法
