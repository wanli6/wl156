---
title: Java网络编程相关4—headers
date: 2020-07-11 10:55:13
tags:
---
## User-Agent
#### HTTP header
`HTTP`消息头`Headers`是`HTTP`协议的一项重要内容，作用是在发起请求的时候，除了请求参数，可以附加更多的东西。（Headers属于隐藏的数据，不能被直接看到）。
#### User-Agent
`User-Agent`是存放在`Headers`中的一种数据信息。作用是在指定URL发送请求的时候，告诉服务端当前用户的浏览器版本，类型，甚至操作系统，CPU等非隐私的`技术信息`。

使用`addHeader()`方法将`User-Agent`添加到`Headers`中
例：
```
Request request = new Request.Builder().url(url).addHeader("User-Agent", "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1").build();
```

## Referer
除了User-Agent还有一种更严格的检查，图片防盗链。
浏览器在请求**网页**中的图片或其它文件时，会**自动**在`Headers`中加一个`Referer`信息，表示请求的来源。
可以用以下语句模拟Referer
```
Request request = new Request.Builder().url(url).addHeader("Referer", "https://ham.youkeda.com/course/j14/3/1/0")    
                            .build();
```

## Host
Host表示当前请求的域名。虽然域名已经存在于URL中，但遇到复杂的场景，例如使用代理服务器、或者URL中不写域名而是写IP地址，设置HOST就非常有用了。
例：
```
Request request = new Request.Builder().url(url).addHeader("Host", "www.douban.com").build();
```
ps: Host的值一定是一个域名并且**不带协议头**。