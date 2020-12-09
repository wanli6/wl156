---
title: Web前端进阶
date: 2020-11-09 11:06:11
tags:
---
# CSS伪类
## CSS伪类--CSS伪元素
 伪元素就是利用 `css` 代码在标签内部的前面，或者后边添加一个行内元素，这个行内元素可以理解为 `span`

### 清除浮动
使父元素包住浮动的子元素
如下CSS代码就是清除浮动的CSS代码

```
#! CSS
.clearfix::after{
  content: '';
  display: block;
  clear: both;
}
```

**哪个盒子的子元素有浮动，就在哪个盒子上添加清除浮动**

## CSS伪类--事件伪类
#### `hover`--鼠标移动上去
具体代码：
```
li:hover{
    background-color: #47A0FC;
    color: white;
}
```
#### `active`--鼠标点击时
#### `focus`--获取焦点后
## CSS伪类--列表伪类
### 1 匹配父元素中的第一个子元素--`:first-child`
代码：
```
ul>li:first-child{
    background-color: #3687FC;
    color: #FFFFFF;
}
```
### 2 匹配父元素中的最后一个子元素--`:last-child`
### 3 匹配父元素中的第n个子元素--`:nth-child()`

`nth-child()`后边的括号还可以是`odd(偶数）`和`even(奇数)`

# CSS装饰
## cursor
`cursor`的添加方式很简单，如下
```
<p>点击这里了解更多cursor性质</p>
```
```
p{
    cursor: pointer;
}
```
`cursor`的值很多,如下：

```
  cursor: pointer;
  cursor: default;
  cursor: text;
  cursor: move;
  cursor: grab;
  cursor: zoom-out;
  ```

## box-shadow/text-shadow
```
div{
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.11);
}
 ```