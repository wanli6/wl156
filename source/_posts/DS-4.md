---
title: 数组与广义表（一）
date: 2020-12-08 22:54:20
tags:
---

# 数组
数组完成的操作较少，只有构造，销毁，取值，赋值。
## 数组的数据结构定义
```
ADT  Array {
	数据对象:ji=0,…,bi – 1,  i = 1,2,…,n, //bi是数组第i维上的长度
	        D={aj1aj2···ajn|aj1aj2···ajn∈ElemSet }
	数据关系:R = {R1,R2,…,Rn}
		        Ri={<aj1···aji···aj,aj1···aji+1···ajn>|0≤jk≤bk–1, 1≤k≤n,
                k!=i0≤ji≤bi–2,<aj1···aji···aj,aj1···aji+1···ajn>∈D, i=2,3,…,n}
	基本操作：
	InitArray(&A,n,bound1,…,boundn)
	DestroyArray(&A)
	Value(A,&e,index1,…,indexn)
	Assign(&A,e,index1,…,indexn)	       
}ADT Array	
```
## 数组的顺序存储
顺序存储的定义
```
typedef struct{
    ElemType *base; //数组元素基址
    int dim;        //数组维度
    int *bounds;    //数组维界基址
    int *constants; //数组映像函数常量基址
} 
```

计算数组任一元素的地址需要的三要素：
1. 数组的起始地址（即基地址）
2. 数组维数和各维的长度；
3. 数组中每个元素所占的存储单元

已知二维数组A（b1,b2)，每个元素占L个存储单元， LOC(0,0)是数组第一个元素的起始地址，以行序为主存储，求LOC(i,j)。
**行主序：LOC(i,j) = LOC(0,0) + ( b2*i + j ) * L**
**列主序：LOC(i,j) = LOC(0,0) + ( b1*j + i ) * L**
## 稀疏矩阵
### 特殊矩阵-对称矩阵
```
      i(i+1)/2+j  (i≥j)(存储下三角元素)
k=
       j(j+1)/2+i (i＜j)(存储上三角元素)
```
### 特殊矩阵-（上）下矩阵
```
下：
    i*(i-1)/2+j-1 (i≥j)
k=
      n*(n+1)/2      i＜j
上：
        (i-1) (2n-i+2)/2+j-i (i≤j))
k=
        n*(n+1)/2      i>j
```
### 特殊矩阵-对角矩阵
不在第一行的非零元ai,j，它前面已经存储了前i-1行元素为2+3(i-2)
若ai,j是本行需第1个存储的元素(ai,i-1)，k=3(i-1)-1，(j=i+1)
若ai,j是本行需第2个存储的元素(ai,i) ，k=3(i-1)，(i=j)
若ai,j是本行需第3个存储的元素(ai,i+1) ，k=3(i-1)+1，(j=i+1)
三式合并有**k=2(i-1)+j-1**。


```
Status TransposeSMatrix(TSMatrix M,TSMatrix &T)
{   T.mu=M.nu;T.nu=M.mu;T.tu=M.tu;
     if(T.tu)          //tu非零元个数, mu行数， nu列数
     {   q=1;
          for(col=1;col<=M.nu;++col)
             for(p=1;p<=M.tu;++p)
              　if(M.data[q].j==col)
              　{   T.data[q].i=M.data[p].j; T.data[q].j=M.data[p].i;
               　   T.data[q].e=M.data[p].e; ++q;}   }
   return OK;
}//TransposeSMatrix
```
## 矩阵的压缩存储
###  三元组顺序表
```
//-----稀疏矩阵的三元顺序表存储表示-------
#define MAXSIZE 12500   //假设非零元个数的最大值
typedef struct{
	int    i , j;     //该非零元的行下标和列下标
	ElemType e;
}Triple;
typedef struct{
	Triple data[MAXSIZE + 1];	//非零元三元组表，data[0]未用
	int   mu,nu,tu;				//矩阵的行数、列数和非零元个数
}
```
矩阵的转置(1)
```
Status TransposeSMatrix(TSMatrix M,TSMatrix &T)
{   T.mu=M.nu;T.nu=M.mu;T.tu=M.tu;
     if(T.tu)          //tu非零元个数, mu行数， nu列数
     {   q=1;
          for(col=1;col<=M.nu;++col)
             for(p=1;p<=M.tu;++p)
              　if(M.data[q].j==col)
              　{   T.data[q].i=M.data[p].j; T.data[q].j=M.data[p].i;
               　   T.data[q].e=M.data[p].e; ++q;}   }
   return OK;
}//TransposeSMatrix
```
**评价**:	O(nu*tu)，
**优点**:	节省空间， `tu<<mu*nu`适用


