---
title: POJ 2479 Maximum sum
date: 2016-10-9 22:35:00
tags:
- dp
- poj
grammar_cjkRuby: true
categories:
-  CS
comments: true
---

动态规�?Dynamic Programming)中关于最大子序列问题的变形�?<!---more--->

### 题目描述

* 原题地址:		[POJ 2479][1]
* 题目大意:    求不相交两段子段和的最大�?
---

### 解题过程
#### 方法一(超时)
##### 解题思路
首先想到的就是最大子序列的问题，我的第一个思路就是将这个数组分成两段（界限设为m,从下�?开始，到N-1结束），分别求这两个数组的最大子序列，然后将其求和。再从N-2个最大和中取其中的最大值�?
具体思路如下�?1. 将数组用m划分，分别划分为:
    * num\[1],num\[2]->num\[N]
    * num\[1]->num\[2],num\[3]->num\[N]
    * .......
    * num\[1]->num\[N-1],num[N]
2. 对上述N-1对数组，分别求前半段和后半段最大子序列，并将其相加求出该划分条件下，前后半段的最大和�?3. 从N-1个最大和之中选出最大的结果�?

<div align="center">
![方法一示例图][2]
方法一示例�?</div>

##### 代码
```cpp
#include<iostream>
using namespace std;
int num[50001];
int main(){
	int T,N;
	cin>>T;
	while(T--){
		cin>>N;
		for(int i=0;i<N;i++)
			cin>>num[i+1];
		int max_sum=0,cur_max_sum=0;
		for(int m=2;m<=N;m++){//m划分整个数组
		int   pre_next_sum=0,pre_max_sum=0;
		int   latter_next_sum=0,latter_max_sum=0;
			for(int i=1;i<=m;i++){//求前半段的最大子序列
				pre_next_sum+=num[i];
				if(pre_next_sum>pre_max_sum)
					pre_max_sum=pre_next_sum;
				else if(pre_next_sum<0)
					pre_next_sum=0;
			}
			for(int j=m+1;j<=N;j++){//求后半段的最大子序列
				latter_next_sum+=num[j];
				if(latter_next_sum>latter_max_sum)
					latter_max_sum=latter_next_sum;
				else if(latter_next_sum<0)
					latter_next_sum=0;		
			}
			cur_max_sum=pre_max_sum+latter_max_sum;
			if(cur_max_sum>max_sum)//取最大的max_sum
				max_sum=cur_max_sum;
			
		}	
		cout<<max_sum<<endl;
	}
}
```
但是上述方法提交后却出现了超时的问题，可能是嵌套了两个for循环，所以不得不另辟蹊径降低时间复杂度，我们由此得出方法二�?

#### 方法�?##### 解题思路
此方法前半段与方法一相同，均是对数组从左到右求最大子子段和，但是需要用一个数组ltor_sum\[i]来存储num\[1]->num\[i]的最大子子段和�?
具体思路如下:
1. 从左到右依次求出ltor_sum\[i]的值�?2. 再从右到左计算最大子子段和�?3. 将第二步求出的最大值与对应的ltor_sum\[i]求和�?4. 我们使用max_sum存储结果，当从右到左遍历到i=2�?我们将算出正确结果�?<div align="center">
![enter description here][3]
方法二示例图
</div>

##### 代码实现

ltor_sum\[i]\(ltor=left_to_right):	存储num\[1]到num[i]的最大和数组
ltor_temp_max:	存储num\[1]到num[i]的最大和的临时�?ltor_max_sum:	存储num\[1]到num[i]的最大和
max_sum:    代表最终结�?```cpp
#include<iostream>
#define MIN -0x3f3f
using namespace std;
int num[50001],ltor_sum[50001];
int main(){
	int T,N;
	int ltor_temp_max,ltor_max_sum;
	int max_sum;
	int rtol_temp_sum=0,rtol_max_sum;
	cin>>T;
	while(T--){
		scanf("%d",&N);
		ltor_temp_max=0,ltor_max_sum=MIN;
		for(int i=1;i<=N;i++){
			scanf("%d",&num[i]);
			ltor_temp_max+=num[i];
			if(ltor_temp_max>ltor_max_sum)
				ltor_max_sum=ltor_temp_max;
			ltor_sum[i]=ltor_max_sum;
			if(ltor_temp_max<0)
				ltor_temp_max=0;
			
		}
		max_sum=MIN;
		rtol_temp_sum=0,rtol_max_sum=MIN;
		for(int i=N;i>1;i--){
			rtol_temp_sum+=num[i];
		        rtol_max_sum=max(rtol_max_sum,rtol_temp_sum);	
			if(rtol_temp_sum<0)
		           rtol_temp_sum=0;
		     	max_sum=max(ltor_sum[i-1]+rtol_max_sum,max_sum); 
				
		}		
		cout<<max_sum<<endl;			
	}
	return 0;
}
```


  [1]: http://poj.org/problem?id=2479
  [2]: http://i2.buimg.com/4851/67651d5bd4218498.jpg
  [3]: http://i1.piimg.com/4851/db346ff2f2cb9b3b.jpg