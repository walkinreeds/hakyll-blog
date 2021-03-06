---
title: No Left Turns解题报告
tags:
  - ACM
  - 解题报告
id: 79
categories:
  - 陈年旧事
date: 2010-02-10 23:00:02
---

**写于2007-08-26 19:11**

**题目大意：**

在一个n*m的地图上，标记为灰色的是不可走的，标记为白色的是可走的，标记为Ｓ的是起始点，标记为Ｆ的是目的地，而已在地图上只能向前走或向右走，求从Ｓ走到Ｆ的最少步数。

**输入：**

第一行输入t，说明下面有多少组用例

每组用例的第一行输入n和m，n为地图行数，m为地图列数，下面n行输入一个n*m的地图，Ｘ为墙，空格为空地

**输出：**

对每组数据，输出从Ｓ走到Ｆ的最少步数

<!--more-->

**解法类型：**最短路径

**解题思路：**

由于只有两个方向可走，因而不能用普通的求最短路径方法，即用广度优先搜索和标记来遍历地图，如果只搜索不标记的话，或许能求出解来，但时间复杂度太高了。这里用一个数组step[MAX][MAX][4]来保存地图上每一个点的状态，用Dijkstra算法来求最短路径。

先定义四个方向：　０：北　１：东　２：南　３：西

```
Dr[4]={-1,0,1,0}，Dc[4]={0,1,0,-1}

struct{

	　　　　int r;

	　　　　int c;

	　　　　int dir;

	　　}que[MAX]：保存更新过的点的队列

	　　Step[MAX][MAX][4]：保存在地图上从起始点到达点(x,y)的方向为x时的最少步数

	　　Front：队列当前位置

	　　Rear：队列的末尾
```

１．初始化

将Step数组的所有值初始化为无穷大

将S点的四个方向入队，Step[sr][sc][]=0，即可以从Ｓ点沿着四个方向出发

Front=0,Rear=3;

２．更新访问到的点的Step值

用了一个队列que[MAX]来更新Step。假如用优先队列的话，每次读取出来的都是最优解，以后就再不用对该点进行更新，能大大提高效率，由于数据量不大，就没用优先队列，每次有点更新就加进队列中。

每次读取que[front]，r，c，dir分别为当前点的行，列，方向，由定义的方向可知，由当前点可走的方向newdir只能是dir或(dir+1)%4，所以newr=r+Dr[newdir],newc=c+Dc[newdir]，这样就可以确定后继点，如果Step[r][c][dir] +1&lt; Step[newr][newc][newdir],就更新Step[newr][newc][newdir]为Step[r][c][dir]+1,再把新点和方向加入队列中，直到front&gt;rear时停止更新，此时图中的每个点都已经更新为最少步数。

３．找出从Ｓ到Ｆ的最少步数

对Step[fr][fc][]四个方向循环一遍，找出最小值。
