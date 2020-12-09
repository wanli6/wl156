---
title: 队列操作函数的代码实现(含测试程序)
date: 2020-12-07 21:18:31
tags:
---
```
//队列操作实现
//单链队列---队列的链式存储结构
#include<stdio.h>
#include<stdlib.h>
#define OK 1
#define ERROR 0
typedef int Status;
typedef int QElemType;

typedef struct QNode{
    QElemType data;
    struct QNode *next;
}QNode,*QueuePtr;
typedef struct {
    QueuePtr front;//队头
    QueuePtr rear;//队尾
}LinkQueue;
//初始化队列
Status InitQueue(LinkQueue &Q){
    Q.front= Q.rear=(QueuePtr)malloc(sizeof(QNode));
    if(!Q.front) return ERROR;
    Q.front->next=NULL;
    return OK;
}
//销毁队列
Status DestroyQueue(LinkQueue &Q){
    while(Q.front){
        Q.rear=Q.front->next;
        free(Q.front);
        Q.front=Q.rear;
    }
    return OK;
}

//插入
Status EnQueue(LinkQueue &Q,QElemType e){
    QueuePtr p;
    p=(QueuePtr)malloc(sizeof(QNode));
    if(!p)  exit(0);
    p->data=e;
    p->next=NULL;
    Q.rear->next=p;
    Q.rear=p;
    return OK;
}

//删除
Status DeQueue (LinkQueue &Q,QElemType &e){
    QueuePtr q;
    if(Q.front==Q.rear) return ERROR;
    q=Q.front->next;
    e=q->data;
    Q.front->next=q->next;
    if(Q.rear==q) Q.rear = Q.front;
    free(q);
    return OK;
}
//判空
Status QueueEmpty(LinkQueue &Q){
    if(Q.rear==Q.front)    return OK;
    else return 0;
}
//清除队列
Status ClearQueue(LinkQueue &Q){
    QElemType e;
    while(!QueueEmpty(Q))  DeQueue(Q,e);
    return OK;
}
//队列长度
int QueueLength(LinkQueue &Q){
    int length=0;
    QueuePtr p,q;
    p=Q.front;  q=Q.rear;
    while(p!=q) {
        p=p->next;
        length++;
    }
    return length;
}
//获取队头元素
Status GetHead(LinkQueue &Q, QElemType e){
    e=Q.front->next->data;
    return OK;
}

//遍历输出
Status QueueTraverse(LinkQueue &Q){
    QueuePtr p,q;
    QElemType e;
    p=Q.front;
    while((p=p->next)!=Q.rear->next){
        e=p->data;
        printf("%d  ",e);
    }
    return OK;
}


int main()
{
    LinkQueue Q;
    InitQueue(Q);
	EnQueue(Q, 1);
	EnQueue(Q, 2);
	EnQueue(Q, 3);
	EnQueue(Q, 4);
	EnQueue(Q, 5);
	EnQueue(Q, 6);
	EnQueue(Q, 7);
	EnQueue(Q, 8);
	EnQueue(Q, 9);
	printf("链队列元素个数：%d\n",QueueLength(Q));
	printf("展示元素：\n");
	QueueTraverse(Q);
	int e1, e2;
	DeQueue(Q, e1);
	DeQueue(Q, e2);
	printf("删除元素：%d,%d\n", e1, e2);
	printf("展示元素：\n");
	QueueTraverse(Q);
    printf("\n");
	printf("链队列元素个数：%d\n", QueueLength(Q));
	ClearQueue(Q);
	printf("清空元素后，长度为%d，6rear = %p，front=%p",QueueLength(Q), Q.rear,Q.front);
	printf("\n");
	system("pause");
    return 0;
}
```