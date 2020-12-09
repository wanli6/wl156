---
title: 数据结构之线性表
date: 2020-11-16 20:12:56
tags:
---
### 线性表
最简单，最常用的一种数据结构。
一个数据元素可以由若干个**数据项**组成，这种情况下，常把数据元素称为**记录**，含有大量线性表的数据文件又称为**文件**。
抽象数据类型线性表的定义如下：

**ADT** List{
**数据对象**：D={ai|ai=ElemSet,i=1,2,..,n,n≥0}
**数据关系**：R1={<ai-1,ai>|ai-1,ai∈D,i=2,...,n}
**基本操作**：
`IniList(&L)`
操作结果：构造一个新的线性表L。
`DestroyList(&L)`
操作结果：销毁线性表
`ClearList(&L)`
操作结果：将L重置为空表
`ListEmpty(L)`
操作结果：若L为空表，则返回TURE，否则返回FALSE
`ListLength(L)`
操作结果：返回L中数据元素个数
`GetElem(L,i,&e)`
初始条件：线性表已存在，1≤i≤ListLength(L)     
操作结果：用e返回L中第i个数据元素的值
`LocateElem(L,e,compare())`
操作结果：返回L中第一个与e满足关系compare()的数据元素的位序。若这样的数据元素不存在，则返回结果为0.
`PriorElem(L,cur_e,&pre_e)`
若cur-e是L的数据元素，且不是第一个，则用pre_e返回它的前驱，否则操作失败，pre_e无定义
`NextElem(L,cur_e,&text_e)`
若cur_e是L的数据元素，且不是最后一个，则用next_e返回它的后继，否则操作失败，next_e无定义
`ListInsert(&L,i,e)`
初始条件：线性表已存在，1≤i≤ListLength(L)+1
操作结果：在L中第i个位置之前插入新的数据元素e,L的长度加1
`ListDelete(&L,i,&e)`
初始条件： 线性表存在且非空，1≤i≤ListLength(L)
操作结果：删除L的第I个数据元素，并用e返回其值，L的长度减一
`ListTravarse(L,visit())`
操作结果：依次对L的每个数据元素调用函数visit().一旦visit()失败，则操作失败。
}ADT List


