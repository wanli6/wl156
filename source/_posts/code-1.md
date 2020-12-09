---
title: 线性表的代码实现
date: 2020-11-16 22:29:51
tags:
---

#### 包含各种功能函数的头文件`"list.h`
```
#ifndef _LIST_H_

#define _LIST_H_

//代码部分
#include <stdio.h>
#include <stdlib.h>
#define ERROR 0
#define OVERFLOW 0
#define ok 1
#define TURE 1
#define FALSE 0
#define LISTINCREMENT 5
#define LIST_INIT_SIZE 10
typedef int Status;
typedef int ElemType;
typedef struct
{
    ElemType *elem;
    int length;
    int listsize;
} SqList;
//利用数组初始化l
void CreateList(SqList *L, ElemType a[5], int n){
    int i;
    for (i=0; i<n; i++){
        *(L->elem+i) = a[i];
    }
    L->length=n;

}//初始化一个顺序表
//初始化线性表
Status InitList(SqList *l)
{
    l->elem = (int *)malloc(LIST_INIT_SIZE * sizeof(int));
    if (!l->elem)
    {
        printf("无法申请空间!\n");
        exit(OVERFLOW);
    }
    l->length = 0;
    l->listsize = LIST_INIT_SIZE;
    return ok;
}
//销毁一个线性表
Status DestoryList(SqList *L)
{
    free(L->elem);
    L->elem = NULL;
    L->length = 0;
    L->listsize = 0;
    return ok;
}
//将L重置为空表
Status ClearList(SqList *l)
{
    l->length = 0;
    return ok;
}
//线性表L已存在
Status ListEmpty(SqList *l)
{
    if (l->length == 0)
        return true;
    else
        return false;
}
//返回线性表中数据元素个数
Status ListLength(SqList *l)
{
    return l->length;
}
//获取L中第i个赋值给e
Status GetElem(SqList *L, int i, int *e)
{
    *e = *(L->elem + i);
    return ok;
}
//返回第一个与e相等的数的下标
Status LocateList(SqList L, int e)
{
    int i;
    int *p = L.elem;
    for (i = 0; i < L.length; i++, p++)
    {
        if (*p == e)
            return i;
    }
    return FALSE;
}
//若cur_e不是第一个，返回前驱
Status PriorElem(SqList L, int cur_e, int *pri_e)
{
    int i;
    int *p = L.elem;
    for (i = 0; i < L.length; i++, p++)
    { //顺序表长度已知，故用for循环
        if (i == 0 && *p == cur_e)
            return FALSE;
        if (*p == cur_e)
        {                  //找到了当前元素且不是第一个元素，
            *pri_e = *--p; //将其前驱赋给引用参数
            return TURE;
        }
    }
    return FALSE;
}
//若cur_e不是最后一个，返回后继
Status NextElem(SqList L, int cur_e, int *Nex_e)
{
    int i;
    int *p = L.elem;
    for (i = 0; i < L.length; i++, p++)
    {
        if (i == L.length - 1 && *p == cur_e)
            return FALSE;
        if (*p == cur_e)
        {
            *Nex_e = *++p;
            return TURE;
        }
    }
    return FALSE;
}
//在指定位置插入元素,在L的第i个位置插入元素e
Status ListInsert(SqList *l, int i, int e)
{
    if (!l->length) //有空余空间
    {
        l->length++;
        l->elem[l->length - 1] = e;
        return ok;
    }
    else if (i < 1 || i > l->length + 1)
    {
        printf("插入位置错误!\n");
        return OVERFLOW;
    }
    if (l->length == l->listsize) //表已满
    {
        int *newbase;
        newbase = (ElemType *)realloc(l->elem, (l->listsize + LISTINCREMENT) * sizeof(ElemType));
        if (!newbase)
        {
            printf("增加空间失败!\n");
            return OVERFLOW;
        }
        l->elem = newbase;
        l->listsize += LISTINCREMENT;
    }
    int *p;
    p = l->elem + l->length - 1;
    while (p >= l->elem + i - 1)
        *(p + 1) = *p--;
    l->elem[i - 1] = e;
    l->length++;
    return ok;
}
//删除第i个元素，用e返回其值
Status ListDelete(SqList *L, int i, int *e)
{
    int *p, *q;
    if (i < 1 || i > L->length)
        return FALSE;
    p = L->elem + i - 1;         //p指向要删除的元素的位置
    q = L->elem + L->length - 1; //q指向表中最后一个元素位置
    *e = *p;                     //将要删除的元素保存起来
    for (; p <= q; p++)          //从要删除元素的后面一个元素开始移动元素
        *p = *(p + 1);
    L->length--; //表长减一
    return TURE;
}
//遍历L
Status TravelList(SqList L)
{
    int i;
    int *p = L.elem;
    for (i = 0; i < L.length; i++, p++)
    {
        printf("第%d个元素为：%d\n", i, *p);
    }
    return TURE;
}


#endif
```

#### 利用头文件的一个简单的应用
```
#include "list.h"
void MergeList(SqList La,SqList Lb,SqList &Lc){
    int i,j,k,La_len,Lb_len,ai,bj;
    i=j=0;
    k=0;
    La_len = La.length;
    Lb_len = Lb.length;
    while(i<La_len&&j<Lb_len){
        GetElem(&La, i, &ai);
        GetElem(&Lb, j, &bj);
        if(ai<=bj) {ListInsert(&Lc, k++, ai);++i;}
        else{ListInsert(&Lc, k++, bj);++j;}
    }
    while (i<La_len) {
        GetElem(&La, i++, &ai);
        ListInsert(&Lc, k++, ai);
    }
    while (j<Lb_len) {
        GetElem(&Lb, j++, &bj);
        ListInsert(&Lc, k++, bj);
    }
}
int main(){
    int la[5] = {1,3,5,7,9};
    int lb[5] = {2,3,6,8,10};
    SqList La,Lb,Lc;
    InitList(&La);
    InitList(&Lb);
    InitList(&Lc);
    CreateList(&La, la, 5);
    CreateList(&Lb, lb, 5);
    MergeList(La, Lb, Lc);
    TravelList(Lc);
    system("pause");
    return 0;
}

```