矩阵的转置(2)--快速转置
设`num`和`cpot`两个向量
`num[col]` ：矩阵M的第col列中非零元素的个数，
`cpot[col]`: 矩阵中第col列第一个非零元在T.data中的位置
````
cpot[1]=1;
cpot[col]=cpot[col-1]+num[col-1] (2<=col<=a.nu)
````
```
Status FastTransposeSMatrix(TSMatrix M,TSMatrix &T)
{  T.mu=M.nu;T.nu=M.mu;T.tu=M.tu;
    if(T.tu) 
    {  for(col=1;col<=M.nu;++col) num[col]=0;
        for(t=1;t<=M.tu;++t) ++num[M.data[t].j];//求每列非零元个数
        cpot[1]=1;
        for(col=2;col<=M.nu;++col) 
              cpot[col]=cpot[col-1]+num[col-1];
        for(p=1;p<=M.tu;++p) 
        {   col=M.data[p].j;q=cpot[col];
             T.data[q].i=M.data[p].j; T.data[q].j=M.data[p].i;
             T.data[q].e=M.data[p].e;++cpot[col];       }
    }
    return OK;
}//TransposeSMatrix  
```
时间复杂度为`O(nu+tu)`,若`tu~mu*nu`
则为`O(mu*nu)`，与经典算法相同，多用了两个辅助向量。

###  行逻辑链接的顺序表
需求：随机存取任一行的非0元
方法：记住`Am*n`每一行第一个非0元在三元组表中的位置
设数组`rpos[1..n]`:矩阵中的每行第一个非零元素的起始位置。
　　　`rpos[1]=1`;
      rpos[row]=rpos[row-1]+第row-1行的非零元素个数
行逻辑链接顺序表:在稀疏矩阵存储结构中固定指示行信息的辅助数组rpos
```
-------行逻辑链接表存储结构的Ｃ语言描述--------
typedef struct
{   Triple data[MAXSIZE+1]; 	//非零元三元组表
     int rpos[MAXRC+1];			//各行第一个非零元素位置表
     int mu,nu,tu;          	//矩阵的行、列、非零元个数
}RLSMatrix 
```

稀疏矩阵相乘：
```
Status MultSMarix (RLSMatrix& M, RLSMatrix& N, RLSMatrix& Q){
	if (M.nu != N.mu) 	return ERROR;
	Q.mu=M.mu;Q.NU=N.nu;Q.tu=0; // Q初始化
	if (M.tu*N.tu != 0) {  // Q是非零矩阵
		for (arow=1; arow<=M.mu; ++arow) {	// 处理M的每一行
			ctemp[] = 0;    // 当前行各元素累加器清零
			Q.rpos[arow] = Q.tu+1;	//设当前行第一个非零元在三元组表中的位序   
			if(arrow<M.mu)tp=M.rpos[arrow+1];    
			else tp=M.tu+1;   
		for (p=M.rpos[arow]; p<tp; ++p)	// 对当前行中每一个非零元
			brow=M.data[p].j;  	// 找到对应元在N中的行号
			if (brow < N.nu )  t = N.rpos[brow+1];
			else  { t = N.tu+1 }  // t指示该行中最后一个非零元的位置
			for (q=N.rpos[brow];  q< t;  ++q) {
				ccol = N.data[q].j;   	// 乘积元素在Q中列号
				ctemp[ccol] += M.data[p].e * N.data[q].e;     
			} // for q
		} // 求得Q中第crow( =arow)行的非零元
		for (ccol=1; ccol<=Q.nu; ++ccol) 	// 压缩存储该行非零元
			if (ctemp[ccol]) {  // 该列元素为非零元
				if (++Q.tu > MAXSIZE)  return ERROR;
				Q.data[Q.tu] = { arow, ccol, ctemp[ccol] };
			} // if         
		} // for arow     
	} // if      
	return OK;    
} //MultSMatrix
```
累加器ctemp初始化的时间复杂度为: `O(M.mu*N.mu)`，
求Q的所有非零元的时间复杂度为:     `O(M.tu*N.tu/N.mu)`，
进行压缩存储的时间复杂度为:            `O(M.mu*N.nu)`，
总的时间复杂度: `O(M.mu*N.nu+M.tu*N.tu/N.mu)`。

###  十字链表
当非零元个数和位置在操作过程中变化较大时，就不宜采用顺序存储结构，采用链式存储更恰当。
```
存储结构的C语言描述
typedef struct OLNode{
	int i,j;	//行列下标
	ElemType e;  
	Struct OLNode *right,*down;	//该非零元所在的行表和列表所在的链域
}OLNode,*OLink;
typedef struct{
	OLink *rhead,*chead;	//行和列链表头指针向量基址
	int mu,nu,tu;	//稀疏矩阵的行数，列数和非零元个数
}CrossLink;

```