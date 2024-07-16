---
title: "Maximum Bipartite Matching"
date: 2024-07-14 00:22:00 +09:00
categories: [Problem Solving, Algorithm]
tags:
  - bipartite graph
  - maximum bipartite matching
  - ps
  - problem solving
  - algorithm
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

> Maximum Bipartite Matching is well-known algorithm in algorithm contests.

<div markdown="1">

## 1. Bipartite Graph 
If you can paint all vertices of a graph in two colors such that adjacent vertices have different colors, the graph is called a bipartite graph.

## 2. Maximum Bipartite Matching
Maximum Bipartite Matching is an algorithm used to find the maximum matching in a bipartite graph.

So, what is maximum matching?

A matching in a graph is a set of edges such that no two edges share a common vertex. In other words, it is a way of pairing up vertices such that each vertex is connected to exactly one other vertex via the edges in the matching.

In a bipartite graph, the goal of the maximum bipartite matching algorithm is to find the largest possible matching, where the number of paired vertices is maximized. This means identifying the maximum number of edges that can be included in the matching without any two edges overlapping on a same vertex.

The algorithm is particularly useful in various practical applications, such as job assignments, where tasks need to be assigned to workers in a way that maximizes efficiency while ensuring no worker is assigned more than one task.

<img src="/assets/img/posts/2024-07-14-algorithm-maximum-bipartite-matching/sample_graph.jpeg" alt="maximum bipartite matching"/>

For example, the maximum bipartite matching is 4 in the above picture. Of course, another way of matching the edges can exist. You can match vertex 3 and vertex 4 instead of matching vertex 2 and vertex 4.

## 3. Algorithm Explanation

To be written...

## 4. Code
- If there exists an edge connecting vertex 1 in group A and vertex 2 in group B and an edge connecting vertex 1 in group A and vertex 3 in group B, we indicate this as 𝑉[1]={2,3}
- The assign array indicates where a vertex in group B connects to in group A.
- The visit array tracks the visiting history.
- The time complexity is 𝑂(𝑁𝐸)(where 𝑁 is the number of vertices and 𝐸 is the number of edges).

```c++
bool dfs(int v,vector<vector<int>> & V,vector<int> &visit, vector<int> &assign){
    for(auto to:V[v]){
        if(visit[to]!=0) continue;
        visit[to]=1;
        if(assign[to]==0 || dfs(assign[to],V,visit,assign)){ 
            assign[to]=v;
            return true;
        }
    }
    return false;
}

int maximumBipartiteMatching(int n,vector<vector<int>> & V){
    vector<int> visit(N+10,0);
    vector<int> assign(N+10,0);
    int ans=0;
    for(int i=1; i<=n;i++){
        for(int j=1;j<=n;j++) visit[j]=0; 
        if(dfs(i,V,visit,assign)) ans++;
    }
    
    return ans;
}
```
</div>
