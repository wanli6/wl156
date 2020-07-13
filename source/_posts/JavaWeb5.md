---
title: Java网络编程相关5—下载操作
date: 2020-07-12 14:27:53
tags:
---
## 下载文件
在Java中写入文件必须经过三个步骤：
* 创建文件对象
* 写入内容
* 关闭写入内容

#### 写入文本文件
```
import java.io.File;
import java.io.FileWriter;

// 文件对象
File file = new File("foo.txt");

// 写入内容
FileWriter fileWritter = new FileWriter(file.getName());
fileWritter.write(content);

// 关闭
fileWritter.close();
```
**写入文件类必须关闭**

#### 写入二进制文件
```import java.io.File;
import java.io.FileOutputStream;

// 文件对象
File file = new File("china-city-list.xlsx");

// 写文件
FileOutputStream fos = new FileOutputStream(file);
fos.write(data);

// 必须刷新并关闭
fos.flush();
fos.close();
```
**必须执行刷新，关闭操作**
（图片与二进制操作一致）

## 解析excel
阿里出品的easyexcel是快速、简单操作excel的库，需要加入如下的依赖：
```
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>easyexcel</artifactId>
  <version>2.1.6</version>
</dependency>
```
#### 调用库
```
import com.alibaba.excel.EasyExcel;
import java.util.Map;
import java.util.List;

// 读取第一个sheet
List<Map<Integer, String>> sheetDatas = EasyExcel.read("xzq_201907.xlsx").sheet(0).doReadSync();
// List 中每个元素表示一行
for (Map<Integer, String> rowData : sheetDatas) {
  // Map 中用序号指代每一列
  for (Integer index : rowData.keySet()) {
    // 列值
    String columnValue = rowData.get(index);
  }
}
```
 