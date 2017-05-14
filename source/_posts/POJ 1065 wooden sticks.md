---
title: POJ 1065 Wooden Sticks
date: 2016-9-26 20:12:00
tags: 
- dp
- greedy
- poj
grammar_cjkRuby: true
catagories:
- CS
comments: true
---

一道既能用动态规�?Dynamic Programming)，也能用贪心(Greedy)求解的问题�?![enter description here][1]
<!---more--->


### 题目描述
* 原题地址:	[POJ 1065][2]

---
### 解题过程
#### 方法一（动态规划）
##### 解题思路
   在这个问题中，我们可以把它简化为一个LIS（最长增长子序列）类型的一个问题。因为可以证�?*不下降序列完全覆盖数就是最长下降子列的长度**，所以有多少个不下降序列就有多长的递减子序列，即我们也将其按照l(第一优先�?,w（第二优先级）的顺序从小到大排列，再找出这个序列的一个最长减小子序列即可。而LIS是动态规划的一个典型问题，因为它有着动态规划的两个典型特征�?1.	最优子结构�?一个问题的最优解由它子问题的最优解来决定，即数列a1,a2…an的LIS由a1,a2…a（n-1）的LIS决定�?2.	重叠子问�?在求解数列an的LIS问题中，其子问题会被反复求解。故可用动态规划的记忆化搜索特性来降低求解的时间空间复杂度�?
---
![][3]
##### 代码实现
```cpp
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef struct s{
	int l;
	int w;	
}s;
bool cmp(const s &a,const s &b){
		return a.l<=b.l;
}
s wood[5001];
int dp[5001];
int main(){
	int N,T;
	cin>>T;
	while(T--){
	memset(dp,0,sizeof(dp));
	cin>>N;
	for(int i=1;i<=N;i++)
		cin>>wood[i].l>>wood[i].w;
	sort(wood+1,wood+N+1,cmp);
	int maxlen=0;;
	for(int i=1;i<=N;i++){
		dp[i]=1;
		for(int j=1;j<i;j++)
		if(wood[i].w<wood[j].w&&dp[i]<dp[j]+1){
			dp[i]=dp[j]+1;
		}		
		maxlen=max(maxlen,dp[i]);
	}
	cout<<maxlen<<endl;
    }
} 
```
---

#### 方法二（贪心�?
##### 解题思路
找到若干个单调递增子序列，我们需将其按照l(第一优先�?,w（第二优先级）的顺序从小到大排列。接着从第一个（最小的）木棍依次向后寻找木棍，使这m条木棍（m<=n）严格地递增（即w<=w'&&l<=l'）。然后依次从未使用到的木棍里面继续寻找该种序列，重复若干次，直到所有的木棍均在若干个该种序列的集合中。在寻找该种序列的过程中，我们不顾及其他子问题的解，只是从原序列中找出严格大于当前木棍的木棍，这便是贪心算法的贪心选择性质�?
---
![][4]


##### 代码实现
```cpp
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef struct s{
	int l;
	int w;	
}s;
bool cmp(const s &a,const s &b){
		return a.l<=b.l;
}
s wood[5001];
int flag[5001];
int main(){
	int N,T;
	cin>>T;
	while(T--){
	memset(dp,0,sizeof(dp));
	cin>>N;
	for(int i=1;i<=N;i++)
		cin>>wood[i].l>>wood[i].w;
	sort(wood+1,wood+N+1,cmp);
	int cnt_seq=0;;
	for(size_t i=1;i<=N;++i){
	s last=wood[i];//last是一个wood，用来存储当前元素，以便和之后的元素作比�?	if(flag[i]==0){//flag是一个标记数组，记录该木棍是否被访问过。该if语句判断是否访问过该木棍，若未被访问，则新增一个递增序列�?	for(size_t j=i;j<=N;++j)
		if(last.w<=wood[j].w&&flag[j]==0){
		last=wood[j];
		flag[j]=1;//如果符合递增的要求，即把这个木棍放到当前的序列中，并标记为已访问
		}
		++ cnt_seq;//序列数加一，即所需时间加一
		  }
		}
	cout<< cnt_seq<<endl;
   }
} 
```

---
### 分析总结
该题拥有贪心以及动态规划两种解决方案。在看题之后，给人的第一感觉实用动态规划求解，然而寻找状态的转移方程并不是很容易。主要是没有看出这个问题是LIS的变种，而这一点也是我参考别人的博客才理解。贪心算法则是我在寻找子问题时悟出的另一种解�?将木棍以递增的序列排序，然后将这一序列分为几堆(按照l<=l�?&w<=w’的规则)，则堆数就是所需要的时间。动态规划也是这个思想，但在分堆的时候应用了一个更简单的解决方案:即我们从排好序的总序列中找到一个严格递减的子序列，那么子序列所含的元素数便是贪心算法中的堆数，即是我们所要花费的时间�?此题我们可以看出，虽然动态规划的适用性更广，但其变化性强，在寻找最优子问题和状态转移方程时容易遇到困难，而这还需要多加练习才能解决。而贪心算法虽然易于想到，但其却有一定的适用范围。以我做题的经验来看，往往能用贪心算法解决的问题，也能用动态规划解决；反之未然�?

  [1]: http://www.feizl.com/upload2007/2015_05/15050910458129.jpg
  [2]: http://poj.org/problem?id=1065
  [3]: http://p1.bqimg.com/4851/9c691041bb6ad8cc.jpg
  [4]: http://i1.piimg.com/4851/e7292cd52613166e.jpg