---
title: "SCC (Strongly Connected Component) Algorithm"
date: 2024-07-04 00:13:00 +09:00
categories: [Problem Solving, Algorithm]
tags:
  - scc
  - tarjan's algorithm
  - kosaraju's algorithm
  - scc algorithm
  - algorithm
  - ps
  - problem solving
---

> The SCC algorithm can be performed in O(N) time complexity, and there are two main methods to achieve this: Tarjan's algorithm and Kosaraju's algorithm.

> Referenced from 『Programming Contest Challenge Book』
> 
> <img src="https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1328003271i/13446808.jpg"/>
> 
{: .prompt-info }

## 1. Description
In a directed graph, a subset of vertices S satisfies the condition that "for any two vertices u and v in S, there is a path from u to v". Such subset S is referred to as a strongly connected component (SCC). An SCC is a subset of vertices to which no other vertex subset can be added while maintaining strong connectivity. Any directed graph can be decomposed into a union of several SCCs, each of which has no intersection with others. After this process, a Directed Acyclic Graph (DAG) is formed. The algorithm that transforms a directed graph into a DAG composed of SCCs is commonly known as the SCC algorithm.

## 2. Tarjan's Algorithm

Let's suppose n is the number of vertices and for a given vertex number v, the contents of the vector E[v] represent the vertex numbers that can be reached from v.

```c++
#include <bits/stdc++.h>
using namespace std;
int dfs(int v, int n, vector<vector<int>>& E, vector<bool> &finished, vector<int> &d, int &cnt, stack<int> &st, vector<vector<int>> &ans){
  int p;
  p=d[v]= ++cnt;
  st.push(v);
  for(auto to:E[v]){
    if(d[to]==0){
      p=min(p,dfs(to, n, E, finished, d, cnt, st, ans));
    } else if(!finished[to]){
      p=min(p,d[to]);
    }
  }
  
  if(p==d[v]){
    vector<int> scc;
    while(true){
      int t=st.top();st.pop();
      finished[t]=true;
      scc.push_back(t);

      if(t==v) break;
    }

    ans.push_back(scc);
  }

  return p;
}

vector<vector<int>> tarjan(int n,vector<vector<int>> &E){
  vector<bool> finished(n+1);
  vector<int> d(n+1);
  vector<vector<int>> ans;
  stack<int> st;
  int cnt=0;

  for(int i=1;i<=n;i++){
    if(!finished[i]) dfs(i, n, E, finished, d, cnt, st, ans);
  }

  return ans;
}
```

## 3. Kosaraju's Algorithm

To be written...