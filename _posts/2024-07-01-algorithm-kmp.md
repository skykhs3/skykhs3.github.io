---
title: "KMP(Knuth-Morris-Pratt) Algorithm"
date: 2024-07-01 00:13:00 +09:00
categories: [Problem Solving, Algorithm]
tags:
  [
    kmp algorithm,
    string search,
    ps,
    problem solving,
    algorithm techiques,
    knuth,
    morris,
    pratt,
    algorithm,
  ]
---

> The KMP (Knuth-Morris-Pratt) algorithm is a fast string search algorithm.

> Referenced from 『프로그래밍 대회에서 배우는 알고리즘 문제 해결 전략 세트2』 p.657
> 
> <img src="https://image.yes24.com/Goods/8006522/XL">
{: .prompt-info }

## 1. Description

Let's suppose there are two strings, A and B.
The length of A is N and the length of B is M.
We can find out where B is included in A using the KMP algorithm.
The time complexity of this algorithm is O(N + M).

## 2. Code

```c++
vector<int> getPartialMatch(string &B){
  int M=(int)B.size(), matched=0;
  vector<int> pi(M, 0);

  for(int i=1;i<M;i++){
    while(matched>0 && B[matched]!=B[i]) matched=pi[matched-1];
    if(B[matched]==B[i]) pi[i]=++matched;
  }
  return pi;
}

void kmp(string &A, string &B){
  int matched=0;
  int N = (int)A.size(), M = (int)B.size();
  vector<int> pi=getPartialMatch(B);
  vector<int> whereBIsIncluded;
  
  for(int i=0;i<N;i++){
    while(matched>0 && A[i]!=B[matched])
      matched=pi[matched-1];
    if(A[i]==B[matched]){
      ++matched;
      if(matched==M){
        whereBIsIncluded.push_back(i-matched+1);
        matched=pi[matched-1];
      }
    }
  }
}
```
