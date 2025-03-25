---
title: "Suffix Array and LCP Array"
date: 2024-07-25 01:29:00 +09:00
categories: [Problem Solving, Algorithm]
post: skykhs3
math: true
tags:
  - suffix array
  - lcp array
  - algorithm
  - problem solving
  - Manber-Myers
  - Kasai's
---

<div markdown="1">
> Suffix Array and LCP Array are known as the Swiss Army knife in string-related problem solving.

## 1. Description

### Suffix Array
A suffix array for a string $s$ is an array of integers representing the starting positions of all suffixes of $s$ in lexicographical order. More formally, if $\text{s}$ is a string of length $\text{n}$, $\text{suffixArray}[i]$ indicates the starting index of the i-th smallest suffix of $s$ when sorted lexicographically.

For example, if $\text{s}$ = "$\text{banana}$", the suffix array would be an array containing the starting positions of the suffixes "$\text{a}$", "$\text{ana}$", "$\text{anana}$", "$\text{banana}$", "$\text{na}$", and "$\text{nana}$", sorted in lexicographical order.

In this case, its suffix array is $[5, 3, 1, 0, 4, 2]$.


### LCP Array
**The Longest Common Prefix (LCP)** array for a string s is an array that stores the lengths of the longest common prefixes between consecutive suffixes in the suffix array. Specifically, if suffixArray is the suffix array of s, then the LCP array LCP is defined such that $\text{LCP}[i]$ is the length of the longest common prefix between the suffixes starting at $\text{suffixArray}[i]$ and $\text{suffixArray}[i-1]$.

For instance, if $\text{s}$ = "$\text{banana}$", the suffix array will represent the suffixes "$\text{a}$", "$\text{ana}$", "$\text{anana}$", "$\text{banana}$", "$\text{na}$", and "$\text{nana}$", then the LCP array will provide the lengths of the common prefixes between adjacent suffixes in this order.

In this case, its LCP array is $[null, 1, 3, 0, 0, 2]$
## 2. Code
A Suffix Array can be obtained using the Manber-Myers algorithm. If we use quicksort, the time complexity is $O(Nlog^2N)$. If we use counting sort, the time complexity is $O(NlogN)$.

We can also obtain a LCP Array from a Suffix Array using Kasai's algorithm. It's time complexity is $O(N)$

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_VALUE_TO_HANDLE_ASCII = 300;

struct Comparator {
  const vector<int> &group;
  int t;
  Comparator(const vector<int> &_group, int _t): group(_group),t(_t) {};
  bool operator () (int indexA, int indexB){ // Allows this object to be used as a function
    if(group[indexA]!=group[indexB]) return group[indexA] < group[indexB];
    return group[indexA+t]<group[indexB+t];
    // Since group[indexA] == group[indexB], there is at least an overlap of t characters from the start.
    // Thus, the lengths of s[indexA..] and s[indexB..] are at least t.
    // In other words, indexA + t <= n and indexB + t <= n.
    // To handle the case where the index is n, set group[n] = -1.
  }
};

void countingSort(vector<int> &suffixArray, vector<int> &group, int t){
  int n=suffixArray.size();
  int maxVal=max(MAX_VALUE_TO_HANDLE_ASCII,n+1);
  // group can range from 0 to n. Consider the array bounds carefully.
  vector<int> c(maxVal);
  vector<int> idx(n);

  for(int i=0;i<n;i++)  idx[i]=suffixArray[i]+t<n ? group[suffixArray[i]+t] : 0;
  for(int i=0;i<n;i++) c[idx[i]]++;
  for(int i=1;i<maxVal;i++) c[i]+=c[i-1];

  vector<int> temp(n);
  for(int i=n-1;i>=0;i--){
    temp[--c[idx[i]]]=suffixArray[i];
  }

  swap(suffixArray, temp);
}

vector<int> getSuffixArray(const string &s){
  int n = s.size(), t=1;
  vector<int> group(n+1);
  // group[i] = the group number that s[i...] belongs to when sorted by the first t characters.
  vector<int> suffixArray(n);
  // suffixArray[0] = i means s[i..] is the lexicographically smallest.
  for(int i=0;i<n;i++) group[i]=s[i], suffixArray[i]=i;
  group[n]=0;
  while(t<n){
    Comparator compareUsing2T(group,t);
    // Choose between O(NlogN) quick sort and O(N) counting sort.
   // sort(suffixArray.begin(),suffixArray.end(),compareUsing2T);
    countingSort(suffixArray,group,t); countingSort(suffixArray,group,0);

    t*=2;
    if(t>=n) break;

    vector<int> newGroup(n+1);
    newGroup[n]=0;
    newGroup[suffixArray[0]]=1;

    for(int i=1;i<n;i++)
      if(compareUsing2T(suffixArray[i-1],suffixArray[i]))
        newGroup[suffixArray[i]]=newGroup[suffixArray[i-1]]+1;
      else
        newGroup[suffixArray[i]]=newGroup[suffixArray[i-1]];

    swap(group, newGroup);
  }

  return suffixArray;
}

vector<int> getLcpArray(string s, vector<int> &suffixArray){
  int n=s.size();
  vector<int> iSA(n), LCP(n);

  for(int i=0;i<n;i++) iSA[suffixArray[i]]=i;
  for(int i=0,L=0;i<n;i++){
    if(iSA[i]==0) continue;
    while(i+L<n && suffixArray[iSA[i]-1]+L<n
      && s[i+L]==s[suffixArray[iSA[i]-1]+L]) L++;
    LCP[iSA[i]]=L;
    L=max(L-1,0);
  }

  return LCP;
}

int main(){
  string s;
  cin>>s;

  auto suffixArray=getSuffixArray(s);
  for(auto index:suffixArray) cout<<index<<" ";
  cout<<endl;

  auto lcpArray=getLcpArray(s, suffixArray);
  for(int i=0;i<lcpArray.size();i++)
    if(i==0) cout<<"x ";
    else cout<<lcpArray[i]<<" ";
}
```

## 3. Example

String: "**banana**"

| i   | Suffix | Suffix Array | iSA | LCP Array |
| --- | ------ | ------------ | --- |
| 0   | a      | 5            | 3   | null      |
| 1   | ana    | 3            | 2   | 1         |
| 2   | anana  | 1            | 5   | 3         |
| 3   | banana | 0            | 1   | 0         |
| 4   | na     | 4            | 4   | 0         |
| 5   | nana   | 2            | 0   | 2         |


**Ordered by iSA**

| iSA | Suffix | LCP Array |
| --- | ------ | --------- |
| 0   | banana | 0         |
| 1   | anana  | 3         |
| 2   | nana   | 2         |
| 3   | ana    | 1         |
| 4   | na     | 0         |
| 5   | a      | null      |


> [You can test your code here](https://www.acmicpc.net/problem/9248){:target="_blank"}
{: .prompt-tip }

</div>
