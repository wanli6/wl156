---
title: 线性链表
date: 2020-12-01 21:27:48
tags:
---
## 带头结点的线性链表类型
```
#include <stdio.h>
#include <stdlib.h>
#define ElemType int
#define ERROR 0
#define Status int
#define OK 1
#define FALSE 0
#define TRUE 1
typedef struct LNode // 结点类型
{
    ElemType data;
    LNode *next;
} * Link, *Position;
typedef struct // 链表类型
{
    Link head, tail; // 分别指向线性链表中的头结点和最后一个结点
    int len;         // 指示线性链表中数据元素的个数
} LinkList;

void MakeNode(Link &p, ElemType e)
{ // 分配由p指向的值为e的结点。若分配失败，则退出
    p = (Link)malloc(sizeof(LNode));
    if (!p)
        exit(ERROR);
    p->data = e;
}
void FreeNode(Link &p)
{ // 释放p所指结点
    free(p);
    p = NULL;
}
void InitList(LinkList &L)
{ // 构造一个空的线性链表L
    Link p;
    p = (Link)malloc(sizeof(LNode)); // 生成头结点
    if (p)
    {
        p->next = NULL;
        L.head = L.tail = p;
        L.len = 0;
    }
    else
        exit(ERROR);
}
void ClearList(LinkList &L)
{ // 将线性链表L重置为空表，并释放原链表的结点空间
    Link p, q;
    if (L.head != L.tail) // 不是空表
    {
        p = q = L.head->next;
        L.head->next = NULL;
        while (p != L.tail)
        {
            p = q->next;
            free(q);
            q = p;
        }
        free(q);
        L.tail = L.head;
        L.len = 0;
    }
}
void DestroyList(LinkList &L)
{                 // 销毁线性链表L，L不再存在
    ClearList(L); // 清空链表
    FreeNode(L.head);
    L.tail = NULL;
    L.len = 0;
}
void InsFirst(LinkList &L, Link h, Link s) // 形参增加L,因为需修改L
{                                          // h指向L的一个结点，把h当做头结点，将s所指结点插入在第一个结点之前
    s->next = h->next;
    h->next = s;
    if (h == L.tail)      // h指向尾结点
        L.tail = h->next; // 修改尾指针
    L.len++;
}
Status DelFirst(LinkList &L, Link h, Link &q) // 形参增加L,因为需修改L
{                                             // h指向L的一个结点，把h当做头结点，删除链表中的第一个结点并以q返回。
    // 若链表为空(h指向尾结点)，q=NULL，返回FALSE
    q = h->next;
    if (q) // 链表非空
    {
        h->next = q->next;
        if (!h->next)   // 删除尾结点
            L.tail = h; // 修改尾指针
        L.len--;
        return OK;
    }
    else
        return FALSE; // 链表空
}
void Append(LinkList &L, Link s)
{ // 将指针s(s->data为第一个数据元素)所指(彼此以指针相链，以NULL结尾)的
    // 一串结点链接在线性链表L的最后一个结点之后，并改变链表L的尾指针指向新的尾结点
    int i = 1;
    L.tail->next = s;
    while (s->next)
    {
        s = s->next;
        i++;
    }
    L.tail = s;
    L.len += i;
}
Position PriorPos(LinkList L, Link p)
{ // 已知p指向线性链表L中的一个结点，返回p所指结点的直接前驱的位置。若无前驱，则返回NULL
    Link q;
    q = L.head->next;
    if (q == p) // 无前驱
        return NULL;
    else
    {
        while (q->next != p) // q不是p的直接前驱
            q = q->next;
        return q;
    }
}
Status Remove(LinkList &L, Link &q)
{ // 删除线性链表L中的尾结点并以q返回，改变链表L的尾指针指向新的尾结点
    Link p = L.head;
    if (L.len == 0) // 空表
    {
        q = NULL;
        return FALSE;
    }
    while (p->next != L.tail)
        p = p->next;
    q = L.tail;
    p->next = NULL;
    L.tail = p;
    L.len--;
    return OK;
}
void InsBefore(LinkList &L, Link &p, Link s)
{ // 已知p指向线性链表L中的一个结点，将s所指结点插入在p所指结点之前，
    // 并修改指针p指向新插入的结点
    Link q;
    q = PriorPos(L, p); // q是p的前驱
    if (!q)             // p无前驱
        q = L.head;
    s->next = p;
    q->next = s;
    p = s;
    L.len++;
}
void InsAfter(LinkList &L, Link &p, Link s)
{ // 已知p指向线性链表L中的一个结点，将s所指结点插入在p所指结点之后，
    // 并修改指针p指向新插入的结点
    if (p == L.tail) // 修改尾指针
        L.tail = s;
    s->next = p->next;
    p->next = s;
    p = s;
    L.len++;
}
void SetCurElem(Link p, ElemType e)
{ // 已知p指向线性链表中的一个结点，用e更新p所指结点中数据元素的值
    p->data = e;
}
ElemType GetCurElem(Link p)
{ // 已知p指向线性链表中的一个结点，返回p所指结点中数据元素的值
    return p->data;
}
Status ListEmpty(LinkList L)
{ // 若线性链表L为空表，则返回TRUE；否则返回FALSE
    if (L.len)
        return FALSE;
    else
        return TRUE;
}
int ListLength(LinkList L)
{ // 返回线性链表L中元素个数
    return L.len;
}
Position GetHead(LinkList L)
{ // 返回线性链表L中头结点的位置
    return L.head;
}
Position GetLast(LinkList L)
{ // 返回线性链表L中最后一个结点的位置
    return L.tail;
}
Position NextPos(Link p)
{ // 已知p指向线性链表L中的一个结点，返回p所指结点的直接后继的位置。若无后继，则返回NULL
    return p->next;
}
Status LocatePos(LinkList L, int i, Link &p)
{ // 返回p指示线性链表L中第i个结点的位置，并返回OK，i值不合法时返回ERROR。i=0为头结点
    int j;
    if (i < 0 || i > L.len)
        return ERROR;
    else
    {
        p = L.head;
        for (j = 1; j <= i; j++)
            p = p->next;
        return OK;
    }
}
Position LocateElem(LinkList L, ElemType e, Status (*compare)(ElemType, ElemType))
{ // 返回线性链表L中第1个与e满足函数compare()判定关系的元素的位置，
    // 若不存在这样的元素，则返回NULL
    Link p = L.head;
    do
        p = p->next;
    while (p && !(compare(p->data, e))); // 没到表尾且没找到满足关系的元素
    return p;
}
void ListTraverse(LinkList L, void (*visit)(ElemType))
{ // 依次对L的每个数据元素调用函数visit()
    Link p = L.head->next;
    int j;
    for (j = 1; j <= L.len; j++)
    {
        visit(p->data);
        p = p->next;
    }
    printf("\n");
}
void OrderInsert(LinkList &L, ElemType e, int (*comp)(ElemType, ElemType))
{ // 已知L为有序线性链表，将元素e按非降序插入在L中。(用于一元多项式)
    Link o, p, q;
    q = L.head;
    p = q->next;
    while (p != NULL && comp(p->data, e) < 0) // p不是表尾且元素值小于e
    {
        q = p;
        p = p->next;
    }
    o = (Link)malloc(sizeof(LNode)); // 生成结点
    o->data = e;                     // 赋值
    q->next = o;                     // 插入
    o->next = p;
    L.len++;        // 表长加1
    if (!p)         // 插在表尾
        L.tail = o; // 修改尾结点
}
Status LocateElem(LinkList L, ElemType e, Position &q, int (*compare)(ElemType, ElemType))
{ // 若升序链表L中存在与e满足判定函数compare()取值为0的元素，则q指示L中
    // 第一个值为e的结点的位置，并返回TRUE；否则q指示第一个与e满足判定函数
    // compare()取值>0的元素的前驱的位置。并返回FALSE。(用于一元多项式)
    Link p = L.head, pp;
    do
    {
        pp = p;
        p = p->next;
    } while (p && (compare(p->data, e) < 0)); // 没到表尾且p->data.expn<e.expn
    if (!p || compare(p->data, e) > 0)        // 到表尾或compare(p->data,e)>0
    {
        q = pp;
        return FALSE;
    }
    else // 找到
    {
        q = p;
        return TRUE;
    }
}
```