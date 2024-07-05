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
In a directed graph, a subset of vertices S satisfies the condition that "for any two vertices u and v in S, there is a path from u to v". Such subset S is referred to as a strongly connected component (SCC). An SCC is a subset of vertices to which no other vertex subset can be added while maintaining strong connectivity. Any directed graph can be decomposed into a union of several SCCs, each of which has no intersection with others. After this process, a Directed Acyclic Graph (DAG) is formed. The algorithm that transforms a directed graph into a DAG composed of SCCs is commonly known as the SCC algorithm( O(N) ).

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e1/Scc-1.svg/220px-Scc-1.svg.png">

  * In the graph above, vertices a, b, and e form an SCC. Vertices f and g form another SCC, and vertices c, d, and h form yet another SCC.

## 2. Tarjan's Algorithm

Let's suppose n is the number of vertices and for a given vertex number v, the contents of the vector E[v] represent the vertex numbers that can be reached from v.

```c++
#include <bits/stdc++.h>
using namespace std;
int dfs(int v, int n, vector<vector<int>>& E, vector<bool> &finished, vector<int> &d, int &cnt, stack<int> &stck, vector<vector<int>> &ans){
  int p; // Stores the minimum value of d among the reachable child vertices (including itself) that are not in SCC.
  p = d[v] = ++cnt;
  stck.push(v);
  for (auto to : E[v]) {
    // Explore vertices that have not been visited yet.
    if (d[to] == 0) p = min(p, dfs(to, n, E, finished, d, cnt, stck, ans));
    // The vertex has already been explored through another path, and we already know its d value.
    else if (!finished[to]) p = min(p, d[to]);
  }

  if (p == d[v]) {
    // If the minimum value of d among the reachable child vertices (including itself) is equal to my d, then I am the start of an SCC.
    vector<int> scc;
    while(true){
      int t=stck.top();stck.pop();
      finished[t]=true;
      scc.push_back(t);

      if(t==v) break;
    }
    ans.push_back(scc);
  }
  return p;
}

vector<vector<int>> tarjan(int n,vector<vector<int>> &E){
  vector<bool> finished(n+1); // Records whether the vertex has been included in an SCC
  vector<int> d(n+1); // Stores the order of visits
  vector<vector<int>> ans; // SCC output
  stack<int> stck; // Stores vertices in the order they were visited
  int cnt=0; // A variable for ordering

  for(int i=1;i<=n;i++)
    if(!finished[i]) dfs(i, n, E, finished, d, cnt, stck, ans);

  return ans;
}
```

## 3. Kosaraju's Algorithm
Let's suppose the same input situation as Tarjan's algorithm.

```c++
#include <bits/stdc++.h>
using namespace std;
void createReversedEdge(int n,vector<vector<int>> &rE,vector<vector<int>> &E){
  for(int i=1;i<=n;i++)
    for(auto to:E[i])
      rE[to].push_back(i);
}

void dfs(int v,int n,vector<vector<int>> &E, vector<bool> &visited, vector<int> &stck){
  visited[v]=true;
  for(auto to:E[v])
    if(!visited[to]) dfs(to, n, E, visited, stk);
  stck.push_back(v);
}

void rdfs(int v,int n,vector<vector<int>> &rE, vector<bool> &visited, vector<int> &scc){
  visited[v]=true;
  scc.push_back(v);
  for(auto to:rE[v])
    if(!visited[to]) rdfs(to, n, rE, visited, scc);
}

vector<vector<int>> kosaraju(int n,vector<vector<int>> E){
  vector<bool> visited(n+1); // Records whether the vertex has been included in an SCC
  vector<int> stck; // A variable to record post-order travelsal
  vector<vector<int>> ans; // SCC output
  vector<vector<int>> rE(n+1); // Graph with reversed edge directions

  for(int i=1;i<=n;i++)
    if(!visited[i]) dfs(i,n,E,visited,stck);
  
  createReversedEdge(n,rE,E);

  fill(visited.begin(),visited.end(),false);

  for(int i=stck.size()-1;i>=0;i--){
    vector<int> scc;
    if(!visited[stck[i]]){
      rdfs(stck[i],n,rE,visited,scc);
      ans.push_back(scc);
    }
  }

  return ans;
}
```

## 4. Difference between Tarjan's and Kosaraju's
Tarjan's uses one DFS and Kosaraju's uses two DFS, but Tarjan's is more difficult to understand than Kosaraju's. You can use either one according to your preference.