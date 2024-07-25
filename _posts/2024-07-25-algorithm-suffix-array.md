---
title: "Suffix Array and LCP Array"
date: 2024-07-25 00:01:29 +09:00
categories: [Problem Solving, Algorithm]
tags:
  - string
  - suffix array
  - lcp array
  - suffix
  - lcp
  - ps
  - problem solving
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<div markdown="1">

## 1. Description

### Suffix Array
A suffix array for a string $$s$$ is an array of integers representing the starting positions of all suffixes of $$s$$ in lexicographical order. More formally, if $$s$$ is a string of length $$n$$, $$\text{suffixArray}[i]$$ indicates the starting index of the i-th smallest suffix of $$s$$ when sorted lexicographically.

For example, if $$s$$ = "$$banana$$", the suffix array would be an array containing the starting positions of the suffixes "$$a$$", "$$ana$$", "$$anana$$", "$$banana$$", "$$na$$", and "$$nana$$", sorted in lexicographical order.

In this case, its suffix array is $$[5, 3, 1, 0, 4, 2]$$.


### LCP Array
**The Longest Common Prefix (LCP)** array for a string s is an array that stores the lengths of the longest common prefixes between consecutive suffixes in the suffix array. Specifically, if suffixArray is the suffix array of s, then the LCP array LCP is defined such that $$\text{LCP}[i]$$ is the length of the longest common prefix between the suffixes starting at $$\text{suffixArray}[i]$$ and $$\text{suffixArray}[i-1]$$.

For instance, if $$s$$ = "$$banana$$", the suffix array will represent the suffixes "$$a$$", "$$ana$$", "$$anana$$", "$$banana$$", "$$na$$", and "$$nana$$", then the LCP array will provide the lengths of the common prefixes between adjacent suffixes in this order.

In this case, its LCP array is $$[null, 1, 3, 0, 0, 2]$$
## 2. Code

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
    return group[indexA+t]<group[indexB+t]; // Since group[indexA] == group[indexB], there is at least an overlap of t characters from the start.
    // Thus, the lengths of s[indexA..] and s[indexB..] are at least t. In other words, indexA + t <= n and indexB + t <= n.
    // To handle the case where the index is n, set group[n] = -1.
  }
};

void countingSort(vector<int> &suffixArray, vector<int> &group, int t){
  int n=suffixArray.size();
  int sum,maxi=max(MAX_VALUE_TO_HANDLE_ASCII,n+1); // group can range from 0 to n. Consider the array bounds carefully.
  vector<int> c(maxi);
  for(int i=0;i<n;i++) c[i+t<n?group[i+t]:0]++;
  for(int i=1;i<maxi;i++) c[i]+=c[i-1];

  vector<int> temp(n);
  for(int i=n-1;i>=0;i--) temp[--c[suffixArray[i]+t<n?group[suffixArray[i]+t]:0]]=suffixArray[i];
  suffixArray=temp;
}

vector<int> getSuffixArray(const string &s){
  int n = s.size(), t=1;
  vector<int> group(n+1); // group[i] = the group number that s[i...] belongs to when sorted by the first t characters.
  vector<int> suffixArray(n); // suffixArray[0] = i means s[i..] is the lexicographically smallest.
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
    group=newGroup;
  }

  return suffixArray;
}

vector<int> getLcpArray(string s, vector<int> &suffixArray){
  int n=s.size();
  vector<int> iSA(n);
  vector<int> LCP(n);

  for(int i=0;i<n;i++) iSA[suffixArray[i]]=i;
  for(int i=0,L=0;i<n;i++){
    if(iSA[i]==0) continue;
    while(i+L<n && suffixArray[iSA[i]-1]+L<n && s[i+L]==s[suffixArray[iSA[i]-1]+L]) L++;
    LCP[iSA[i]]=L;
    L=max(L-1,0);
  }

  return LCP;
}

int main(){
  string s;
  cin>>s;

  auto suffixArray=getSuffixArray(s);
  for(auto index:suffixArray)cout<<index+1<< " ";
  cout <<endl;

  auto lcpArray=getLcpArray(s, suffixArray);
  for(int i=0;i<lcpArray.size();i++)
    if(i==0) cout<<"x ";
    else cout<<lcpArray[i]<<" ";
}
```
> [You can test your code here](https://www.acmicpc.net/problem/9248){:target="_blank"}
{: .prompt-tip }

</div>
