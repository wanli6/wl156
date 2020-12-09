---
title: 栈的操作函数（含测试）
date: 2020-12-07 23:27:48
tags:
---

```
#include<stdio.h>
#include<stdlib.h>
#define STACK_INIT_SIZE 100    //初始分配空间
#define STACKINCREMENT 10 //存储空间分配增量
#define SElemType int
#define Status int
#define OK 1
#define ERROR 0

typedef struct {
    SElemType *base;
    SElemType *top;
    int stacksize;
}SqStack;

// 创建栈，栈长为预设值
Status InitStack(SqStack &S)      
{
    S.base=(SElemType*)malloc(STACK_INIT_SIZE*sizeof(SElemType));
    if(!S.base) return ERROR;
    S.top=S.base;
    S.stacksize=STACK_INIT_SIZE;
    return OK;
}
 
//入栈
Status Push(SqStack &S,SElemType e)   
{
    if(S.top-S.base>=S.stacksize)
    {
        S.base=(SElemType*)realloc(S.base,(S.stacksize+STACKINCREMENT)*sizeof(SElemType));
        if(S.base) return ERROR;
        S.top=S.base+S.stacksize;
        S.stacksize+=STACKINCREMENT;
    }
    *S.top++=e;
    return OK;
}
 
 //删除栈顶元素
Status Pop(SqStack &S,SElemType &e)   
{
    if(S.top==S.base) return ERROR;
    e=*--S.top;
    return OK;
}
 
//栈顶元素
Status GetTop(SqStack S,SElemType &e)   
{
    if(S.top==S.base) return ERROR;
    e=*(S.top-1);
    return OK;
}
 
//计算栈中元素个数
int StackLength(SqStack S)   
{
    int i=0;
    while(S.top!=S.base)
    {
        i++;
        S.top--;
    }
    return i;
}
//判空
Status StackEmpty(SqStack S){
    if(S.top==S.base)
        return true;
    else
        return false;
}
//置空
Status ClearStack(SqStack &S){
    S.top=S.base;
    return OK;
}
//销毁
Status DestroyStack(SqStack &S){
    int len = S.stacksize;
    for(int i =0; i < len; i++){
        free(S.base);
        S.base++;
    }
    S.base = S.top = NULL;
    S.stacksize = 0;
} 
//遍历栈
Status StackTraverse(SqStack S)
{
    SElemType *p=(SElemType*)malloc(sizeof(SElemType));
    p=S.top;
 
    if(S.top==S.base)
        printf("The Stack is Empty!");
    else
    {
        printf("The Stack is:");
        while(p!=S.base)
        {
            p--;
            printf("% d",*p);
        }
    }
    printf("\n");
    return OK;
}
 
//主函数
int main()
{
    int a;
    SqStack S;
    SElemType x,e;
    if(InitStack(S)) printf("A Stack Has Created.\n");
    while(1)
    {
        printf("1:Push\n2:Pop\n3:Get the Top\n4:Return the Length of the Stack\n5:Load the Stack\n0:Exit\nPlease choose:\n");
        scanf("%d",&a);
        switch(a)
        {
        case 1:
            scanf("%d",&x);
            if(!Push(S,x)) printf("Push Error!\n");
            else printf("The Element %d is Successfully Pushed!\n",x);
            break;
        case 2:
            if(!Pop(S,e)) printf("Pop Error!\n");
            else
 
                printf("The Element %d is Successfully Poped!\n",e);
            break;
        case 3:
            if(!GetTop(S,e)) printf("GetTop Error!\n");
            else printf("The Top Element is %d!\n",e);
            break;
        case 4:
            printf("The Length of the Stack is %d!\n",StackLength(S));
            break;
        case 5:
            StackTraverse(S);
            break;
        case 0:
            return 1;
        }
    }
}
```