---
title: 栈与队列（二）
date: 2020-12-07 21:07:12
tags:
---
# 队列
和**栈**相反，**队列**是一种**先进先出**（first in first out,缩写**FIFO**)的线性表。它只允许在表的一端进行插入，另一端删除元素。
## 队列的抽象数据类型定义

    ADT Queue{

    数据对象：D={ai|ai∈ElemSet, i=1,2, …,n, n≥0}

    数据关系：R1={<ai-1,ai>|ai-1,ai∈D, i=1,2, …,n}
    约定a1为队列头，an为队列尾。
    
    基本操作：

        InitQueue( &Q )
        操作结果：构造一个空队列Q。
        DestroyQueue ( &Q )
        初始条件：队列Q已存在。
        操作结果：销毁队列Q。
        ClearQueue ( &Q )
        初始条件：队列Q已存在。
        操作结果：将Q清为空队列。
        QueueEmpty( Q )
        初始条件：队列Q已存在。
        操作结果：若Q为空队列，则返回TRUE，否则返回FALSE。
        QueueLength( Q )
        初始条件：队列Q已存在。
        操作结果：返回Q的数据元素个数，即队列的长度。
        GetHead( Q, &e )
        初始条件：队列Q已存在且非空。
        操作结果：用e返回Q的队头元素。
        EnQueue( &Q, e )
        初始条件：队列Q已存在。
        操作结果：插入元素e为Q的新的队尾元素。
        DeQueue( &Q, &e )
        初始条件：队列Q已存在且非空。
        操作结果：删除Q的队头元素，并用e返回其值。
        QueueTraverse( Q, visit() )
        初始条件：队列Q已存在且非空。
        操作结果：从队头到队尾依次对Q的每个数据元素调用函数visit()。一旦visit()失败，则操作失败。
    }ADT Queue