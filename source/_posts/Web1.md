---
title: Web前端基础
date: 2020-07-22 15:45:38
tags:
---
## HTML标签的特点

1. 由**尖括号**包围关键词组成。
2. 成对出现。例如： <p>  </p>
3. 并不是所有标签都有其对应的结束标签。

## 外部样式常用的选择器
1. 标签选择器
2. 类选择器
````
<p class="article">
  class是定义类的关键字，article是类名，类名可以任意，但是要符合规范
</p>
.article{
          color:skyblue;
      }
````
3. id选择器
````
<p id="p-item">这是一段文字</p>
#p-item {
  font-size: 24px;
  font-weight: 400;
}
````

## 高级选择器
* 后代选择器
* 交集选择器
* 子选择器
* 并集选择器
### 1 后代选择器
/* 选择id名为password的标签内部所有类名为box的元素内部的所有p标签 */
#password .box p{}
/* 选择所有p标签内部的所有span标签 */
p span{}
/* 选择所有p标签内部的所有类名为spanItem的标签 */
p .spanItem{}
### 2 交集选择器
写法：`a.special{}`
````
<a href="#" class="special">超链接</a>
<a href="#">超链接</a>
<a href="#">超链接</a>
<a href="#">超链接</a>
````
意思是a标签中类名为special的标签

### 3 子选择器
写法： p>span
与后代选择器类似
区别：后代选择器选择所有后代，子代选择器只选择子代

### 4 并集选择器
写法：.box,p,h3,.phone{}
上面这段代码的意思是给类名为box,phone标签名为p、h3的标签添加相同的属性。