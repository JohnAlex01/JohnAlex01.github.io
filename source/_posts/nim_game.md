---
title: LeetCode-Nim Game解题思路
date: 2016-9-19 17:30:00
tags: leetcode
grammar_cjkRuby: true
comments: true
categories:
-  CS
---

这是一道简单的推理�?![enter description here][1]

<!-- more -->
### 题目描述

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

注解�?      你和你的朋友玩下面的NIM游戏：在桌上的一堆石头，每一次你轮流取出1�?块石头。移除最后一块石头的人将是赢家。你将首先出手取出石头�?你们两个都很聪明都有最佳的游戏策略。编写一个函数，确定你是否可以赢得游戏中给定的一些石头堆中的数量�?例如，如果有4块石头，那么你永远不会赢得游戏：无论�?�?，或3个石头你删除，最后一块石头将永远被你的朋友取走�?
### 解题思路
1. 若石头数�?--3个，无论如何都会取胜�?2. 若石头数�?，第一轮无论取多少个石头（1--3个），最后一块石头都将是对方取走，我们暂且将4称为**死亡数字**�?3. 若石头数�?--7个，你可以取1--3块石头，�?*死亡数字**�?块石头）留给对方，这样，无论如何你将取胜�?4. 若石头数�?，无论取1块，2块，还是3块石头，所剩的石头仍处�?--7块之间，对方掌握主动权，可将**死亡数字**�?块石头）留给你�?5. 以此类推，若石头数为4的整数倍， 则你将失败�?6. 所以，若n%4==0，游戏失败；反之则胜�?

### 代码实现
```cpp
class Solution {
public:
    bool canWinNim(int n) {
        return n%4;
    }
};
```


  [1]: http://i1.piimg.com/4851/b42836db996f7dd9.jpg