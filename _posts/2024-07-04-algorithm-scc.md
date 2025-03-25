---
title: "SCC (Strongly Connected Component) Algorithm"
date: 2024-07-04 13:00:00 +09:00
categories: [Problem Solving, Algorithm]
post: skykhs3
tags:
  - scc
  - tarjan's algorithm
  - kosaraju's algorithm
  - scc algorithm
  - algorithm
  - ps
  - problem solving
---

<div markdown="1">
> The SCC algorithm can be performed in O(N+M) time complexity, and there are two main methods to achieve this: Tarjan's algorithm and Kosaraju's algorithm.

> Referenced from 『Programming Contest Challenge Book』
> 
> <img src="https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1328003271i/13446808.jpg" alt="Programming Contest Challenge Book"/>
> 
{: .prompt-info }

## 1. Description
In a directed graph, a subset of vertices S is called a Strongly Connected Component (SCC) if every pair of vertices u and v in S has a path from u to v and from v to u. Essentially, all vertices within an SCC are mutually reachable.

An SCC is maximal in the sense that no additional vertices can be added to it without breaking the property of strong connectivity. This means that there is no other subset of vertices in the graph that could be combined with the SCC to form a larger SCC.

Every directed graph can be decomposed into a union of SCCs, where each SCC is disjoint from the others. After this decomposition, the remaining graph becomes a Directed Acyclic Graph (DAG), where SCCs are represented as individual nodes.

The algorithm used to identify and construct SCCs in a directed graph is often referred to as the SCC algorithm. One efficient algorithm for this task operates in O(N+M) time complexity, where N is the number of vertices in the graph and M is the number of edges in the graph.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e1/Scc-1.svg/220px-Scc-1.svg.png" alt="SCC"/>

  * In the graph above, vertices a, b, and e form an SCC. Vertices f and g form another SCC, and vertices c, d, and h form yet another SCC.


---

## 2. Tarjan's Algorithm

Let's suppose n is the number of vertices (The indices of the graph start from 1) and for a given vertex number v, the contents of the vector G[v] represent the vertex numbers that can be reached from v.

```c++
#include <bits/stdc++.h>
using namespace std;
int dfs(int v, int n, int &cnt, vector<vector<int>> &G, vector<int> &d, vector<bool> &finished, stack<int> &S, vector<vector<int>> &ans){
  int p; // Stores the minimum value of d among the reachable child vertices (including itself) that are not in SCC.
  p = d[v] = ++cnt;
  S.push(v);
  for (auto to : G[v]) {
    if (d[to] == 0) p = min(p, dfs(to, n, cnt, G, d, finished, S, ans)); // Explore vertices that have not been visited yet.
    else if (!finished[to]) p = min(p, d[to]); // The vertex has already been explored through another path, and we already know its d value.
  }

  if (p == d[v]) { // If the minimum value of d among the reachable child vertices (including itself) is equal to my d, then I am the start of an SCC.
    vector<int> scc;
    while(true){
      int t=S.top(); S.pop();
      finished[t]=true;
      scc.push_back(t);
      if(t==v) break;
    }
    ans.push_back(scc);
  }
  return p;
}

vector<vector<int>> tarjan(int n, vector<vector<int>> &G){
  vector<bool> finished(n+1); // Records whether the vertex has been included in an SCC
  vector<int> d(n+1); // Stores the order of visits
  vector<vector<int>> ans; // SCC output
  stack<int> S; // Stores vertices in the order they were visited
  int cnt=0; // A variable for ordering

  for(int i=1;i<=n;i++)
    if(d[i]==0) dfs(i, n, G, finished, d, cnt, S, ans);

  return ans;
}
```

---

## 3. Kosaraju's Algorithm
Let's suppose the same input situation as Tarjan's algorithm.

```c++
#include <bits/stdc++.h>
using namespace std;
void createReversedEdge(int n, vector<vector<int>> &rG, vector<vector<int>> &G){
  for(int i=1;i<=n;i++)
    for(auto to:G[i])
      rG[to].push_back(i);
}

void dfs(int v, vector<vector<int>> &G, vector<bool> &visited, vector<int> &vs){
  visited[v]=true;
  for(auto to:G[v])
    if(!visited[to]) dfs(to, G, visited, vs);
  vs.push_back(v);
}

void rdfs(int v, vector<vector<int>> &rG, vector<bool> &visited, vector<int> &scc){
  visited[v]=true;
  scc.push_back(v);
  for(auto to:rG[v])
    if(!visited[to]) rdfs(to, rG, visited, scc);
}

vector<vector<int>> kosaraju(int n, vector<vector<int>> &G){
  vector<bool> visited(n+1); // Records whether the vertex has been included in an SCC
  vector<int> vs; // A variable to record post-order travelsal
  vector<vector<int>> ans; // SCC output
  vector<vector<int>> rG(n+1); // Graph with reversed edge directions
  createReversedEdge(n,rG,G);

  for(int i=1;i<=n;i++)
    if(!visited[i]) dfs(i,G,visited,vs);
  
  fill(visited.begin(),visited.end(),false);
  for(int i=vs.size()-1;i>=0;i--){
    vector<int> scc;
    if(!visited[vs[i]]){
      rdfs(vs[i],rG,visited,scc);
      ans.push_back(scc);
    }
  }

  return ans;
}
```
---

## 4. Difference between Tarjan's and Kosaraju's
Tarjan's uses one DFS and Kosaraju's uses two DFS, but Tarjan's is more difficult to understand than Kosaraju's. You can use either one according to your preference.
</div>